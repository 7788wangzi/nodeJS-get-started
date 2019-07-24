# nodeJS - get started

## What is Node.js

Node App are:

- Build twice as fast with fewer people
- 33% fewer lines of code
- 40% fewer files
- 2x request/sec
- 35% faster response time

Why it's popular?

- Great for prototyping and agile development
- Superfast and highly scalable
- JavaScript everywhere
- Cleaner and more consistent codebase
- Large ecosystem of open-source libs

What it is? - Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine, Node is not a programming language. Node is not a framework.

**NOTE**: NodeJs is suitable for handling requests asynchronously, not suitable for compute-heavy applications.

## Setup Environment

Install [Node.js](https://nodejs.org/)

Open command prompt, create a new folder:

```commandline
mkdir first-app
cd first-app
```
Use `npm` to initialized a new Node.js app, this will create a **package.json** file containing metadata that describes the app.
```commandline
npm init -y
```

Open this folder in Visual Studio Code:

```
code .
```

Add a new file `app.js`, and in the js file, write a regular javascript code, for example, a function:

```javascript
// define a function
function sayHello(name){
    console.log("Hello, "+ name);
}

// Call the function
sayHello('Qijie');
```

Run this code in command line with command:

```
node app.js
```

## Node Core

```js
console.log();

setTimeout();
clearTimeout();

setInterval();
clearInterval();

```

## Module

In Node.js, every js file is called a **Module**, the variables and functions that are defined inside a module are private by default, they are not accessible from outside the module. In order to make a variable or function to be public, use the keyword **exports**.

### How to create a module

In VS Code, create a new js file called logger.js, then define a variable and a function as following:

```javascript
var url = 'http://mylogger.io/log';

var log = function(message){
    console.log(message);
}

module.exports.log = log;
module.exports.endPoint = url;
```

### How to use a module

In the main module `app.js`, we import logger.js and call the log function:

```javascript
const mylog = require('./logger'); // mylog is an instance of the module, use mylog.xx to use the functions and variables
mylog.log('hello from another module');
```

**NOTE**: In each js file, the code will be wrapped inside a function like following:

```javascript
(function (exports, require, module, __filename, __dirname){
    var url = 'http://mylogger.io/log';

    var log = function(message){
        console.log(message);
    }

    module.exports.log = log;
    module.exports.endPoint = url;
})
```

you could see *exports, require, module* are parameters of this function, therefore we could use it in our js file.

### Use *path* module

```javascript
const path = require('path');
var pathObj = path.parse(__filename);
console.log(pathObj);
```

### Use *os* module

```javascript
const os = require('os');

var totalMemory = os.totalmem();
var freeMemory = os.freemem();

console.log(`Total Memory: ${totalMemory}`);
console.log(`Free Memory: ${freeMemory}`);
```

### Use *fs* module

```javascript
const fs = require('fs');

// Synchronous
const files = fs.readdirSync('./');
console.log(files);

//Asyncrhonous
fs.readdir('./',function(err, files){
    if(error){
        console.log("Error: "+error)
    }
    if(files){
        console.log("Results: "+files);
    }
})
```

Example of fs, output the files under a folder path:

```javascript
const fs = require('fs');
fs.readdir('d:\\',function(error,files){
   	if(error){
    	console.log(error);
	}        

    files.forEach(function(file){
        console.write(file+"\r\n");
    })
           
});
```

### Use *events* module

```javascript
const EventEmitter = require('events'); // Upper case E for EventEmitter, means this is a Class

const emitter = new EventEmitter();
emitter.on("MessageLogged",function(arg){
    console.log("listener called", arg)
});

emitter.emit('MessageLogged',{id:1,url:"http://"});
```

### Use *http* module

Http module is an EventEmitter, you could use the *.on* events.

```javascript
const http = require('http');
var server = http.createServer();

server.on("connection",function(socket){
    console.log('New Connection...');
});

server.listen(3000,function(){
    console.log('Listening on port 3000...');
});


```

Example of http, print Multiplication Table to browser:

```javascript
const http = require('http');
var server = http.createServer((req, res) =>{
    if(req.url ==="/"){
        for(let i=1; i<=9; i++)
        {
            for(let j=1; j<=i; j++)
            {
                res.write(j+'*'+i+"="+j*i+" ");
            }
            res.write('\r\n');
        }

        res.end();
    }
});

server.listen(3000,function(){
    console.log('Listening on port 3000...');
});
```

output result:

![multiply table ](/static/9multiply9table.PNG)



