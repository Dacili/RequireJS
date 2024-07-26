# RequireJS
https://requirejs.org/docs/api.html  

## What is RequireJS?

## When to use it?

## How to use it?

## Configuration settings: 
### baseUrl
baseUrl: the root path to use for all module lookups

### data-main attribute
If no baseUrl is explicitly set in the configuration, the default value will be the location of the HTML page that loads require.js.   
If a data-main attribute is used, that path will become the baseUrl.  

### paths
paths: path mappings for module names not found directly under baseUrl  
The path that is used for a module name should not include an extension (e.g. .js), since the path mapping could be for a directory.  
The path mapping code will automatically add the .js extension when mapping the module name to a path. 

### waitSeconds
waitSeconds: The number of seconds to wait before giving up on loading a script.  
Setting it to 0 disables the timeout. The default is 7 seconds.
