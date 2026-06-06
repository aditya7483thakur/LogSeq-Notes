### Problem it solves
	- Decouple **who triggers an action** from **who performs the action**, while allowing:
		- undo / redo
		- queuing
		- logging
		- scheduling actions
-
- ### Bad Design (Tightly Coupled Invoker)
	- ```tsx
	  class SmartHomeController {
	    turnOnLight() {
	      light.on();
	    }

	    setTemperature(temp: number) {
	      thermostat.setTemperature(temp);
	    }
	  }
	  ```
-
- ### Why this is a problem
	- Controller knows **every device**
	- Adding a new device/action → modify controller
	- No undo/redo
	- No scheduling or command history
	- Hard to map UI buttons to actions
	- 🚨 **LLD smell:**
		- > "Invoker directly calls business logic"
-
- ### Core Idea (Command)
	- > **Turn an action into an object**
	- Each action knows:
		- what to do
		- which receiver to act on
		- (optionally) how to undo itself
-
- ### Good Design (Command Pattern)
	- ### 1️⃣ Command Interface
	  ```tsx
	  interface Command {
	    execute(): void;
	    undo(): void;
	  }
	  ```
	- ### 2️⃣ Receivers (Do the real work)
	  ```tsx
	  class Light {
	    on() { console.log("Light ON"); }
	    off() { console.log("Light OFF"); }
	  }

	  class Thermostat {
	    private temp = 20;

	    setTemperature(t: number) {
	      this.temp = t;
	      console.log("Temp set to", t);
	    }

	    getCurrentTemperature() {
	      return this.temp;
	    }
	  }
	  ```
	- ### 3️⃣ Concrete Commands (Action = Object)
	  ```tsx
	  class LightOnCommand implements Command {
	    constructor(private light: Light) {}

	    execute() { this.light.on(); }
	    undo() { this.light.off(); }
	  }
	  ```
	  ```tsx
	  class SetTemperatureCommand implements Command {
	    private prevTemp!: number;

	    constructor(
	      private thermostat: Thermostat,
	      private newTemp: number
	    ) {}

	    execute() {
	      this.prevTemp = this.thermostat.getCurrentTemperature();
	      this.thermostat.setTemperature(this.newTemp);
	    }

	    undo() {
	      this.thermostat.setTemperature(this.prevTemp);
	    }
	  }
	  ```
	- ### 4️⃣ Invoker (Triggers command)
	  ```tsx
	  class SmartButton {
	    private history: Command[] = [];

	    setCommand(cmd: Command) {
	      this.history.push(cmd);
	      cmd.execute();
	    }

	    undo() {
	      const cmd = this.history.pop();
	      cmd?.undo();
	    }
	  }
	  ```
	- ### Usage
	  ```tsx
	  const light = new Light();
	  const thermostat = new Thermostat();

	  const button = new SmartButton();

	  button.setCommand(new LightOnCommand(light));
	  button.setCommand(new SetTemperatureCommand(thermostat, 22));

	  button.undo(); // undo temp
	  button.undo(); // undo light on
	  ```
-
- ### What this achieves
	- Invoker knows **only Command**
	- Devices know **nothing about UI**
	- Undo logic stays with the command
	- New actions added without modifying invoker
-
- ### Key Intuition ⭐ (MOST IMPORTANT)
	- > **Command = "Action becomes a first-class object"**
	- Once action is an object, you can:
		- store it
		- queue it
		- undo it
		- retry it
		- log it
-
- ### When to use it
	- UI buttons / menus
	- Undo / redo
	- Job queues
	- Task scheduling
	- API request execution
	- Remote controls
-
- ### When NOT to use it
	- Simple one-off method calls
	- No need for undo / queue / decoupling
	- Logic is trivial
	- > **Interview rule:**
		- If a function is enough, don't wrap it in Command.
-
- ### Common Interview Traps ❌
	- Confusing Command with Strategy
	- Putting business logic in Invoker
	- Creating commands when no flexibility is needed
	- Over-abstracting simple calls
