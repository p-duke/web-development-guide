_Note: May need to do a separate article on IoC_
## Javascript Composition

### Composition in Javascript<sup>[1]</sup>
- When creating an object domain Javascript provides flexibility based on its prototypical inheritance. One approach is to use a more classical inheritance approach or another is using composition. 
- One difference is that inheritance provides a *is-a* relationship while composition is more of a *has-a* relationship.
- Inheritance is when a class is based on another using the same implementation (Object.prototype parent)
  - A Dog subclass would gain methods and properties from an Animal superclass like poop or bark
  - This creates a relationship of a Dog *is a* Animal
- Composition is about taking simple objects and combining them to build more complex ones.
  - To build a Car you might define a function for constructing essential features like engine, design and brakes.
  - This creates a relationship of a Car *has a* engine, brakes, and design.
- Mixins is a way of achieveing inheritance
  - 


## Dependency Injection

### Dependency Injection in Javascript<sup>[2]</sup>
- Inversion of control (IoC) and dependency injection can be considered synonomous
- Dependency injection is a software design pattern allows someone to remove hard coded dependencies and make it flexible to change them
- Dependencies can be injected via a constructor, defined method, or a setter property.
- Advantages
  - Decreses coupling between object and its dependency
  - Doesn't require any change in code behavior
  - Helps isolate the client from impacts in code changes
  - Allows the system to be reconfigured without changing the existing code
  - Allows concurrent or independent development
  - Makes code more maintainable and testable by replacing dependencies with mocks and stubs
- Disadvantages
  - When instantiating a type you have to know which dependencies to use
  - Hides a types instantiation and dependency resolving logic so if an error occurs it can be difficult to troubleshoot
  - It can require you to write more lines of code
  - It can be slower when instantiating a type with keyword new

In this example the dependencies are hard coded:
```javascript
function Engine() {
  this.hp = 256;
}

Engine.prototype.start = function () {
	console.log("Engine with " + this.hp + " hp has been started...");
}

function Car() {
	this.name = "wv";
	this.engine = new Engine();
}

Car.prototype.start = function () {
	if (this.engine) {
		this.engine.start();
	}
}

function Driver() {
	this.name = "tom";
	this.car = new Car();
}

Driver.prototype.drive = function () {
	if (this.car) {
		this.car.start();
	}
}

var driver = Driver();
driver.drive();

// Engine with 256 hp has been started...
```
We have modified our existing code and it is extensible:
```javascript
function Engine(hp) {
	this.hp = hp;
}

Engine.prototype.start = function () {
	console.log("Engine with " + this.hp + " hp has been started...");
}

function Car(name, engine) {
	this.name = name;
	this.engine = engine;
}

Car.prototype.start = function () {
	if (this.engine) {
		this.engine.start();
	}
}

function Driver(name, car) {
	this.name = name;
	this.car = car;
}

Driver.prototype.drive = function () {
	if (this.car) {
		this.car.start();
	}
}

var driver = Driver("tom");

driver.car = new Car("wv", new Engine(256)));

driver.drive();

// Engine with 256 hp has been started...
```

### Demystifying Depenendency Injection<sup>[3]</sup>

In this example you can see that the UserService has the instance of the Database hardcoded

```javascript
class Database {
    insert(table, attributes) {
        // inserts record in database
        // ...

        const isSuccessful = true
        return isSuccessful
    }
}

class UserService {
    create(user) {
        // do a lot of validation etc.
        // ...

        const db = new Database
        return db.insert('users', user)
    }
}

const userService = new UserService
const result = userService.create({ id: 1})
console.log(result)
```

Rather than newing up the Database instance inside the "create" method, we inject it into the UserService like this
```javascript
class Database {
    insert(table, attributes) {
        // inserts record in database
        const isSuccessful = true
        return isSuccessful
    }
}

class UserService {
    constructor(db) {
        this.db = db
    }

    create(user) {
        return this.db.insert('users', user)
    }
}

const db = new Database
const userService = new UserService(db)

const result = userService.create({ id: 1})
console.log(result)
```

### Sources

[1]: https://www.richardkotze.com/coding/composition-in-javascript 'Composition in Javascript'
[Composition in Javascript](https://www.richardkotze.com/coding/composition-in-javascript) 

[2]: https://www.devbridge.com/articles/dependency-injection-in-javascript/ 'Dependency Injection in Javascript'
[Dependency Injection in Javascript](https://www.devbridge.com/articles/dependency-injection-in-javascript/) 

[3]: https://dev.to/michi/demystifying-dependency-injection-inversion-of-control-service-containers-and-service-providers-5e72 'Demystifying Dependency Injection'
[Demystifying Dependency Injection](https://dev.to/michi/demystifying-dependency-injection-inversion-of-control-service-containers-and-service-providers-5e72) 
