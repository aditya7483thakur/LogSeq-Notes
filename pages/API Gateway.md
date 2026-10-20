- 1) What is an API Gateway?
  collapsed:: true
	- Definition
	  collapsed:: true
		- A single entry point in front of one or more backend services. Clients call the gateway; it forwards each request to the right backend.
		- API Gateway = reverse proxy + API policies (routing, auth, rate limits, TLS termination, CORS, transforms, caching, logging).
		- Flow
		  ```
		  Client
		    |
		    v
		  API Gateway  (auth, rate limit, CORS, logging)
		    |
		    +--> /users  -> user-service
		    +--> /orders -> order-service
		  ```
	- Why it exists
	  collapsed:: true
		- Without gateway: every service implements auth, rate limiting, CORS and logging itself.
		- With gateway: configure them once at the edge; works for any backend in any language.
	- Compared to
	  collapsed:: true
		- Reverse proxy (plain nginx): only forwards traffic. A gateway is a reverse proxy with API features (consumers, keys, quotas, plugins).
		- Load balancer: spreads traffic across copies of ONE service. A gateway can load-balance too, but it also decides WHICH service gets the request and enforces policy.
	- NestJS analogy
	  collapsed:: true
		- Guards, interceptors and middleware, but running OUTSIDE your app, in front of all your services, configured instead of coded.
	- APISIX (example engine)
	  collapsed:: true
		- Built on NGINX + Lua.
		- Config is stored in etcd and changed through the Admin API.
		- Changes reach every node within milliseconds, no restart or reload.
- 2) How API Gateways are used
  collapsed:: true
	- Typical uses
		- One public domain for many microservices: `/users` -> user-service, `/orders` -> order-service.
		- Offload auth, rate limits and CORS from backends.
		- Keep backends private: only the gateway is public, backends sit on a private network.
		- API versioning (`/v1` -> old backend, `/v2` -> new backend) and canary / traffic splitting.
	- Deployment styles
		- Self-hosted: you run APISIX / Kong / Envoy yourself.
		- Managed: a cloud provider runs it for you (sometimes a dedicated gateway per customer).
	- Building blocks (APISIX)
	  ```
	  Route        -> which requests to catch (host + path + method)
	  Upstream     -> where to send them (backend servers + load balancing)
	  Service      -> reusable upstream + shared plugins for many routes
	  Plugin       -> what to do on the way (auth, limits, transforms...)
	  Global Rule  -> plugins that run on EVERY request, whatever the route
	  Consumer     -> who is calling (identified by auth plugins)   -> covered in a later note
	  SSL          -> certificates, chosen by SNI (domain)            -> covered in a later note
	  ```
	- Lifecycle of one request
	  ```
	  client -> DNS -> gateway public IP
	    |
	    +-- TLS handshake (certificate chosen by SNI = requested domain)
	    +-- route match (host + URI + method)      no match -> 404
	    +-- global rules (every request)
	    +-- route plugins (auth, CORS, transforms...)
	    +-- forward to upstream (backend) -> response back to client
	  ```
	- Not APISIX-only: same ideas, different names
	  
	  | Gateway | Matches requests | Backend |
	  | --- | --- | --- |
	  | APISIX | Route | Upstream (or Service = upstream + shared plugins) |
	  | Kong | Route | Gateway Service -> Upstream + Targets (load balancing) |
	  | Envoy | Listener -> Route configuration | Cluster |
	  | AWS API Gateway | Resource + Method (deployed to a Stage) | Integration |
		- Watch out: a Kong "Gateway Service" is the backend definition (like an APISIX upstream), NOT an APISIX service.
