Modules.js
===================

A lightweight modules manager for JavaScript. Don't handle scripts loading.

Use can use it like that :

```javascript

var FooBar = Modules.define('play.foo.bar', function() {
    return {
         hello: function(name) { return ("Hello " + name + "!"); }
    };
});

Modules.use('play.foo.bar', function(fb) {
    console.log(fb.hello('foobar'));
});

console.log(FooBar.hello('foobar'));

var Bar = Bar || Modules.get('play.foo.bar');
console.log(Bar.hello('foobar'));

Modules.create('ModuleA', function(ModuleA) {
    ModuleA.hello = function() {
        console.log("Hello from module A");
    };
    ModuleA.setupModule = function() {
        console.log("Setup ModuleA");
    };
    ModuleA.messageReceived = function(msg) {
        console.log("Received in ModuleA %s", msg);
    };
});

Modules.create('ModuleA:1.0', function(ModuleA) {
    ModuleA.hello = function() {
        console.log("Hello from module A v 1.0");
    };
    ModuleA.setupModule = function() {
        console.log("Setup ModuleA v 1.0");
    };
    ModuleA.messageReceived = function(msg) {
        console.log("Received in ModuleA v 1.0 %s", msg);
    };
});

Modules.create('ModuleA:2.0', function(ModuleA) {
    ModuleA.hello = function() {
        console.log("Hello from module A v 2.0");
    };
    ModuleA.setupModule = function() {
        console.log("Setup ModuleA v 2.0");
    };
    ModuleA.messageReceived = function(msg) {
        console.log("Received in ModuleA v 2.0 %s", msg);
    };
});

Modules.define('ModuleB', function() {
    return {
        hello: function() {
            console.log("Hello from module B");
        },
        moduleReady: function() {
            console.log("ModuleB is ready !!!");
        },
        messageReceived: function(msg) {
            console.log("Received in ModuleB %s", msg);
        }
    };
});

Modules.createWithDependencies('ModuleC', ['ModuleA', 'ModuleB'], function(ModuleC, ModuleA, ModuleB) {
    ModuleC.hello = function() {
        ModuleA.hello();
        ModuleB.hello();
        console.log("Hello from module C");
    };
    ModuleC.messageReceived = function(msg) {
        console.log("Received in ModuleC %s", msg);
    };
});

Modules.defineWithDependencies('ModuleD', ['ModuleC', 'get'], function(ModuleC, get) {
    return {
        hello: function() {
            ModuleC.hello();
            console.log("Hello from module D");
        },
        messageReceived: function(msg) {
            console.log("Received in ModuleD %s", msg);
           get('ModuleA').hello();
        }
    };
});

Modules.initModules();

Modules.use('ModuleD', function(ModuleD) {
    ModuleD.hello();
});
Modules.uses(['ModuleA', 'ModuleB', 'ModuleC', 'ModuleD'], function(ModuleA, ModuleB, ModuleC, ModuleD) {
    ModuleA.hello();
    ModuleB.hello();
    ModuleC.hello();
    ModuleD.hello();
});

Modules.broadcast("Hello Modules ...");

Modules.sendToModule('ModuleA', 'Hello ModuleA ...');

Modules.sendToModules(['ModuleA', 'ModuleB'], 'Hello ModuleA and ModuleB ...');

Modules.sendToModulesMatching(/Module[A-B]/i, 'Hello Module matching AB...');
Modules.sendToModulesMatching(/Module[C-D]/i, 'Hello Module matching CD...');

Modules.sendToModulesMatching(/Module[A-Z]:[0-9*]\.[0-9*]/i, 'Hello versioned Modules ...');
Modules.sendToModulesMatching(/Module[A-z]:1\.[0-9*]/i, 'Hello Modules in v 1.x ...');
Modules.sendToModulesMatching(/Module[A-Z]:2\.[0-9*]/i, 'Hello Modules in v 2.x ...');

```