new info of ngsijs
==================

This library has been modified to work with iot-broker


More info about iotbroker [Install IOTbroker info] and [historyqueries info]

[Install IOTbroker info]: http://fiware-iot-broker.readthedocs.io/en/latest/docker/index.html
[historyqueries info]: http://fiware-iot-broker.readthedocs.io/en/latest/historyqueries/index.html

And only 2 querys of v1 are tested

connection.v1.query
and 
connection.v1.updateAttributes


### Examples
--------
#### query
```javascript

var connection = new NGSI.Connection("http://iotbroker.example.com:8060");
connection.v1.query([
    { type: 'Room', id: 'Room.*', isPattern: true }
],
    ['temperature'],
    {
        restriction: {
            "attributeExpression": "",
            "scopes": [{
                "scopeType": "ISO8601TimeInterval",
                "scopeValue": "2015-08-18T16:54:00+0000/2018-08-18T16:55:00+0000"
            }]
        },
        limit: 100,
        offset: 200,
        details: true,
        onSuccess: function (data) {
            console.log("OK")
            console.log(JSON.stringify(data))
        },
        onFailure: function (data) {
            console.log("ERROR")
            console.log(JSON.stringify(data))
        }
    }
);
```

#### update
```javascript

var connection = new NGSI.Connection("http://iotbroker.example.com:8060");
var today = new Date();
var dd = today.getDate();
var mm = today.getMonth() + 1; //January is 0! 
var yyyy = today.getFullYear();
if (dd < 10) { dd = '0' + dd }
if (mm < 10) { mm = '0' + mm }
var h = today.getHours();
var m = today.getMinutes();
var s = today.getSeconds();

var today = yyyy + "-" + mm + "-" + dd + " " + h + ":" + m + ":" + s;

    connection.v1.updateAttributes([
        {
            'entity': { type: 'Room', id: 'Room1' },
            "attributes": [
                {
                    "name": "temperature",
                    "type": "float",
                    "value": "26.5",
                    "metadata": [{
                        "name": "date",
                        "type": "date",
                        "value": ""+today
                    }]
                }
            ]
        }
    ], {
            onSuccess: function (data) {
                console.log(data)
            },
            onFailure: function (data) {
                console.log(data)
            }
        }
    );


```

none of v2 functions will work because v2 is not implemented on iotbroker



old info ngsijs
===============

[![License](https://img.shields.io/badge/license-AGPLv3+%20with%20classpath--like%20exception-blue.svg)](LICENSE)
[![Documentation Status](https://img.shields.io/badge/docs-stable-brightgreen.svg?style=flat)](https://conwetlab.github.io/ngsijs/stable)
[![Build Status](https://travis-ci.org/conwetlab/ngsijs.svg?branch=master)](https://travis-ci.org/conwetlab/ngsijs)
[![Coverage Status](https://coveralls.io/repos/github/conwetlab/ngsijs/badge.svg?branch=master)](https://coveralls.io/github/conwetlab/ngsijs?branch=master)

Ngsijs is the JavaScript library used by
[WireCloud](https://github.com/Wirecloud/wirecloud) for adding FIWARE NGSI
capabilities to widgets and operators. However, this library has also been
designed to be used in other environments as normal web pages and
clients/servers running on Node.js.

This library has been developed following the FIWARE
[NGSI v1](http://telefonicaid.github.io/fiware-orion/api/v1/) and
[NGSI v2](http://fiware.github.io/specifications/ngsiv2/stable/) specifications
and has been tested to work against version 0.26.0+ of the Orion Context Broker.

Reference documentation of the API is available at
[http://conwetlab.github.io/ngsijs/stable/NGSI.html](http://conwetlab.github.io/ngsijs/stable/NGSI.html).


Using ngsijs from normal web pages
----------------------------------

Just include a `<script>` element linking to the `NGSI.min.js` file:

```html
<script type="text/javascript" src="url_to_NGSI.js"></script>
```

Once added the `<script>` element, you will be able to use all the features
provided by the ngsijs library (except receiving notifications):

```javascript
var connection = new NGSI.Connection("http://orion.example.com:1026");
connection.v2.listEntities().then((result) => {
    response.results.forEach((entity) => {
        console.log(entity.id);
    });
});
```

This example will display the `id` of the first 20 entities. See the
documentation of the [`listEntities`](http://conwetlab.github.io/ngsijs/stable/NGSI.Connection.html#.%22v2.listEntities%22__anchor)
method for more info.

To be able to receive notifications inside a web browser the library requires
the use of a [ngsi-proxy](https://github.com/conwetlab/ngsi-proxy) server. You
can use your own instance or you the `ngsi-proxy` available at
`https://ngsiproxy.lab.fiware.org`.

```javascript
var connection = new NGSI.Connection("http://orion.example.com:1026", {
    ngsi_proxy_url: "https://ngsiproxy.lab.fiware.org"
});
```

Using ngsijs from Node.js
-------------------------

```bash
$ npm install ngsijs
```

After installing the ngsijs node module, you will be able to use the API as usual:

```javascript
var NGSI = require('ngsijs');
var connection = new NGSI.Connection("http://orion.example.com:1026");
```

**Note:** Node.js doesn't require the usage of a ngsi-proxy as you can create
an HTTP endpoint easily (e.g. using express). Anyway, you can use it if you
want, you only have to take into account that is better to directly provide the
HTTP endpoint to reduce the overhead.


Using ngsijs from WireCloud widgets/operators
---------------------------------------------

Take a look to the "3.2.1. Using Orion Context Broker" tutorial available at
[FIWARE Academy].

[FIWARE Academy]: http://edu.fiware.org/course/view.php?id=53#section-3

