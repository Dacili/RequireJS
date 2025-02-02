# RequireJS
https://requirejs.org/docs/api.html  

## What is a module in JS? 
Splitting JavaScript programs up into separate modules that can be imported when needed.   
(rather than keeping it in a single file and moving those pieces to other files or projects.)   
Modules can contain functions, variables, and objects that perform specific functions.  

## Modular JS libraries and frameworks: 
#### 1. CommonJS 
is a project to standardize the module ecosystem for JavaScript outside of web browsers.  
CommonJS's specification of how modules should work is widely used today for server-side JavaScript with **Node.js**.
#### 2. AMD (Asynchronous module definition) 
AMD based module systems such as RequireJS.  
AMD is a JavaScript specification that defines an interface for writing and loading modules, such that the module and its dependencies can be asynchronously loaded.
#### 3. UMD  (Universal Module Definition)
one more module system, suggested as a universal one, is compatible with AMD and CommonJS.
#### Newer: 
#### 4. Webpack 
is used to bundle modules and optimize the code.  
When using webpack to bundle your app, you can pick from a variety of module syntax styles including ES6, CommonJS, and AMD.

#### 5. Babel
- is a JavaScript compiler  
- is used to transpile the JavaScript code, making it compatible with older browsers.  
Transpile = process of converting source code from one high-level programming language to another

