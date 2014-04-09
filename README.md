http-static-gzip-regexp
=======================

Middleware for using pre-compressed gzip files based on regexp url pattern for Node.js

Serves compressed files for the given regexp url pattern. If the url requested matches the regular expression and the client accepts gzip encoding the the url is appended with ".gz" and the necessary headers are added.

Dynamic compression hurts performance of the server. Let the build tool handle compression, for example with grunt and the compress plugin: https://github.com/gruntjs/grunt-contrib-compress
Use a similar regexp used during build time to load the compressed file just for files that are static. Not checking upfront if the .gz file exists removes the need to go to the os/disk for each request.

This solution can be used with any static server as long as this middleware is added first. This means it can be used with express static https://www.npmjs.org/package/express - but also with servers that supports caching like https://www.npmjs.org/package/express-static-cache and https://www.npmjs.org/package/st

You can still add dynamic compression for files that you don't have a static version for, just add the dynamic compression middleware layer after this one (ex. app.use(express.compress()))

## Installation
	  $ npm install connect-gzip-static
	  
## Usage
```javascript
staticGzip =  require('http-static-gzip-regexp')

//Add the middleware express way:
app.use(staticGzip(/(\.html|\.js|\.css)$/));

//Add dynamic compression if you don't handle all files static
app.use(express.compress());

//Add a static web handler you prefer
app.use(express.static('path'));
```

## Differences with similar modules:
- https://www.npmjs.org/package/connect-gzip-static
Checks if the .gz file exists on disk, requires extra call to the OS for each request. This is the server itself, can't be used with a server of choice.

- http://pmuellr.blogspot.be/2013/11/gzip-encoding-and-compress-middleware.html
Same idea but not yet in npm module. Also tested this repository with the wrk benchmarking tool. Benchmark according for you're own usecase: https://github.com/wg/wrk 

# License
MIT

