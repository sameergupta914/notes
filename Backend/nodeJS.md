# Node js notes

- -dirname -> shows the current directory, not always accessible
- module -> helps to export one js file to another, can also decide which part or function to send commonjs modules.
- package-> it is a folder which contains a package.json file. (json: javascript object notation).
	

- two ways of moduling->
	- Commonjs-> normally .js is commonjs module. if `{"type" : "module"}` present in package.json then .mjs file becomes commonjs. for exporting in cjs->
		- `module.exports= FlightRepository;`
		- `module.exports={ flight_service, search_service }`
	- for importing in cjs->
		- `const FlightRepository = require('./Repository/FLightRepository');`
		- `const { }= require('./Service/index')`

	- Ecmascript js-> inorder to use ecmascript modules, rename file to .mjs instead of .js OR is by adding this in package.json file: `{ "type" : "module" }` , after this .js file becomes ecmascript and .mjs become commonjs. for exporting in this->
		- `export default FlightRepository;`
	- to import in ejs->
		- `import FlightRepository from './Repository/FlightRepository.js';`

- ![title](img/moduling.png)

- `console.log(process.argv);'` -> gives an array
1st element- using what element youre executing the file
2nd element- path of the file
3rd,4th,..- arguments you have passed
- `process.stdout.write(" ");` -> will not give a line space unless given.

- package manager in node (npm(node package manager), yarn) -> manages the installation of packages handles its dependencies resolution, version management.

- npx(part of npm) -> used to directly executes some function of the installed package from the terminal
minimist- `node file_path -a X -b Y` , X and Y are your inputs

- axios- helps you to make http call

- fs- `const fs=require("fs");`
- in es6 `--dirname` cant be used to get file path instead this is used `import.meta.url`

- to read file from index.html-> 
```
import {readFile} from 'fs/promises';
const filePath=new URL('./index.html', import.meta.url);
const data= await readFile(filePath, {encoding: 'utf8' });
console.log(data); 
```	

- `npm init` -> to initialize basic node project, to create a basic node file or to create package.json file type in terminal, json file is created containing these metadata.

- installing some dependencies like axios `npm i axios`, a package-lock.json file is created which ensures that it contains the same version of all packages and their dependencies when used in different enviroment.
- we don't upload node_modules file on GitHub so package-lock.json helps in ensuring we are using same version of all stuffs

# Telegram Bot

```
*const axios=require('axios');
const {Telegraf}=require('telegraf');

let sameer='i love you too shruti baby, lets meet on 31st december and celebrate our 3 year togetherness, ok cutie?';

const bot=new Telegraf('7831772906:AAGAP2wSOk_WRrww0gtghGfuPRmjLrsdtkE');

bot.start((ctx) => ctx.reply('welcome cutie'));

bot.command('iloveyou' , (ctx) => ctx.reply(sameer));

bot.command('ok', (ctx) => ctx.reply('okay cutie thanks a lot'));

bot.on('sticker', (ctx) => ctx.reply('❤️'));

bot.command('sex' , (ctx) => ctx.reply('gndi gndi baate mat kro mere se mai bas ek bot hu smjhi gndi ladki'));

bot.command('xyz', async function(ctx){

    const res=await axios.get('<https://raw.githubusercontent.com/singhsanket143/Sep-2022-Node-Batch-Codes/refs/heads/master/Nov_19-Project_0/index.js>');

    return ctx.reply(res.data);

})

bot.launch();*

```

# CLIENT SERVER ARCHITECTURE

Server->
- server can be a hardware or a software. it is a hardware or a computer program that provides a service to another computer program or a different machine.

- API contracts- REST, JRPC, SOAP, etc

- REST-> representational state transfer, this is just a set of rules-
	- every real life entity is expected to be represented as a resource.
	- every time with a RESTfull api request we have to sent type/methods of the request.
	- there will be dedicated URLs.

- GET-> retrieve info about a resource, data is sent in URL, that means it gets saved in out history, we can log it cache it etc

