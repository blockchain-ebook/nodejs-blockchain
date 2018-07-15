# Chapter 4 - Starting with app.js 

Note: The chinese book is at: http://bitcoin-on-nodejs.ebookchain.org

Yishu official site: http://ebookchain.org (Wechat group: chainclub)

Translation Cooperation: http://chainmap.org (BlockChain Developer Community) + https://elementus.io/ (Blockchain Data )

Translator/Validator: [Anbei Zhao](https://www.linkedin.com/in/anbei-zhao-b18b48167/), [Christopher Chen](https://www.linkedin.com/in/cchen408/)

Publisher: [George Zhao](https://www.linkedin.com/in/george-zhao-9568865/)


## Preface

In the introductory chapter, we know that node.js applications are combined into one file, which can also be split into multiple files for ease of development.

The file that to be split is either app.js or server.js, which is also called the entry program.

Ebookcoin uses app.js. In this chapter, we'll take a close look at the document and learn about overall architectural process.

## The source code

https://github.com/Ebookcoin/ebookcoin/blob/v0.1.3/app.js


## Interpretation

### 1. Configuration processing

Any application will provide some parameters. There are many ways to deal with these parameters. But in general, we usually need to provide an ideal environment, the default configuration, and a way to modify it.

#### (1) global default configuration

Usually when there are a few default parameters, you can code them directly. But a more flexible approach is to use separate files. The file./config.json is used to hold the global configuration, such as:

```
{
    "port": 7000,
    "address": "0.0.0.0",
    "serveHttpAPI": true,
    "serveHttpWallet": true,
    "version": "0.1.1",
    "fileLogLevel": "info",
    "consoleLogLevel": "log",
    "sharePort": true,
    ...
```

In practice, use ‘require’. 

```
var appConfig = require("./config.json"); //line 4
```

However, for flexibility, the default value usually allows users to modify it.

#### (2) use commander component to introduce command-line options

commander is a node.js third-party component (installed with NPM) that is often used to develop command-line tools and is extremely simple to use. 

```
// line 1
var program = require('commander');

// line 19
program
    .version(packageJson.version)
    .option('-c, --config <path>', 'Config file path')
    .option('-p, --port <port>', 'Listening port number')
    .option('-a, --address <ip>', 'Listening host name or ip')
    .option('-b, --blockchain <path>', 'Blockchain db path')
    .option('-x, --peers [peers...]', 'Peers list')
    .option('-l, --log <level>', 'Log level')
    .parse(process.argv);
```

In this way, you can add -c,-p and other options when executing commands on the command line, such as:

```
node app.js -p 8888
```

At this point, this option is saved as program.port, so you can modify it manually:

```
// line 39
if (program.port) {
    appConfig.port = program.port;
}
```


This is a common and simple way to handle node. js application global configuration.

We'll introduce commander components in the next chapter.

## 2. Exception capture

In the summary of the first part, we mentioned Exception capture for global exceptions.Note: the domain module is no longer recommended, and this code will be removed in subsequent updates.

### (1) use uncaughtException to catch process exceptions

```
// line 65
process.on('uncaughtException', function (err) {
    // handle the error safely
    logger.fatal('System error', { message: err.message, stack: err.stack });
    process.emit('cleanup');
});
```

### (2) use domain module to catch global exceptions

```
// line 96
var d = require('domain').create();
d.on('error', function (err) {
    logger.fatal('Domain master', { message: err.message, stack: err.stack });
    process.exit(0);
});
d.run(function () {
...
```


In addition, domain is also used for each module

```
// line 415
var d = require('domain').create();

d.on('error', function (err) {
scope.logger.fatal('domain ' + name, {message: err.message, stack: err.stack});
});
...
```

## 3. Module loading

We use async process management components to load modules.

We use async.auto for sequential calls; and use async.parallel to run modules in parallel. When an error occurs, we use async.eachSeries.

The following figure illustrates the sequence of loading and running of each module from lines 103 to 438.

In the next chapter, we will learn async component in detail. Here, you don't have to worry too much about async as long as you can understand code's intent. 

### (1) initial network

We see the Express framework in package.json. Through the introduction in the previous section, you know that you must initialize in the entry program. How is it called? 

As we know, Express is an important web development framework for node.js. The network here is essentially a web application based on Express, which is the reason why the white paper will promote the Http protocol.

```
// line 215
network: ['config', function (cb, scope) {
    var express = require('express');
    var app = express();
    var server = require('http').createServer(app);
    var io = require('socket.io')(server);

    if (scope.config.ssl.enabled) {
        var privateKey = fs.readFileSync(scope.config.ssl.options.key);
        var certificate = fs.readFileSync(scope.config.ssl.options.cert);

        var https = require('https').createServer({
    ...

```

Note: this is the common commands of async.auto, all callbacks of network are put in an array, and the last callback function (function (cb, scope) {/ / code}), can be called naturally, such as scope.config.

The code above is aimed to initialize our service. 

### (2) build links

Starting with the code below, we can see the structure of our application. 

```
// line 270
connect: ['config', 'public', 'genesisblock', 'logger', 'build', 'network', function (cb, scope) {
```

Then, we load a few middleware, stating that 1)the application accepts HTML files driven by ejs template and 2)static files such as iviews, mages and styles are in the public folder. 

```
// line 277
scope.network.app.engine('html', require('ejs').renderFile);
scope.network.app.use(require('express-domain-middleware'));
scope.network.app.set('view engine', 'ejs');
scope.network.app.set('views', path.join(__dirname, 'public'));
scope.network.app.use(scope.network.express.static(path.join(__dirname, 'public')));
...
```



The next step is the processing of request parameters and response data, including the filtering of blacklists and whitelists among peers nodes, and finally start the service operation:

```
// line 336
scope.network.server.listen(scope.config.port, scope.config.address, function (err) {
```

### (3) loading logic

As you can see from the code, its core logical functions are account management, transaction and blockchain. These modules, in essence, encapsulate database operations: account corresponds to modules/accounts module, transaction corresponds to modules/transactions module, block corresponds to modules/blocks module, and we will analyze them together when we introduce related modules.

// line 379

```
logic: ['dbLite', 'bus', 'scheme', 'genesisblock', function (cb, scope) {

    // async.auto
    async.auto({
        ...
        account: ["dbLite", "bus", "scheme", 'genesisblock', function (cb, scope) {
            new Account(scope, cb);
        }],
        transaction: ["dbLite", "bus", "scheme", 'genesisblock', "account", function (cb, scope) {
            new Transaction(scope, cb);
        }],
        block: ["dbLite", "bus", "scheme", 'genesisblock', "account", "transaction", function (cb, scope) {
            new Block(scope, cb);
        }]
    }, cb);
    ...

```

### (4) load modules

The execution results of all the above code are shared by modules at this step. As shown in the following code, each module adopts consistent (not necessarily identical) parameters and processing methods, which are simple and convenient to handle:

```
// line 411
modules: ['network', 'connect', 'config', 'logger', 'bus', 'sequence', 'dbSequence', 'balancesSequence', 'dbLite', 'logic', function (cb, scope) {

    // apply `domain` to each module
    Object.keys(config.modules).forEach(function (name) {
        tasks[name] = function (cb) {
            var d = require('domain').create();

            d.on('error', function (err) {
                ...
            });

            d.run(function () {
                ...
            });
        }
    });

    // run each module in parallel
    async.parallel(tasks, function (err, results) {
        cb(err, results);
    });
```

All modules are processed in parallel.



