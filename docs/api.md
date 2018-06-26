# API

The ixo platform has a number of different components that each has a set of APIs

## ixo module API
### Install using npm

`npm install --save ixo-module`

###Create new Ixo Object (Without provider)

```
import Ixo from 'ixo-module';
var ixo = new Ixo('ixo_node_url')
```

* TODO *

## Keysafe Browser Extension API

* Todo *

## Project Datastore API

### Introduction
The project data store holds the data relating to projects.  The API is split into a public API and a non public API. The public API requests do not require cryptographic signatures, while all other requests must be signed and adher to the capabilities that have been granted to the signer.

### Public API
 *Todo

 Upload an image

 Fetch image

 Upload a Json file

 Fetch Json file*

### Private API

The ixo project data store (pds) uses JSON-RPC to receive client requests.  The structure of all calls follow the same structure:

URI: `<pds server>/api/request`

Request type: `POST`

Structure: 
`{"jsonrpc": "2.0", "method": "<method name>", 	"id": <message id>, 	"params": <json data object> }`

| Variable             | Description            |
|----------------------|-----------------------|
| `<node server>`      | The URL of the server |
| `<entity>`           | The entity to send the method |
| `<method name>`      | The name of the method to call defined in the config file |
| `<message id>`       | The message ID, used to correlate asynchronous responses |
| `<json data object>` | The parameters that are passed to the method handler |

### Structure of params object

Everything in the payload section is signed to create a signature.  It should be packed using `JSON.stringify()` method before signing. 

```
{
	"jsonrpc":"2.0", 
	"method":"createAgent",
	"id": 123,
	"params": {
		"payload": {
			
            "template": {
                "name": "create_agent"
            },
            "data": {"projectDid": "did:ixo:TknEju4pjyRQvVehivZ82x",
            		 "name": "Brennon",
            		 "surname": "Hampton",
            		 "email": "brennon@me.com",
            		 "agentDid": "did:sov:64",
            		 "role": "SA"}
        },
        "signature": {
            "type": "ed25519-sha-256",
            "created": "2018-06-27T16:02:20Z", 
            "creator": "did:sov:2p19P17cr6XavfMJ8htYSS",
            "signatureValue": "A011D11A2D91A9CB03ECFFB7D9AFC1001DB56B3DABF42BDD0F4D00352A9B8E0E73E85F0B4586DA2934696C0A78602EEB047EA6B3D9096C1A0C3FB144E6A51C09"
        }
    }
}

```

#### Create Project

Creates a new project.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "createProject", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <create project data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <project data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "_id": "5a66e09b38f45f01d90d122a",
    }
}
```

#### Create Agent

Creates a new agent.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "createAgent", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <create agent data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <agent data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "_id": "5a66e09b38f45f01d90d122a",
        "version": 1
    }
}
```

#### Update Agent Status

Update Agent Status

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "updateAgentStatus", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <update agent data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <agent data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "did": <creator's did>
        "_id": "5a66e09b38f45f01d90d122a",
        "version": 1
    }
}
```

#### List Agents

List claims and latest status.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "listAgents", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <data to filter>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": [
        {
            <agent data>,
            "currentStatus": {
                <agent status data>
            }
        }
    ]
}
```

#### Submit Claim

Creates a new claim.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "submitClaim", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <usubmit claim data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <claim data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "_id": "5a66e09b38f45f01d90d122a"
    }
}
```

#### Evaluate Claim

Evaluate a new claim.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "evaluateClaim", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <evaluate claim data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <claim data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "_id": "5a66e09b38f45f01d90d122a",
        "version": 1
    }
}
```

#### List Claim

List claims and latest status.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "listClaims", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <data to filter>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": [
        {
            <claim data>,
            "evaluations": {
                <claim status data>
            }
        }
    ]
}
```

## ixo Blockchain API

* TODO *

## ixo Explorer API

* TODO *