## Ways to execute JS script
There are several ways an external script can be executed:  
- **async**: The script is downloaded *in parallel* to parsing the page and *executed* as soon as it is available (**before parsing completes**)  
```
<script src="medi.js" async></script>
```  
- **defer**: The script is downloaded *in parallel* to parsing the page, and executed **after parsing (of page) completes**   
```
<script src="medi.js" defer></script>
```  
- **neither async or defer** is present (it's synchronous): The script is downloaded and executed immediately, **blocking parsing until the script is completed**    
```
<script src="medi.js"></script>
```

## What is RequireJS?
RequireJS is a JavaScript *file and module loader*.  
In JavaScript, *module loaders* are tools or mechanisms that allow you to load and manage *modules*.  
*Modules* are reusable pieces of code.  
RequireJS always **loads modules asynchronously**, with no blocking.

## Why to use it?
There are some reasons:  
 • In a large app, a lot of JavaScript files are needed, and *each script tag needs a request*.  
 ```
 <script src='https://unpkg.com/@cometchat/chat-sdk-javascript@4.0.7/CometChat.js' type="text/javascript"></script>
```  
 • You have to put them in a same order in which they are called, i.e. *File which is dependent on other should be loaded after the dependent ones*.  
```
<script src="demo.js"></script>
<script src="dependsOnDemo.js"></script>
``` 
## How to use it?
#### 1. Download the file  
(https://requirejs.org/docs/download.html#requirejs) and name it *require.js*  
#### 2. Use script tag in HTML for require.js file
 ```
 <script src="scripts/require.js"></script>
```
#### 3. **Config** file  
a) Use **default** config:    
If you don't create any config, it will use defaults, from require.js file:  
![image](https://github.com/user-attachments/assets/9b0a0943-019c-45d4-9ad3-930ee3785e10)  
Notice that baseUrl is searching path ./  
**Which means load any module from current folder.**  
Relative paths symbols:  
./ (current folder)  
../ (parent)

b) **Create config file**:  
If you want to create a config file, you could add something like this code to new js file (create something like require-js-config.js).  
Check *Configuration settings* section for more details regarding the config.  
```
   requirejs.config({
    //By default load any module IDs from js/lib
    baseUrl: 'js/lib',
    //except, if the module ID starts with "app", load it from the js/app directory. paths config is relative to the baseUrl,
    // and never includes a ".js" extension since the paths config could be for a directory.
    paths: {
        app: '../app'
    }
   });  
```

#### 4. use **requirejs()** function
##### a) you need 1 module to be loaded for some function
```
requirejs(["medina"], function(exampleParam) {
   // you code stuff is here
   // exampleParam.callFunction();
});
```
- once medina.js file is loaded, the function will be executed
- *The parameter exampleParam in function can be named whatever you like, and it's an object from the module you loaded. So you could now use functions from that module.*

##### b) you need several modules to be loaded
```
requirejs(['jquery', 'canvas'],
function   ($, canvas) {
    //jQuery and canvas modules are all
    //loaded and can be used here now.
});
```
if we require *more modules*, then in the function we will have *as many params*, where the **order will be matched** as in requirejs function  
![image](https://github.com/user-attachments/assets/54266a22-1315-41bc-ae16-b358c2a37ba5)  

If you created a config file, you could call it same way      
```
<script src="scripts/require.js"></script>
<script>
require(['scripts/require-js-config'], function() {
    // Configuration loaded now, safe to do other require calls that depend on that config.
    require(['foo'], function(foo) {
    });
});
</script>
```
The code above technically means:
- Load require.js  
- After that, when require-js-config is loaded, execute the code from the function,
- After that once foo is loaded, proceed with its function.  
#### 5. or you could define your own module with define()
##### a) Define named module without dependencies
 ```
define("medii",
        function(cart, inventory) {
            //Define foo/title object in here.
       }
    );
```
##### b) Define named module with dependencies
define() function allows the module to declare its dependencies before being loaded:   
 ```
 define("medii",
        ["my/cart", "my/inventory"],
        function(cart, inventory) {
            //Define foo/title object in here.
       }
    );
```
The code above defines new module medii, once the dependencies in the array are loaded.   
And then call that module wherever you need it like:  

```
require(["medii"], function(myModule) {
});
```
## requirejs() vs require()
Sometimes we will see one or other use in practice. But technically, **they're the same**.  
They implemented require(), to make it easier to cooperate with other AMD loaders on globally agreed names.  
(https://stackoverflow.com/questions/13605600/requirejs-difference-between-requirejs-and-require-functions)
## Configuration settings: 
### baseUrl
baseUrl: the root path to use for all module lookups  
Load any module from the folder that matches url.  

### data-main attribute
```
<script data-main="scripts/main" src="scripts/require.js"></script>
```
If no baseUrl is explicitly set in the configuration, the default value will be the location of the HTML page that loads require.js.   
If a data-main attribute is used, that path will become the baseUrl (in example, it would be main.js path).  
If you *want to do require()* calls *in the HTML* page, then it is best to *not use data-main*.  
![image](https://github.com/user-attachments/assets/6eec26e2-139e-44e8-a314-81af6600cbe8)  

data-main is only intended for use when the page just has *one main entry point*, the data-main script. 

### paths
paths: path mappings for module names *not found directly under baseUrl*  
The path that is used for a module name should not include an extension (e.g. .js), since the path mapping could be for a directory.  
The path mapping code will automatically add the .js extension when mapping the module name to a path. 

### waitSeconds
waitSeconds: The number of seconds to wait before giving up on loading a script.  
Setting it to 0 disables the timeout. The default is 7 seconds.

## Why in some places there is a data-main attribute in the script tag?
(https://stackoverflow.com/questions/35027046/difference-between-data-main-and-normal-script-loading)  
Script tag with data-main attribute:
```
<script data-main="scripts/main" src="scripts/require.js"></script>
```
or without data-main attribute
```
<script src="scripts/require.js"></script>
```
The first one is equivalent to this:
```
<script src="scripts/require.js"></script>
<script>require(["scripts/main"])</script>
```
Also, the difference is mentioned in *configuration settings* part, for data-main attr.
## Define vs require
- **define**: used to define modules, to use in multiple locations (reuse)
- **require**: load and use existing modules (I want this module + also load all its dependencies)

https://stackoverflow.com/questions/9507606/when-should-i-use-require-and-when-to-use-define  
https://stackoverflow.com/questions/17366073/requirejs-define-vs-require

## Errors 
The error that made me learn more about require js is:  
MISMATCHED ANONYMOUS DEFINE() MODULES (https://requirejs.org/docs/errors.html#mismatch)    
The external library has in their SDK js file, *define* without the name of the module, so that blows require js.  
```
define([],t)
```
https://stackoverflow.com/questions/63793934/how-to-solve-mismatched-anonymous-define-module

