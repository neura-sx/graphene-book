# Interacting with a witness node
### Prerequisites

We assume  you have the `witness_node` already compliled (or downloaded from [the offical respository](https://github.com/bitshares/bitshares-2/releases/latest)).

Apart from that you will need these tools installed:

* [Nodejs 4.2.6](https://nodejs.org/dist/v4.2.5/node-v4.2.5-x64.msi)
> Version 5.5.x might work as well but I haven't tried using it.

* [Python 2.7.11](https://www.python.org/ftp/python/2.7.11/python-2.7.11.msi)
> Make sure it's Python 2.7.x not 3.5.x.  
Also, use the custom installation and allow to add python to your system path.

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

### Two types of RPC calls
We have two types of witness node API calls:
* Unrestricted calls: these are stateless calls.

* Restricted calls: calls that are restricted by default (`network_node_api`) or have been restricted by configuration are not accessible via RPC because a statefull protocol (web socket) is required.

### Run unrestricted API calls
An unrestricted API call against the witness node looks like this:
```
{"id":888, "method":"call", "params":[0,"get_accounts",[["1.2.0"]]]}  
```
> The number `888` is just a random identifier, you can use whatever value you want.

### Run restricted API calls
As for the restricted API calls, the first thing we need to do is to log in.
```
{"id":888,"method":"call","params":[1,"login",["",""]]}
```
...and you should receive a response similar to this:
```
{"id":2,"result":true}
```
...which gives a positive confirmation about your log-in attempt.

> You may be required to put your username and pasword into the quotes. In our case we have used empty values in the form of `""`.

Before we can subscribe to any object changes and get notified automatically, we first need to ask for access to the database API with this command:
```
{"id":888,"method":"call","params":[1,"database",[]]}  
```
You will receive a database API id in this format:
```
{"id":888,"result":2}
```
So in this case the database API id is `2` but it might be different in your case. You need to use this id, by replacing the `DATABASE_API_ID` placeholder, when you run the subsequent calls:
```
{"id":888,"method":"call","params":[DATABASE_API_ID,"get_config",[]]}  
{"id":888,"method":"call","params":[DATABASE_API_ID,"get_chain_id",[]]}  
{"id":888,"method":"call","params":[DATABASE_API_ID,"list_assets",["",10]]}  
```