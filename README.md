##Janus

Janus is a fake rest api server which can be used for various purpose including frontend application development , testing etc.

### Features

* Easy to configure and use.
* Supports all http methods and REST resources.
* Supports CORS.
* Basic Auth Support
* Available in all platforms.


### Install

##### Get Binary Distribution

You can download the latest binary distribution from [here](https://github.com/jijeshmohan/janus/releases/tag/1.1.0)

##### Build from source

[Go](https://golang.org/dl/) should be installed (version **1.4+** is required) in the system. Make sure you have Go properly installed, including setting up your [GOPATH](http://golang.org/doc/code.html#GOPATH).

```go get -u github.com/jijeshmohan/janus```

### How to run

To run janus, go to any directory and create a json file called ```config.json``` . This defines the REST endpoints which server need to expose. ( config.json file described in the next section.). Run the application in the same directory by typing `janus` in the terminal


Note : A sample config and associated files are there in example directory.


### Basic Structure of a config.json file

The basic structure of config file shown below. A config file has follwing attributes
* port
* enableLog
* delay
* auth
* jwt
* resources
* urls
* static 

Detailed description of each attributes are below.

e.g config.json

```json
{
  "port": 8080,
  "enableLog": false,
  "delay": 0,
  "auth": {
    "username": "user1",
    "password": "secret"
  },
  "resources": [
    {
      "name": "user",
      "headers": {
        "key": "value"
      }
    }
  ],
  "urls": [
    {
      "url": "direct/url/for/something",
      "method": "GET",
      "content_type": "application/json",
      "status": 200,
      "file": "./files/some.json",
      "headers": {
        "key": "value",
        "key1": "value1"
      }
    }
  ]
}
```

##### Port

**port** in configuration file defines in which port the server needs to run.This is an optional field and if not provided the default port is 8000.



##### EnableLog

**enableLog** in configuration file defines whether we need to log the requests form clients. Default values is false.


##### Delay

**delay** in configuration file provides a response delay ( in milliseconds). Default value is 0 which means there is no delay. This feature helps to simulate network delay or server response time.

##### Auth

**auth** in configuration provides basic auth support in janus. This is an optional field. If provided this will verify the username and password for all requests.

You need to specify username and password like below

```js
"auth": {
  "username": "user1",
  "password": "password"
}
```

##### JWT

**jwt** in configuration provides jwt support in janus. This will issue a new jwt token when they call the url specified in jwt section below.


```js
"jwt": {
  "url": "/auth/token",
  "exp:" 12, // expiry in minutes 
  "secret": "secret key"
  "data": {
    "userid": 1234,
    "admin": false
  }
}
```

##### Static 

**staic** in configuration allows to serve static files in janus. With this option you can make janus to serve static files for your api ( e.g. javascript frontend like angular, reactjs etc..)

```js
"static": {
	"url": "/ui",
	"path": "public"
}
```
In above configuration, url specifies what would be the root path of your static files , like `http://localhost:8080/ui/index.html` and `path` specified the actual directory from which teh static files need to be served. This is relative to the configuration files directory. 


##### REST enspoints

You can define two types of REST endpoints in the configuration file

* resources
* urls

##### Resources


This represent basic REST resource which will exposes all [standard methods](http://restful-api-design.readthedocs.org/en/latest/methods.html#standard-methods). Janus will look for a folder with the name of the resource in the same directory as routes.json for sending the data correspoding to the methods.

e.g:

```js
{
	"name": "user"
	"headers": {  // headers field is optional
		"key": "value"
	}
}
```

e.g for user resource , it wil look follwoing files to send the data

| Verb | Url | FILE | Description |
|--------|--------|---|---|
| GET       | /user       |  ./user/index.json  | if the file not available app will send a 404|
| POST       | /user       |  ./user/post.json  | if file is present it will send 201 with the content of the file otherwise 404|
|GET| /user/:item |./user/[any file name which is matching :item].json | if the file present, it will send the file with 200, otherwise 404. you can add item1.json, item2.json etc if you want to fake different get request
| PUT | /user/:item |  will use the same file specified above | if file is present it will send 200 with the content of the file otherwise 404. You can specify any number of files to match the get requst like  user1.json, admin.json etc.
| PATCH | /user/:item | will use the same file specified above | if file is present it will send 200 with the content of the file otherwise 404.
| DELETE | /user/:item | will use the same file specified above | if file is present it will send 200 with the content of the file otherwise 405. If you have specified item1.json, then it will send 200 for that particular item's request.

You can specify any header informations which need to send along with the methods. This is optional field

##### Urls
Urls section is for specifying individual urls which can't qualify for a standard REST resource methods.

This section gives more freedom in terms for defining HTTP Methods and content type and files.

A single url representasion showed below

```js
 {
   "url": "/admin/user/enable", //mandatory
   "method": "GET", // mandatory
   "content_type": "application/json", // optional. default to application/json; charset=utf-8
   "status": 200, // optional , default to 200
   "file": "./files/some.json" // optional, if not specified , the response will be empty string. if specified it should be a valid file.
   "headers": {  //Optional
    "key": "value",
    "key1": "value1"
   }
}
```

Note: Url also support dynamic url like ```/admin/{user}/enable``` which can match to any  user name like ```/admin/user1/enable``` or ```/admin/anotheruser/enable``` .

### TODO

* File upload support
* Compression
* Websocket support.
* Admin UI for configuration and adding url at runtime.

### Changelog

Please check the changelog [here](https://github.com/jijeshmohan/janus/blob/master/CHANGELOG)

### License

The MIT License.
