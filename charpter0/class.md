###寄生继承
```js
function Vehicle() {
    this.engines = 1;
}
Vehicle.prototype.ignition = function() {
    console.log("Turning on my engines");
}
Vehicle.prototype.drive = function() {
    this.ignition();
    console.log("Streering and moving forward");
}

function Car() {
    var car = new Vehicle();
    car.wheels = 4;
    var vehDrive = car.drive;
    car.drive = function() {
        vehDrive.call(this);
        console.log("Rolling on all " + this.wheels + " wheels!");
    }
    return car;
}

var myCar = new Car();
myCar.drive();

```
