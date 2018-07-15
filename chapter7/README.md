# Chapter 3: Node.js makes backend as simple as frontend

Note: The chinese book is at: http://bitcoin-on-nodejs.ebookchain.org

Yishu official site: http://ebookchain.org (Wechat group: chainclub)

Translation Cooperation: http://chainmap.org (BlockChain Developer Community) + https://elementus.io/ (Blockchain Data )

Translator/Validator: [Anbei Zhao](https://www.linkedin.com/in/anbei-zhao-b18b48167/), [Christopher Chen](https://www.linkedin.com/in/cchen408/)

Publisher: [George Zhao](https://www.linkedin.com/in/george-zhao-9568865/)


## Preface

In this article, we will learn how to build our own APIs with complex business logic.

Why the backend? First, most projects are complex and cannot be done without a backend. Second, processing capability is limited in the frontend. For most web applications, the simpler the frontend code, the better the performance and user experience.

Moreover, as we all know, bitcoin and other crypto currencies usually provide JSON APIs. If we process APIs in backend, we could implement business logic and create blockchain explorers (blockchain.Info), cryptocurrency wallets, and other applications.

## Project Requirement

It's important to understand what we need to do.

* API which can read the github.com data in backend

* Process data and send it to the front-end


## Development

There are many web application  frameworks available for Node.  Express is the de facto standard because it is minimal with many features available as plugins.  Many popular frameworks are based on Express, e.g.  sails, kraken, loopback.

### (1) Install Express

You can do this to install Express:

```
npm install --save express
```

Note: when installing a node.js module, if the --save parameter is specified, the module will be added to the dependencies list in the package.json file. You can then install all the modules listed in the dependencies list automatically with the npm install command.

### (2) Create a simple application

Enter the project directory, create a new file named app.js, and enter the following:

```
var express = require('express');
var app = express();

app.get('/', function (req, res) {
 res.send('Hello World!');
});

var server = app.listen(3000, function () {
 var host = server.address().address;
 var port = server.address().port;

 console.log('Example app listening at http://%s:%s', host, port);
});
```

Then, run the following command:

```
$ node app.js
```

Finally, open your browser and visit  http://localhost:3000/, and you shall see the output of "Hello World". 

This example is a complete web application. We now have a web service that is running on port 3000.

If you have experience using  **gulp**, you can imagine that we've built a pipeline from the backend to the frontend. Next step is to add various processing units to the pipeline so that the data will meet our requirements.

### (3) use the template engine

We already sent the string ‘hello world’ directly to the browser. What if it's an HTML file? Node.js does not have the function of rendering templates directly, so we will have to choose a third-party library to use, such as Jade, EJS, HBS, etc.

In this case, we use EJS, which is like Java's JSP or Rails' RHTML, which embeds code directly in HTML files.

```
npm install --save ejs
```

Now we can set up our first pipeline filter with app.set.

```
app.set('views', './views')
app.set('view engine', 'ejs')

app.get('/', function (req, res) {
 res.render('index');
});
```

Then we create a folder called "views", create index.ejs in it, open it, and copy "Hello Imfly!"  inside.

Restart the server (CTRL + C and use "node app" to startup), refresh the browser, see the changes ("Hello World!" has been changed to "Hello Imfly"), and it has been proved that the modules was enabled successfully.

### (4) use static file service

At the front end, we only have public/index.html files. Now, we copy its code into the views/index.ejs and modify the JavaScript, CSS references. Restart, refresh browser, but you shall see a bunch of messy display, press F12, open the browser console and you shall see error 404, what happened?

So far, we've only provided routing requests under "/" address request, and for any other address request, node.js, will by default return error 404. express, provides a simple solution:

```
app.use(express.static('./public', {
   maxAge: '0', //no cache
   etag: true
}));

app.get('/', function (req, res) {
...
```


This is our second pipeline filter, which means,  files under the public folder, including js, CSS, images, fonts, etc. shall be treated as static files, whose root is "./public". For instance: request address of "/public/js/app.Js" is "http://localhost:3000/js/app.js".

Note: all third-party development packages installed using **bower** are in the bower_components folder and need to be moved to the "public" folder. You also need to add a.bowerrc file to tell bower that the component installation directory has been changed and also need to modify gulpfile.js. You can also copy bower.json to the "public" folder.

Restart the service, refresh the page, now it works.

### (5) request githubApi in the background

There are also many ways to request github in the backend. request enables us to do it directly in the back end, which is equivalent to taking the front-end code directly to the backend. However, there is a better solution. Github provides a development package for node.js, which we can use directly:

```
NPM install lot - save
```

The official address: https://github.com/mikedeboer/node-github

How do we have this knowledge? One way is to read the official document carefully to see what resources it provides. Another way is to search at https://npmjs.com. Usually, many products will provide ready-made solutions there.

This component integrates almost all of the content of the githubApi, including search function. Copy the following code to app.js:

```
Var GitHubApi = the require (" lot ");
var GitHubApi = require("github");

//before app.get('/', ...)
app.get('/search', function(req, res){
       var msg = {
       q: 'bitcoin',
       sort: 'forks',
       order: 'desc',
       per_page: 100
   }

   github.search.repos(msg, function(err, data)        
   res.json(data); //json output
   })
})
```

Request http://localhost:3000/search in the browser, you can see the same outputs  with https://api.github.com/search/repositories?q=bitcoin&sort=forks&order=desc&per_page=100 (the format may be different).

http://localhost:3000/search is our own Api service, if you have your own database, data fusion would be easy.

Request this API and modify line 34 of public/js/app.js as follows:

```
url = url || 'http://localhost:3000/search';
```

Request http://localhost:3000/ in your browser and the result is the same as before.

### (6) modularized reconstruction

So far, we've been modifying app.js, adding various pipeline filters to it. If the business is complex, the file can be very large. In fact, any node.js application can be compressed into such .js file.

However, this is not suitable for development and maintenance. We need to break it down into separate files, identify its function by name, and use it separately, which is the modular development of node.js.

The modularity of node.js is very simple. Typically a module.exports can define a module; A file contains only one module; Any module can be referenced elsewhere using the **require()** method. The style is very similar to front-end:

```
// files/path/to/moduleName.js

// local variable
Var a = ' '

// public method (export module)
Var moduleName = {} or function(){} // is simply an object

// private method
function fun1 () {}

// export module
module.exports = moduleName;

In other files, we can use this:

var moduleName = require('/path/to/moduleName ");
```

By this way, we split the app.js file, into a typical MVC development pattern. In ruby on rails, we separate the code stored in the controllers, models, views, and router. Here, the view is defined in the views folder, and we'll focus on splitting the rest.

#### A) split model

The model is applied to process database or remote API resources. Naturally, we can isolate githubApi requests:

Create new folders and app/models/repo.js, cut and paste the following code

```
var GitHubApi = require("github");

/**
* from https://www.npmjs.com/package/github
* @type {GitHubApi}
*/
var github = new GitHubApi({
   // required 
   version: "3.0.0",
   // optional 
   debug: false,
   protocol: "https",
   host: "api.github.com", // should be api.github.com for GitHub 
   pathPrefix: "", // for some GHEs; none for GitHub 
   timeout: 5000,
   headers: {
       "user-agent": "My-Cool-GitHub-App" // GitHub is happy with a unique user agent 
   }
});

var Repo = {
   search: function(msg, callback) {
       var msg = msg || {
           q: 'bitcoin',
           sort: 'forks',
           order: 'desc',
           per_page: 100
       }

       github.search.repos(msg, callback);
   }
}

module.exports = Repo;
```

Note: Model is a collection of resources like a table in a database, please name items in resource classes and add/delete/check/modify data in the model. Naturally, part of the code in the front-end public/js/utils.js should be transferred here to output the data directly:

```
// from /public/js/utils.js
function treeData(data) {
   var languages = {};

   var result = {
       "name": "languages",
       "children": []
   }

...
```

#### B) split controller
The controller is responsible for requesting data from the model and sending it to the front end. In this case, we use app.get method to extract them separately, put them in the app/controllers/repos.js, and replace the code that requested the githubApi in the model.
var Repo = require('../models/repo');

```
var Repos = {
   //get '/'
   index: function(req, res) {
       res.render('index');
   },

   //get '/search'
   search: function(req, res) {
       Repo.search(req.query.query, function(err, data) {
           res.json(data);
       })
   }
}

module.exports = Repos;
```

Note: by convention, the name of the controller is usually the plural of the name (noun) of the corresponding model; The name of the behavior is usually an action (verb), so repos.index represents the list of libraries version, and repos.search is library information. In app:

```
var repos = require('./app/controllers/repos');

app.get('/search',  repos.search);
app.get('/',  repos.index);
```

This part of the code acts as a distribution route and we continue refactoring.

#### C) split routing

Create app/routing.js, and copy the code above to:

```
var repos = require('./controllers/repos');

var Router = function(app) {
   app.get('/search', repos.search);
   app.get('/', repos.index);
}

module.exports = Router;
```

In app.js:

```
var router = require('./app/router');

router(app);
```

After that, if we want to add any other routes, just modify router. Js.

#### D) collate views

Move views under app/views as a whole, and modify the app.js code so that the template engine points to the folder:

```
app.set('views', './app/views')
```

Then, create app/views/repos folder and move the views/index.ejs file into it.

Note: view is an interface element, usually an HTML file, and by convention its folder has the same name as the controller's repos, and each filename has the same name as the controller's action, such as index-> index.ejs

Of course, the view can also be modularized according to the characteristics of the template engine, and further refined into layout.ejs, header.ejs, etc., which is convenient for repeated use.

After such modularization, we easily implemented a simple MVC framework (as shown in the figure), which greatly improved its usability and scalability. We've been able to quickly add more features.