- POST-> create side effects on a resource, data is not expectedto be sent in URL, rather there is a request body/payload.

- PUT-> make complete update to a resource,

- PATCH-> make partial update to a resource,

- DELETE-> delete a resource

- three ways to send data-
	- request param- in this we send unique identifier of a resource, eg: /movies/lifeofpi
	- query param- along with unique identifier we send key value pairs with it, eg : /categories electronics?company=samsung&order=desc
	- request body- it is a separate payload containing key value pairs

- In rest convention, data/msgs are sent apart from URL are sent in JSON

# SETTING UP HTTP SERVER

- using http module which is inbuilt by node, http module contains a function called as the `createServer`
- this createServer function takes a callback as the input, now inside the callback we get 2 arguments
	- request-> this argument contains all the details about the incoming req
	- response-> this argument contains functions using which we can prepare the response.

- the createServer function returns us the server object
- port-> request will identify the IP address but multiple processes may be running on that IP, so port help in deciding which process to go to.

- code for starting server using node only with the help of http inbuilt module->

```
const http=require('http');
const PORT=3000;
const server=http.createServer(function exec(request,response){
    console.log(request.method);
    if(request.url="/sam"){
        response.end("helo");
    }
});
server.listen(PORT, function process(){
    console.log("heyyy");
});

```

to install and run express- npm init, npm i express
code using expressJS for same->

```
	const app=express();
	const PORT=3000;
	app.get('/',function(req,res){
	    res.send("hey this is common");
	});
	app.post('/home',function(req,res){
	    res.send("this is post request to home");
	});
	app.patch('/sam',function(req,res){
	    res.json({
	        name:"sameergupta",
	        success:true
	    })
	})
	app.listen(PORT,function(){
	    console.log("ji ok");
	});

```

# RESTfull APIs

API to create a blog-

API to read blog-

API to delete blog-

API to update a blog-

- NODEMON->
	- after updating your code in the server, you can automatically re-run your code using -> `npx nodemon index.js` , to download it-> `npm i nodemon`

	- or go to your `package.json` file and add `"start" : "npx nodemon index.js"` in the scripts section, now you can simply re-run using code-> `npm start`

- small code for get,post using array as database-

```
	const bodyParser=require('body-Parser');
	const app=express();
	const PORT=3000;
	app.use(bodyParser.urlencoded({extended:true}));
	app.use(bodyParser.json());
	let arr=[];
	app.get('/blogs',(req,res)=>{
	    return res.status(200).json({
	        data:arr,
	        success:true,
	    });
	});
	app.post('/blogs',(req,res)=>{
	    // console.log(req.body);
	    arr.push({title:req.body.title,
	        content: req.body.content,
	        id:Math.floor(Math.random()*100)
	    });
	    return res.status(201).json({
	        success:true,
	    });
	})
	app.get('/blogs/:id',(req,res)=>{
	   const result= arr.filter((blog)=>blog.id= =req.params.id);
	   return res.status(201).json({
	    data:result,
	    success:true
	   });
	})
	app.listen(PORT,function(){
	    console.log("ji ok",PORT);
	    });

```


# MVC architecture

- model->handles the backend part, includes service, repository, model
- service-> handles all the logic needed
- repository-> handles all the connection or calls the database

- model-> decides one line of the database table, decide structure of database
- view->handles or includes the frontend portion which is viewed by the user
- controllers->passed calls made from user on views to model portion

- middleware-> are functions that have access to the request, response and next middleware function in the application's req-res cycle

```
user->route->middleware->controller->service->repo->service->controller->middleware->route->back to user

```

# Sequelize

- Sequelize is node.js ORM for mysql, oracle, postgres, sqlite and more helps in changing mysql language to class oriented language

```bash
npx sequelize db:create
npx sequelize model:generate --name City --attributes name:String
npx sequelize db:migrate
npx sequelize db:migrate:undo
```