- 3) Routes
  collapsed:: true
	- Definition
		- A rule: "when a request looks like X (host + path + method), send it to backend Y, and apply plugins Z on the way."
	- NestJS analogy
	  ```
	  @Controller({ host: 'api.example.com' })   -> Host
	  @Get('/echo/*') / @Post('/echo/*')          -> URI + Methods
	  @UseGuards / @UseInterceptors               -> Plugins
	  handler that proxies to a backend           -> Upstream
	  ```
	- Worked example (route "echo-test")
	  ```
	  GET https://api.example.com/echo/hello
	   |
	   +-- Host = api.example.com?   yes
	   +-- URI matches /echo/* ?     yes  (* = anything after /echo/)
	   +-- Method GET or POST?       yes
	   +-- run route plugins (none)
	   +-- forward to upstream "echo-backend" (10.0.0.5:8080)
	  ```
		- Any check fails -> the gateway tries other routes. None match -> 404.
	- Matching fields
		- Name / description: labels for humans. The gateway uses an ID internally.
		- URI: the path to match. `*` at the end = anything after it (`/api/v1/*` matches `/api/v1/users` and `/api/v1/orders/5`). A route can have several URIs.
		- Host (optional): which domain (`Host` header) the route answers for.
			- How it works
				- Every request carries the domain in a `Host` header, even though the connection goes to an IP:
				  ```
				  1. DNS:  api.myshop.com -> 103.20.30.40   (browser finds the IP)
				  2. Browser connects to 103.20.30.40 and sends:
				       GET /login
				       Host: api.myshop.com   <-- the domain travels inside the request
				  ```
					- HTTP/2 and HTTP/3 send it as the `:authority` field. Same idea.
				- DNS is what points a domain at the gateway (set up outside the gateway). The Host field doesn't point anything anywhere.
				- Many domains can point at ONE gateway IP:
				  ```
				  api.myshop.com   --+
				  api.myblog.com   --+--DNS--> 103.20.30.40 (one gateway)
				  admin.myshop.com --+
				  ```
					- All three have a `/login`. The path alone can't tell them apart; Host can.
				- A route = IF (matching) + THEN (backend target)
				  ```
				  ROUTE  =  IF: host + path + method      THEN: backend target (where to send)
				  ```
					- Host is only a MATCHING condition: it picks WHICH route handles the request. It is NOT a destination. The chosen route's backend target (inline upstream / upstream / service) decides where the request goes.
				- If/else analogy
				  ```ts
				  if (host === 'api.myshop.com'   && path === '/login') return proxyTo('10.0.0.5:8080'); // shop-backend
				  if (host === 'api.myblog.com'   && path === '/login') return proxyTo('10.0.0.9:3000'); // blog-backend
				  if (host === 'admin.myshop.com' && path === '/login') return proxyTo('10.0.0.7:4000'); // admin-backend
				  ```
					- Host and path are inside the `if` (which route). The backend target is the `return` (where it goes).
				- The same three routes, configured
				  
				  | Route | Path | Host | Backend target |
				  | --- | --- | --- | --- |
				  | shop-login | /login | api.myshop.com | Inline: 10.0.0.5:8080 |
				  | blog-login | /login | api.myblog.com | Upstream "blog-backend" (2 nodes) |
				  | admin-login | /login | admin.myshop.com | Inline: 10.0.0.7:4000 |
				- One request, two steps
				  ```
				  GET /login   Host: api.myblog.com
				    |
				  STEP 1 - WHICH ROUTE? (host + path + method)
				    +-- shop-login:  path yes  host no   -> skip
				    +-- blog-login:  path yes  host yes  -> THIS ONE
				    |
				  STEP 2 - WHERE TO SEND? (blog-login's backend target)
				    +-- upstream "blog-backend"
				          +-- 10.0.0.9:3000    <-- picked by load balancing
				          +-- 10.0.0.10:3000
				  ```
					- Host is used only in step 1. The backend target does step 2.
				- NestJS analogy
				  ```ts
				  @Controller({ host: 'api.myblog.com' })  // Host -> IF this controller handles it
				  export class BlogController {
				    @Get('/login')                         // Path -> IF this method handles it
				    login() {
				      return this.http.get('http://10.0.0.9:3000/login');  // Backend target -> WHERE it goes
				    }
				  }
				  ```
				- Host, path and method are all differentiators. Method works the same way: `GET /login` -> route A and `POST /login` -> route B is valid.
			- Why: virtual hosting. One gateway IP serves many domains, and the same path can go to different backends:
			  ```
			  GET /v1/users  Host: api.shop.com  -> shop-backend
			  GET /v1/users  Host: api.blog.com  -> blog-backend
			  ```
			- No host -> matches ANY domain, including calls to the gateway's raw IP. Side effect: internet scanners hitting the IP reach your backend instead of a 404.
			- Host set -> only that domain matches. DNS must point at the gateway, and HTTPS needs a certificate for that domain in the gateway (picked by SNI) -> covered in a later note.
			- Wildcards work: `*.example.com`.
			- When to fill Host
			  
			  | Situation | Host field |
			  | --- | --- |
			  | One domain points at the gateway (most setups) | Leave empty; path routing is enough |
			  | Several domains share one gateway, different backends | Fill it, one domain per route |
			  | Route should answer ONLY on your domain, not on the raw IP | Fill it with your domain |
		- Methods: which HTTP verbs match. Only GET/POST allowed -> a DELETE won't match. Empty = all methods.
		- Priority: tie-breaker when two routes match the same request. Higher number wins (default 0).
			- Clash: two routes with the same path (and methods) and no Host on either both match the same requests. The higher priority always wins; the other route never gets traffic.
	- Pointing a route at a backend: 3 ways (APISIX)
		- Inline upstream
			- Backend (host:port + options) stored INSIDE the route.
			- Quick, not reusable. Changing the backend IP = editing every route that uses it.
		- Upstream object (`upstream_id`)
			- Separate reusable object: weighted nodes, load-balancing algorithm (roundrobin / chash / ewma / least_conn), scheme, timeouts, Host-header handling, optional health checks.
			- Many routes share it: change it once, every route follows.
			- -> see 4) Upstreams
		- Service object (`service_id`)
			- Upstream + shared plugins (auth, rate limit, CORS...) defined once for many routes.
			- Same plugin on route and service -> the ROUTE's config wins.
			- -> see 5) Services
		- Diagram
		  ```
		  Inline:    Route A [backend inside]     Route B [same backend copied inside]
		  
		  Upstream:  Route A --+
		             Route B --+--> Upstream "users" (nodes + load balancing)
		  
		  Service:   Route A --+
		             Route B --+--> Service "users-api" (auth, CORS plugins)
		                                  |
		                                  v
		                             Upstream "users"
		  ```
		- NestJS analogy
			- Inline = hardcoded URL in one handler.
			- Upstream = a shared HttpModule baseURL injected everywhere.
			- Service = a whole controller with its guards/interceptors and backend already set up; each route is a new method on it.
		- When to pick
		  ```
		  one quick backend / testing                        -> inline upstream
		  same backend for several routes, or >1 node        -> upstream object
		  many routes need same backend AND same plugins     -> service object
		  ```
		- APISIX won't delete an upstream or service that a route still uses.
		- Not APISIX-only: every gateway separates "match the request" from "where it goes" (see the table in 2). Only the names and nesting differ.
	- Plugins on a route (overview)
		- Plugins = the "do something on the way" part.
			- Authentication: key-auth, jwt-auth, basic-auth
			- Security: CORS, IP restriction, CSRF, request validation
			- Traffic control: rate limits, connection limits, caching, circuit breaker, fault injection
			- Transformations: proxy-rewrite, response-rewrite, redirect, mocking
		- Route plugins -> only that route. Global rules -> every request.
		- Managed platforms usually allowlist which plugins customers can use, and block plugin URLs pointing at internal addresses (SSRF protection).
		- Individual plugins, rate limiting and auth in depth -> covered in a later note.
	- How a route gets created (APISIX)
	  ```
	  client / dashboard
	    |
	    v
	  PUT /apisix/admin/routes/<id>   (Admin API)
	    |
	    v
	  stored in etcd -> every APISIX node picks it up within ms, no restart
	  ```
		- Example
		  ```bash
		  curl http://127.0.0.1:9180/apisix/admin/routes/1 \
		    -H "X-API-KEY: $ADMIN_KEY" -X PUT -d '
		  {
		    "name": "echo-test",
		    "uri": "/echo/*",
		    "host": "api.example.com",
		    "methods": ["GET", "POST"],
		    "upstream": { "type": "roundrobin", "nodes": { "10.0.0.5:8080": 1 } }
		  }'
		  ```
