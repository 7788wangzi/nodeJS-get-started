## Use *Express* to build RESTful service



**Re**presentational **S**tate **T**ransfer

CRUD Operations: Create, Read, Update, Delete

| Operation | Method Name | Sample                  |
| --------- | ----------- | ----------------------- |
| Create    | Post        | POST /api/customers     |
| Read      | Get         | GET /api/customers/1    |
| Update    | Put         | PUT /api/customers/1    |
| Delete    | Delete      | DELETE /api/customers/1 |
|           |             |                         |

## get started code

Create a folder, initialized the folder with `npm init --yes` to generate package.json, then create a .js file called app.js, write following code.

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    if(req.url === '/'){
        res.write('Hello World');
        res.end();
    } 
    
    if(req.url === '/api/courses'){
        res.write(JSON.stringify([1,2,3]));
        res.end();
    }
});

server.listen(3000)
Console.log('Listening on port 3000...');
```



## Use *express* module

Express is a fast web framework for node, to install it, use npm command:

```
npm i express
```

require('express') returns a function, that function is an object of type Express Application.

```javascript
const express = require('express');
const app = express();

// In app, the restful HTTP methods are:
app.get()
app.post()
app.put()
app.delete()

```

### *get()* method



get() method is a rout, it has a callback function which contains two parameters, one is [request](<http://expressjs.com/en/4x/api.html#req>)

```javascript
const express = require('express');
const app = express();

app.get('/',(req, res) => {
	res.send('Hello World');
});

app.get('/api/courses', (req, res) => {
    res.send(JSON.stringify('[1,2,3]'));
});
app.listen(3000, () => {console.log('Listening on port 3000...')});


```

## Use *nodemon* 

Use *nodemon* to avoid restart app each time after modification you code.

Install nodemon

```javascript
npm i -g nodemon

```

Now, running the application using nodemon instead of node.

```javascript
nodemon app.js

```

## Use environment variable to define a port

Define PORT variable in command line:

```javascript
set PORT = 5000;
```



Use PORT value in code, for example, to update `app.listen()` code like this:

```javascript
const port = process.env.PORT || 3000; // if process.env.PORT defined, use it, otherwise use 3000
app.listen(port, () => { console.log(`Listening on port ${port}...`)});
```



## Get a single course

```javascript
const express = require('express');

const app = express();

var courses =[
    {id:1, name:'how to cook'},
    {id:2, name:'how to fish'},
    {id:3, name:'how to drink'}
]

app.get("/",(req, res) =>{
    res.send("Hello World!!!");
})

app.get("/api/courses",(req, res)=>{
    res.send([1,2,3]);
})

app.get("/api/courses/:id",(req, res)=>{
    let course = courses.find(c=>c.id===parseInt(req.params.id));

    if(!course)
    {
        // 404
        res.status(404).send('The course with the given id was not found.');
    }
    else
        res.send(course);
})

const port = process.env.PORT || 3000; // if process.env.PORT defined, use it, otherwise use 3000
app.listen(port, () => { console.log(`Listening on port ${port}...`)});
```



Get a single course in Browser with navigating to http://localhost:3000/api/courses/1



## Create a course using *Post()* method

```javascript

app.use(express.json());

app.post('/api/courses', (req, res) => {
    const course = {
        id: courses.length + 1,
        name: req.body.name
    };
    
    courses.push(course);
    
    res.send(course);
});


```

## Use [Postman](<https://www.getpostman.com/>) to post data to server from browser



## Use [JoI](<https://www.npmjs.com/package/joi>) to make validation check

Install Joi in command line: 

```javascript
npm i joi
```

In code, define a variable `const Joi = require('joi')`,  Joi is a class.

```javascript
app.use(express.json());

app.post('/api/courses', (req, res) => {
    
    const schema ={
        name: Joi.string().min(3).required()
    };
    
    const result = Joi.validate(req.body, schema);
    console.log(result);
    
    if(result.error)
    {
        res.status(400).send(result.error.details[0].message);
        return;
    }
    
    const course = {
        id: courses.length + 1,
        name: req.body.name
    };
    
    courses.push(course);
    
    res.send(course);
});
```

## Update with method `put()`

```javascript
app.put('/api/courses/:id', (req, res) => {
   // Look up the course
   // If not existing, return 404
    const course = courses.find(c => c.id === parseInt(req.params.id));
    if(!course) {res.status(404).send('The course with the give id was not found.');}
   
   // Validate
   // If invalid, return 400 - Bad request    
    const schema ={
        name: Joi.string().min(3).required()
    };
    
    const result = Joi.validate(req.body, schema);
    
    if(result.error)
    {
        res.status(400).send(result.error.details[0].message);
        return;
    }
   // Update course
   // Return the updated course    
    course.name = req.body.name;
    res.send(course);
    
});
```

## Delete with method `delete()`

```javascript
app.delete('/api/courses/:id', (req, res) => {
   // Look up the course
   // If not existing, return 404
    const course = courses.find(c => c.id === parseInt(req.params.id));
    if(!course) {res.status(404).send('The course with the give id was not found.');}
    
   // Delete
   	const index = courses.indexOf(course) ;
   	courses.splice(index, 1);
    res.send(course);
});
```

