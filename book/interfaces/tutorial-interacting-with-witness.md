# Interacting with a witness node
### Prerequisites

* We assume that you have the `witness_node` already compliled (or downloaded from [the offical respository](https://github.com/bitshares/bitshares-2/releases/latest)).

* Nodejs 4.2.5
https://nodejs.org/dist/v4.2.5/node-v4.2.5-x64.msi

* Python 2.7.11 (not 3.5.x!)  
https://www.python.org/ftp/python/2.7.11/python-2.7.11.msi  
> Custom installation: allow to add python to your system path.

### Install wscat
Open a *Command Prompt* window and install `wscat`:
```
npm install -g wscat
```

### Run the witness node
Use this command to start the witness node:
```
witness_node --rpc-endpoint 127.0.0.1:11011
```
> You can choose any port you like, e.g. `8090` or `11011`.

### Run wscat
Keep your witness node running and in another *Command Prompt* window start `wscat` and connect it to the web socket of the witness node:
```
wscat -n -c ws://127.0.0.1:11011
```
> Make sure you use the same port as the one specified for the witness node.

### Run API calls
```
{"id":888, "method":"call", "params":[0,"get_accounts",[["1.2.0"]]]}  
```
```
{"id":888,"method":"call","params":[1,"login",["",""]]}  
{"id":888,"method":"call","params":[1,"database",[]]}  
```
```
{"id":888,"method":"call","params":[DATABASE_API_ID,"get_config",[]]}  
{"id":888,"method":"call","params":[DATABASE_API_ID,"get_chain_id",[]]}  
{"id":888,"method":"call","params":[DATABASE_API_ID,"list_assets",["",10]]}  
```


http://docs.bitshares.eu/api/websocket.html