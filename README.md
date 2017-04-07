# api documentation for  [restling (v0.9.1)](https://github.com/lucasfeliciano/restling)  [![npm package](https://img.shields.io/npm/v/npmdoc-restling.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-restling) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-restling.svg)](https://travis-ci.org/npmdoc/node-npmdoc-restling)
#### Restling is a lightweight Node.js module for building promise-based asynchronous HTTP requests.

[![NPM](https://nodei.co/npm/restling.png?downloads=true)](https://www.npmjs.com/package/restling)

[![apidoc](https://npmdoc.github.io/node-npmdoc-restling/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-restling_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-restling/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-restling/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-restling/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Lucas Feliciano",
        "email": "mail@lucasfeliciano.com",
        "url": "http://lucasfeliciano.com/"
    },
    "bugs": {
        "url": "https://github.com/lucasfeliciano/restling/issues"
    },
    "dependencies": {
        "bluebird": "^2.3.11",
        "lodash": "^3.2.0",
        "restler": "^3.2.2"
    },
    "description": "Restling is a lightweight Node.js module for building promise-based asynchronous HTTP requests.",
    "devDependencies": {
        "chai": "^1.9.1",
        "chai-as-promised": "^4.1.1",
        "mocha": "^1.20.1",
        "nock": "^0.59.1"
    },
    "directories": {},
    "dist": {
        "shasum": "bc0dd2441cec2688a4164d1818572a3530f1534f",
        "tarball": "https://registry.npmjs.org/restling/-/restling-0.9.1.tgz"
    },
    "gitHead": "1b23292eb4cce2eacb1eb6fdca0c5351b89196f4",
    "homepage": "https://github.com/lucasfeliciano/restling",
    "keywords": [
        "resquest",
        "http",
        "async",
        "asynchronous",
        "promise",
        "restler",
        "rest",
        "parallel",
        "bluebird"
    ],
    "license": "MIT",
    "main": "restling.js",
    "maintainers": [
        {
            "name": "lucasfeliciano",
            "email": "mail@lucasfeliciano.com"
        }
    ],
    "name": "restling",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/lucasfeliciano/restling.git"
    },
    "scripts": {
        "test": "mocha spec/test.js"
    },
    "version": "0.9.1"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module restling](#apidoc.module.restling)
1.  [function <span class="apidocSignatureSpan">restling.</span>allAsync (myRequests)](#apidoc.element.restling.allAsync)
1.  [function <span class="apidocSignatureSpan">restling.</span>del (args)](#apidoc.element.restling.del)
1.  [function <span class="apidocSignatureSpan">restling.</span>get (args)](#apidoc.element.restling.get)
1.  [function <span class="apidocSignatureSpan">restling.</span>head (args)](#apidoc.element.restling.head)
1.  [function <span class="apidocSignatureSpan">restling.</span>json (args)](#apidoc.element.restling.json)
1.  [function <span class="apidocSignatureSpan">restling.</span>patch (args)](#apidoc.element.restling.patch)
1.  [function <span class="apidocSignatureSpan">restling.</span>post (args)](#apidoc.element.restling.post)
1.  [function <span class="apidocSignatureSpan">restling.</span>postJson (args)](#apidoc.element.restling.postJson)
1.  [function <span class="apidocSignatureSpan">restling.</span>put (args)](#apidoc.element.restling.put)
1.  [function <span class="apidocSignatureSpan">restling.</span>putJson (args)](#apidoc.element.restling.putJson)
1.  [function <span class="apidocSignatureSpan">restling.</span>request (args)](#apidoc.element.restling.request)
1.  [function <span class="apidocSignatureSpan">restling.</span>settleAsync (myRequests)](#apidoc.element.restling.settleAsync)



# <a name="apidoc.module.restling"></a>[module restling](#apidoc.module.restling)

#### <a name="apidoc.element.restling.allAsync"></a>[function <span class="apidocSignatureSpan">restling.</span>allAsync (myRequests)](#apidoc.element.restling.allAsync)
- description and source-code
```javascript
allAsync = function (myRequests) {
  if (_.isArray(myRequests)) {
    return Promise.all(wrapToArray(myRequests));
  }
  else if (_.isObject(myRequests)) {
    return Promise.props(wrapToObject(myRequests));
  }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.restling.del"></a>[function <span class="apidocSignatureSpan">restling.</span>del (args)](#apidoc.element.restling.del)
- description and source-code
```javascript
del = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.restling.get"></a>[function <span class="apidocSignatureSpan">restling.</span>get (args)](#apidoc.element.restling.get)
- description and source-code
```javascript
get = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
...

Basic Usage
-----------

'''javascript
var rest = require('restling');

rest.get('http://google.com').then(function(result){
  console.log(result.data);
}, function(error){
  console.log(error.message);
});
'''
The result passed into the success callback is an object with two keys:
'data' and 'response'.
...
```

#### <a name="apidoc.element.restling.head"></a>[function <span class="apidocSignatureSpan">restling.</span>head (args)](#apidoc.element.restling.head)
- description and source-code
```javascript
head = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.restling.json"></a>[function <span class="apidocSignatureSpan">restling.</span>json (args)](#apidoc.element.restling.json)
- description and source-code
```javascript
json = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.restling.patch"></a>[function <span class="apidocSignatureSpan">restling.</span>patch (args)](#apidoc.element.restling.patch)
- description and source-code
```javascript
patch = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.restling.post"></a>[function <span class="apidocSignatureSpan">restling.</span>post (args)](#apidoc.element.restling.post)
- description and source-code
```javascript
post = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
...
// auto convert xml to object
rest.get('http://twaud.io/api/v1/users/danwrong.xml')
  .then(successCallback, errorCallback);

rest.get('http://someslowdomain.com',{timeout: 10000})
  .then(successCallback, errorCallback);

rest.post('http://user:pass@service.com/action', {
  data: { id: 334 },
}).then(function(result) {
  if (result.response.statusCode == 201) {
    result.data;// you can get at the raw response like this...
  }
},
errorCallback);
...
```

#### <a name="apidoc.element.restling.postJson"></a>[function <span class="apidocSignatureSpan">restling.</span>postJson (args)](#apidoc.element.restling.postJson)
- description and source-code
```javascript
postJson = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
...
    'sound[message]': 'hello from restling!',
    'sound[file]': rest.file('doug-e-fresh_the-show.mp3', null, 321567, null, 'audio/mpeg')
  }
}).then(successCallback, errorCallback);

// post JSON
var jsonData = { id: 334 };
rest.postJson('http://example.com/action', jsonData).then(successCallback, errorCallback);

// put JSON
var jsonData = { id: 334 };
rest.putJson('http://example.com/action', jsonData).then(successCallback, errorCallback);
'''

TODO
...
```

#### <a name="apidoc.element.restling.put"></a>[function <span class="apidocSignatureSpan">restling.</span>put (args)](#apidoc.element.restling.put)
- description and source-code
```javascript
put = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.restling.putJson"></a>[function <span class="apidocSignatureSpan">restling.</span>putJson (args)](#apidoc.element.restling.putJson)
- description and source-code
```javascript
putJson = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
...

// post JSON
var jsonData = { id: 334 };
rest.postJson('http://example.com/action', jsonData).then(successCallback, errorCallback);

// put JSON
var jsonData = { id: 334 };
rest.putJson('http://example.com/action', jsonData).then(successCallback, errorCallback);
'''

TODO
----

* What do you need? Let me know or fork.
...
```

#### <a name="apidoc.element.restling.request"></a>[function <span class="apidocSignatureSpan">restling.</span>request (args)](#apidoc.element.restling.request)
- description and source-code
```javascript
request = function (args){
  var self = this;
  var request = restler[verb].apply(self, arguments);

  return new Promise(function(resolve, reject) {
    request
      .on('success', function(data, response) {
        resolve({'data': data, 'response': response});
      })
      .on('fail', function(data, response) {
        var url = request.url.href,
          method = verb.toUpperCase(),
          errorMessage = 'Cannot ' + method + ' ' + url,
          error = new Error(errorMessage);

        error.statusCode = response.statusCode;
        error.response = response;
        error.data = data;

        reject(error);
      })
      .on('error', function(err) {
        reject(err);
      })
      .on('abort', function() {
        reject(new Promise.CancellationError());
      })
      .on('timeout', function() {
        reject(new Promise.TimeoutError());
      });
  });
}
```
- example usage
```shell
...
  return _.mapValues(myRequests, function(myRequest){
    return functionByMethod(myRequest);
  });
}

function functionByMethod(myRequest) {
  if (_.has(myRequest, 'options') && _.has(myRequest.options, 'method')) {
    return exports.request(myRequest.url, myRequest.options);
  }
  return exports.get(myRequest.url, myRequest.options);
};
...
```

#### <a name="apidoc.element.restling.settleAsync"></a>[function <span class="apidocSignatureSpan">restling.</span>settleAsync (myRequests)](#apidoc.element.restling.settleAsync)
- description and source-code
```javascript
settleAsync = function (myRequests) {

  var wrapped = wrapToArray(myRequests);
  if (_.isArray(myRequests)) {
    return Promise.settle(wrapped).then(function(settledValues){
      return getSettledValues(settledValues);
    });
  }
  else if (_.isObject(myRequests)) {
    var keys = _.keys(myRequests);

    return Promise.settle(wrapped).then(function(settledValues) {
      var resolved = _.zipObject(keys, getSettledValues(settledValues));
      return  Promise.resolve(resolved);
    });
  }
}
```
- example usage
```shell
...


'''javascript
var rest = require('restling');
var obj  = {'google':{'url':'http://google.com'},
            'api':{'url':'http://some/rest/api'}}

rest.settleAsync(obj).then(function(result){
  // handle result here
  // result is {google: responseFromGoogle, api: responseFromApi}
},function(err){
  // handle error here
});
'''
#### Passing an array
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