- 4) Upstreams
  collapsed:: true
	- Definition
		- A saved, named backend that routes point to. It holds one or more backend servers (nodes) and settings for how the gateway talks to them.
		- Route vs upstream: the ROUTE decides WHICH requests; the UPSTREAM decides WHICH SERVERS, HOW to reach them, and HOW to share the load.
		- Flow
		  ```
		  Route "echo-test" (/echo/*)
		    |
		    v
		  Upstream "echo-backend"
		    +-- scheme HTTP, Round Robin
		    +-- node 192.168.3.137:8080
		  ```
	- NestJS analogy
		- Like `HttpModule.register({ baseURL })`: one shared backend definition that many routes use.
		- Add a node -> every route using the upstream starts sending it traffic, with no route changes.
	- Private backends
		- Nodes can use private IPs (e.g. `192.168.3.137:8080`). The gateway reaches them over the private network, so the backend never needs to be public.
		- The backend's firewall / security group must allow the gateway on that port.
	- Fields
		- Name / Description
			- Labels for humans.
		- Scheme
			- The protocol for the gateway -> backend hop ONLY.
			  ```
			  client ==HTTPS==> gateway --HTTP--> backend
			                               ^
			                            "Scheme"
			  ```
			- | Option | Use when the backend speaks... |
			  | --- | --- |
			  | HTTP | plain HTTP (most internal services) |
			  | HTTPS | TLS (e.g. a public API like api.github.com:443) |
			  | gRPC / gRPCs | gRPC without / with TLS |
			- The client can use HTTPS to the gateway while the gateway uses plain HTTP to a private backend. This is called TLS termination.
		- LB Type (load balancing)
			- How requests are spread across nodes. Only matters with 2+ nodes.
			- | Option | Behaviour | Good for |
			  | --- | --- | --- |
			  | Round Robin | takes turns A, B, A, B (respects weights) | default, stateless APIs |
			  | Consistent Hash | same client -> same node (sticky) | backends keeping sessions / cache in memory |
			  | EWMA | picks the fastest-responding node | nodes with uneven speed |
			  | Least Connections | picks the node with the fewest open connections | long / slow requests (uploads, streaming) |
		- Target Nodes
			- The actual servers.
			- | Field | Meaning |
			  | --- | --- |
			  | Host | IP or domain of a backend server |
			  | Port | its port (80, 8080, 443...) |
			  | Weight | its SHARE of traffic compared to the other nodes |
			- Example
			  ```
			  upstream "user-api"  (Round Robin)
			    +-- 192.168.1.10:80  weight 3 -> ~75% of requests
			    +-- 192.168.1.11:80  weight 1 -> ~25%
			  ```
		- Timeout
			- How long the gateway WAITS for the backend. If the backend is slower, the gateway gives up and the client gets an error (even if the backend finishes later).
			  ```
			  client --> gateway --> backend ...working...
			             60s pass, no reply -> gateway gives up -> client gets an error
			  ```
			- NestJS analogy: `HttpModule.register({ timeout: 60000 })`.
			- | Endpoint | Backend takes | Timeout 60s | Timeout 120s |
			  | --- | --- | --- | --- |
			  | GET /users | 0.2s | ok | ok |
			  | GET /reports/yearly | 90s | error at 60s | ok |
			  | POST /upload (2 GB) | 10 min | error | error -> use a large value like 3600 |
			- Two kinds: connect timeout = how long to wait just to REACH the server; send / read timeout = how long to wait while sending the request and reading the reply.
			- Rule of thumb: keep the default for normal APIs; raise it only for known slow endpoints.
	- Host Header (the confusing one)
		- What a "host" is
			- Always a domain name or an IP, sometimes with a port. The same kind of value shows up in different places with different jobs:
			- | Where | Example | Job |
			  | --- | --- | --- |
			  | Client's Host header | shop.example.com | the domain the caller used in their URL |
			  | Route -> Host field | shop.example.com | FILTER: only match requests for this domain |
			  | Upstream -> Target Node host | 10.0.0.5 / api.github.com | WHERE to connect (server address) |
			  | Upstream -> Host Header option | node / pass / rewrite | WHICH NAME to write in the Host header sent to the backend |
		- Why the upstream needs a Host setting
			- Same idea as route Host, one hop later.
			  ```
			  HOP 1                                       HOP 2
			  client --Host: shop.example.com--> GATEWAY --Host: ???--> BACKEND
			                                     reads Host            reads Host
			                                     -> picks a ROUTE      -> picks one of ITS sites
			  ```
			- Every HTTP/1.1 request MUST carry a Host header. When the gateway forwards a request it becomes the client of the backend, so it has to write some Host. This option decides what.
			- Some backends host several sites on one IP and use Host to choose (exactly like the gateway does with routes):
			  ```
			  nginx at 10.0.0.5
			    +-- Host: orders.internal -> orders app
			    +-- Host: users.internal  -> users app
			  ```
			- A backend running only ONE app usually ignores Host, so any option works.
		- The 3 options (APISIX name `pass_host`)
			- | Option | Host sent to the backend | Comes from |
			  | --- | --- | --- |
			  | node (use the node's address) | the address of the node picked for THIS request | Target Nodes -> Host |
			  | pass (keep client's Host), APISIX DEFAULT | exactly what the client sent | the client's URL |
			  | rewrite (fixed host) | one fixed value you type (`upstream_host`) | a separate field on the upstream |
			- Example with real values
			  ```
			  Route:     Host = shop.example.com, URI = /orders/*
			  Upstream:  nodes 10.0.0.5:8080 and 10.0.0.6:8080, fixed host = orders.internal
			  curl https://shop.example.com/orders/1   -> load balancer picks 10.0.0.6
			  ```
			- | Option | Host the backend receives |
			  | --- | --- |
			  | node | 10.0.0.6:8080 |
			  | pass | shop.example.com |
			  | rewrite | orders.internal |
		- Key points
			- An upstream has NO hostname of its own; only nodes do. With "node" and several nodes, each request carries the address of whichever node it went to:
			  ```
			  request 1 -> 10.0.0.5 -> Host: 10.0.0.5:8080
			  request 2 -> 10.0.0.6 -> Host: 10.0.0.6:8080
			  ```
				- The port is added when it isn't the scheme's standard port (80 for HTTP, 443 for HTTPS).
			- rewrite = ONE fixed name for ALL nodes, whichever node is picked. Use it when the backend only answers to a specific name.
			- rewrite does NOT read the route's Host (one upstream can be used by many routes with different hosts). The fixed value lives on the upstream.
			- rewrite changes ONLY the Host header the backend sees:
			  ```
			  route matching            -> uses the client's ORIGINAL Host (happens first, not affected)
			  which node                -> still picked by the load balancer (not affected)
			  Host the backend receives -> replaced by the fixed value
			  ```
		- GitHub example: Host problem vs network problem
			- Upstream: node `api.github.com`, port 443, scheme HTTPS. Route `/zen` -> this upstream.
			  ```
			  node -> gateway --Host: api.github.com-----> GitHub   OK: "I host that" -> responds
			  pass -> gateway --Host: <gateway-domain>---> GitHub   FAIL: "never heard of it" -> error
			  ```
			- The packets reach GitHub fine in both cases. The difference is what GitHub does with the request.
			- Two different layers:
			- | Layer | Question | Checked by | Failure looks like |
			  | --- | --- | --- | --- |
			  | Network | can packets reach the server? | firewalls / security groups | timeout or refused: request NEVER reaches the server |
			  | Application | server got it; does it accept it? | the server's own logic (e.g. Host check) | server REPLIES with an error (4xx) |
			- A wrong Host header is an application-layer problem, not a firewall problem.
			- Rule of thumb: APISIX's default is pass. If the backend checks Host (like api.github.com), switch to node, or rewrite if it needs one specific name.
- 5) Services
  collapsed:: true
	- Definition
		- A Service = ONE upstream + a set of plugins, saved once and reused by many routes. Every route that points to it INHERITS both.
		- It is NOT a group of upstreams. It has one upstream plus shared plugins.
		- Flow
		  ```
		  service "user-api"
		    +-- upstream: user-backend               <-- where requests go
		    +-- plugins: jwt-auth, limit-req, cors   <-- what happens on the way
		  
		  route GET  /users/*   --+
		  route POST /users     --+--> service "user-api"  -> all get same backend + auth + rate limit + CORS
		  route GET  /users/me  --+
		  ```
	- NestJS analogy
		- Guards on the CONTROLLER CLASS apply to every method inside it.
		  ```ts
		  @UseGuards(JwtGuard)                // <- service plugins
		  @UseInterceptors(RateLimit, Cors)
		  @Controller('users')                // <- service (with its upstream)
		  export class UsersController {
		    @Get('*')  findAll() {}           // <- route 1 (inherits everything)
		    @Post()    create()  {}           // <- route 2
		    @Get('me') me()      {}           // <- route 3
		  }
		  ```
	- Why it exists
		- Without a service: auth, rate limit, CORS and upstream are configured separately on EVERY route. 10 routes = 10 copies, and changing the rate limit = 10 edits.
		- With a service: configure once. Changing the service updates every route using it instantly.
	- Fields
		- | Field | Meaning |
		  | --- | --- |
		  | Name / Description | labels for humans |
		  | Upstream | the backend every route using this service sends to (pick an existing upstream) |
		  | Plugins | same plugin choices as on a route; they apply to EVERY route using the service |
		  | WebSocket (`enable_websocket`) | allows long-lived two-way connections (chat, live dashboards) through the gateway |
	- How a route uses a service
		- In the route's backend choice, pick the service. The route stores only the service ID and gets the upstream + plugins from it.
		  ```
		  GET /users/me
		    +-- match route (path + method + host)
		    +-- route says: service = user-api
		    +-- run plugins: jwt-auth, limit-req, cors   (from the service)
		    +-- forward to upstream user-backend          (from the service)
		  ```
	- Route plugins + service plugins: how they combine
		- | Situation | Result |
		  | --- | --- |
		  | plugin only on the service | applies |
		  | plugin only on the route | applies too (added on top) |
		  | SAME plugin on both | the ROUTE's version wins |
		- Example: one route needs a different rate limit
		  ```
		  service "user-api":    jwt-auth, cors, limit-req = 100 req/s
		  route /users/search:   limit-req = 10 req/s  (expensive endpoint)
		  
		  /users/search -> jwt + cors (from service) + 10 req/s (route overrides)
		  other routes  -> jwt + cors + 100 req/s (from service)
		  ```
		- An override REPLACES the whole plugin config, not single fields:
		  ```
		  service limit-req: { rate: 100, burst: 50 }
		  route   limit-req: { rate: 10 }
		  -> route uses ONLY { rate: 10, ... its own values }. It does NOT keep the service's burst 50.
		  ```
			- So when overriding, fill in every field you care about.
		- A route can change or add plugins, but simply leaving a plugin out doesn't remove it (APISIX has a disable flag for that).
		- If one route must skip the service's auth entirely, the simplest option is to not attach that route to the service: point it at the upstream directly and give it only the plugins it needs.
	- The service's upstream is optional in APISIX: why
		- In APISIX a route can point to a service AND have its own upstream at the same time.
		- Where does a route send requests?
		  ```
		  1. route's OWN upstream?      -> yes -> use it (route wins over service)
		  2. no -> SERVICE's upstream?  -> yes -> use it
		           +-- no -> nowhere to send -> 503 "missing upstream configuration in Route or Service"
		  ```
		- So a service can be just a "bag of plugins" with no upstream, and each route brings its own backend:
		  ```
		  service "company-auth" = jwt-auth + cors   (NO upstream)
		  route /users/*   -> service company-auth + own upstream user-backend
		  route /orders/*  -> service company-auth + own upstream order-backend
		  ```
			- Same security everywhere, different backends.
		- APISIX also has a dedicated object for a pure "bag of plugins": Plugin Config (`plugin_config_id`).
			- | APISIX object | Contains | Use when |
			  | --- | --- | --- |
			  | Service | upstream (optional) + plugins | routes share the backend AND the plugins |
			  | Plugin Config | plugins only | routes share ONLY the plugins, each with its own backend |
		- Pitfall: if a dashboard lets a route pick only ONE backend option (service OR upstream, not both), then a service saved without an upstream breaks every route using it. The service and routes save fine, but every request returns 503. In that case, always give the service an upstream.
		- Exception: requests that a plugin answers BEFORE the backend step never need an upstream (failed auth -> 401, rate limited -> 429, redirect -> 301).
	- Route vs Upstream vs Service
		- | | Route | Upstream | Service |
		  | --- | --- | --- | --- |
		  | Answers | WHICH requests? | WHICH servers and how to reach them? | SHARED setup for many routes |
		  | Contains | path, host, methods, priority + a backend | nodes, weights, LB type, scheme, host header, timeout | ONE upstream + plugins |
		  | Used by | (entry point) | routes or services | routes |
	- When to use a service
	  ```
	  1 route                                     -> no service needed, set things on the route
	  several routes, same backend, no plugins    -> just share an upstream
	  several routes, same backend AND same
	  auth / rate limit / CORS                    -> service
	  ```
		- A service can't be deleted while routes still use it. Repoint or delete those routes first.
- 6) Consumers
  collapsed:: true
	- Definition
		- A consumer = "WHO is calling": one client of your API (a mobile app, a partner company, an internal service). Each consumer has a credential (API key, JWT secret, or username/password).
		- The Consumers section is the gateway's CREDENTIAL STORE. Auth plugins on routes check incoming requests against it.
		- Once a request is authenticated, the gateway knows WHO it is, so other plugins can use that identity (per-consumer rate limits, consumer-restriction).
	- NestJS analogy
	  ```
	  Consumer             = a row in your users table (username + password/key)
	  Auth plugin on route = @UseGuards(AuthGuard('api-key'))   <- checks credentials against those rows
	  ```
	- Both halves are needed
	  ```
	  Consumers:      mobile_app -> key: 3f9a...   --+
	                  partner_x  -> key: 81bc...   --+-- WHO is allowed (credentials)
	  
	  Route/Service:  key-auth plugin ON           ----- WHERE a credential is required
	  ```
		- Route WITHOUT an auth plugin = public; consumers are ignored.
		- Route WITH key-auth but no consumers = every request gets 401.
		- New routes are public by default. Each route needs the auth plugin enabled, or it can inherit it from a Service.
	- Request flow (key-auth)
	  ```
	  curl -H "apikey: 3f9a..." https://api.example.com/echo/hello
	    +-- route matches
	    +-- key-auth: which consumer has key 3f9a...?
	    |     found   -> mobile_app -> continue
	    |     missing -> 401 "Missing API key in request"
	    |     wrong   -> 401 "Invalid API key in request"
	    +-- forward to upstream
	  ```
	- Auth types
		- | Type | Credential | How the client sends it |
		  | --- | --- | --- |
		  | API Key (key-auth) | a random key | header `apikey: <key>` (header name configurable; optional query param fallback `?apikey=<key>`) |
		  | JWT (jwt-auth) | JWT key + secret (HS256) | the client SIGNS its own token with the secret (token contains a `key` claim = the consumer's JWT key) and sends `Authorization: Bearer <token>` |
		  | Basic Auth (basic-auth) | username + password | `curl -u user:password` -> `Authorization: Basic ...` |
		- key-auth option "Hide credentials from upstream": the gateway strips the key before forwarding, so the backend never sees it (recommended ON).
		- Many dashboards show a generated key / secret ONLY ONCE at creation. After that it's masked (e.g. `3f9a1c2b........`). Copy it immediately; if lost, recreate the consumer.
	- Using a key on a route
		- 1. Edit the route -> Plugins -> enable API Key Auth -> save.
		- 2. Test:
		  ```bash
		  curl -i https://api.example.com/echo/hello                          # 401 (no key)
		  curl -i -H "apikey: wrong123" https://api.example.com/echo/hello     # 401 (bad key)
		  curl -i -H "apikey: <real-key>" https://api.example.com/echo/hello   # 200
		  ```
	- Consumers are gateway-wide, not per route
		- A consumer's credential works on EVERY route that has the matching auth plugin, not just one route.
		  ```
		  mobile_app (key-auth) --+
		  partner_x  (key-auth) --+--> every key-auth route accepts ALL these keys
		  internal   (key-auth) --+
		  web_user   (jwt-auth) -----> rejected on key-auth routes (different auth type)
		  ```
		- | You want | How |
		  | --- | --- |
		  | any of my API-key clients can call it | key-auth on the route |
		  | only SPECIFIC consumers can call this route | key-auth + consumer-restriction plugin (list allowed usernames) |
		  | anyone can call it (public) | no auth plugin |
		- One line: a consumer = a key; key-auth on a route = "show me ANY valid key"; consumer-restriction = "only THESE keys".
	- Several auth plugins on one route = AND, not OR
		- Enabling key-auth + basic-auth + jwt-auth on the same route means ALL must pass. Each plugin runs separately and rejects the request if ITS credential is missing:
		  ```
		  route with key-auth + basic-auth + jwt-auth
		  client sends only: apikey: a909...
		    +-- key-auth:   valid
		    +-- basic-auth: no "Authorization: Basic ..." -> 401, stop
		    +-- jwt-auth:   (never reached)
		  ```
		- For "any ONE of them is enough" (OR), APISIX has a separate plugin: multi-auth.
		- | Setup | Behaviour |
		  | --- | --- |
		  | key-auth + basic-auth + jwt-auth as separate plugins | AND: every one must pass |
		  | multi-auth containing those three | OR: the first one that succeeds lets the request through; all fail -> 401 "Authorization Failed" |
		- Practical rule: one auth type per route, unless you use multi-auth.
	- How the backend knows who called
		- The gateway AUTHENTICATES, then PASSES the identity to the backend in a header. The backend trusts that header because only the gateway can reach it.
		  ```
		  client --apikey: a909...--> GATEWAY
		                                +-- key-auth valid -> consumer = partner_x
		                                +-- adds header X-Consumer: partner_x
		                                       |
		                                       v  (private network; backend firewall allows ONLY the gateway)
		                              BACKEND reads X-Consumer -> "this is partner_x"
		  ```
		- APISIX example: proxy-rewrite plugin with the built-in variable `$consumer_name`:
		  ```json
		  "proxy-rewrite": { "headers": { "set": { "X-Consumer": "$consumer_name" } } }
		  ```
		- NestJS side: a guard just reads it:
		  ```ts
		  @Injectable()
		  export class GatewayIdentityGuard implements CanActivate {
		    canActivate(ctx: ExecutionContext) {
		      const req = ctx.switchToHttp().getRequest();
		      req.client = req.headers['x-consumer'];   // set by the gateway, already verified
		      return Boolean(req.client);
		    }
		  }
		  ```
		- Rules that make this safe
			- | Rule | Why |
			  | --- | --- |
			  | backend reachable ONLY through the gateway (firewall allows only the gateway IP) | otherwise anyone could send a fake `X-Consumer: admin` header directly to the backend |
			  | authentication at the gateway, authorization in the backend | the gateway knows WHO is calling; only the backend knows whether user 42 may edit order 99 (needs your database) |
			  | with JWT, the backend can still read the token (don't hide credentials) | the gateway already verified signature + expiry; the backend just decodes the claims (user id, roles). Many teams verify again in the backend ("zero trust") |
		- Consumers are usually API CLIENTS (apps, partners, services), not every end user:
		  ```
		  consumer = "mobile_app" / "partner_x" / "billing-service"   <- apps calling your API
		  end user = a person logging into the mobile app              <- usually your own auth / identity provider
		  ```
	- Custom auth logic at the gateway
		- Yes, you can write your own. APISIX options, easiest first:
			- | Way | How it works | Language |
			  | --- | --- | --- |
			  | forward-auth (most common) | for every request the gateway calls YOUR auth service (e.g. a NestJS /verify endpoint); 2xx = allow, 401/403 = block; it can return headers (e.g. user id) to pass on to the backend | any (e.g. NestJS) |
			  | serverless functions | small inline code snippets run inside the gateway per request | Lua |
			  | custom plugin | write a full plugin and load it into APISIX | Lua |
			  | external plugin runner | plugin runs as a separate process next to APISIX | Go / Java / Python |
			  ```
			  client --> GATEWAY --"is this token ok?"--> your auth service (NestJS)
			               |                               +-- 200 + X-User-Id: 42 (ok) / 401 (blocked)
			               +-- ok -> forward to backend with X-User-Id: 42
			  ```
		- Note: the gateway only offers plugins it has loaded. A managed gateway may not offer forward-auth or custom plugins to customers.
- 7) API Gateway and Kubernetes
  id:: 0ceeeaa7-df77-406f-88d2-baf5425e5807
  collapsed:: true
	- Does Kubernetes have a built-in API gateway?
		- No. Kubernetes only defines routing resources (config objects). Nothing happens until you install a controller that implements them.
			- Ingress: host / path -> Service rules. The Ingress API is frozen; Kubernetes now recommends Gateway API.
			- Gateway API: an add-on with GatewayClass, Gateway and HTTPRoute resources.
		- The controller you install can be a plain proxy (routing + TLS) or a full API gateway like APISIX (adds consumers, auth, rate limits, plugins).
		- Ingress basics -> [[Kubernetes]] -> Kubernetes Networking -> 3. Ingress
	- Ingress controller vs API gateway
		- | | Plain Ingress controller | API gateway |
		  | --- | --- | --- |
		  | Routes host + path -> Service | yes | yes |
		  | TLS termination | yes | yes |
		  | Consumers, auth, per-consumer rate limits, plugins | basic | full |
	- Setup 1: gateway INSIDE the cluster (as the Ingress controller)
	  ```
	  Client
	    |
	    v
	  Cloud Load Balancer (public IP)      <- exposed with a LoadBalancer Service
	    |
	    v
	  APISIX pods (inside the cluster)     <- APISIX Ingress Controller turns K8s YAML into APISIX routes
	    |
	    v
	  Backend pods                         <- upstream nodes = the Service's pods
	  ```
		- Routes are written as K8s YAML (Ingress, Gateway API HTTPRoute, or APISIX's own resources). The controller translates them into APISIX routes and upstreams.
		- APISIX takes the place of the plain NGINX Ingress controller from the Kubernetes notes.
	- Setup 2: gateway OUTSIDE the cluster
	  ```
	  Client
	    |
	    v
	  API Gateway (VM or managed, outside K8s)
	    |
	    v
	  Cluster entry (LoadBalancer IP or Ingress)
	    |
	    v
	  Service -> Pods
	  ```
		- The gateway's upstream node = the cluster's entry address (load balancer IP / domain).
		- Two layers: the gateway applies API policies; the cluster's Ingress / LoadBalancer just routes inside the cluster.
		- Useful when one gateway fronts several clusters, or Kubernetes and non-Kubernetes backends together.
	- Which sits in front of whom?
		- The API gateway is the edge: clients hit it first, and Kubernetes runs the backends behind it.
		- Inside the cluster, the gateway is just more pods at the edge. Outside, it's a separate box in front of the cluster.
	- One line: Kubernetes runs your services; the API gateway decides who can reach them and how, either as the cluster's ingress (inside) or in front of it (outside).
- Interview one-liner
	- An API gateway is a reverse proxy with API policies: routes match requests (host + path + method), upstreams say where they go, and plugins apply auth, limits and transforms on the way, configured once at the edge for every backend.