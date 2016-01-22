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
* Unrestricted API-0 calls: these are stateless calls, as API-0 is meant for stateless querying.

* Restricted API-1 calls: these are statefull calls, as API-1 is meant for authencitated interaction where a statefull protocol (web socket) is required.

### Run unrestricted API calls
Unrestricted API-0 calls have an API identifier equal to `0`.  
Below is an example of unrestricted API call :
```
{"id":888, "method":"call", "params":[0,"get_accounts",[["1.2.0"]]]}  
```
> The number `888` is a request identifier which can have whatever value you want, as opposed to the value `0` (i.e. the first item in `params`) which specifies the API identifier.

### Run restricted API calls
As for the restricted API-1 calls, for security reasons, the witness node distinguishes five different APIs:
* Login API
* Database API
* Account History API
* Network Broadcast API
* Network Nodes API

The Login API implements the bottom layer of the RPC API and it has an API identifier equal to `1`.   
All other APIs must be requested from this API so the first thing we need to do is log in:
```
{"id":888,"method":"call","params":[1,"login",["",""]]}
```
...and you should receive a response similar to this:
```
{"id":888,"result":true}
```
...which gives a positive confirmation about your log-in attempt.

> You may be required to put your username and pasword into the quotes. In our case we have used empty values in the form of `""`. Also, note the value `1` (i.e. the first item in `params`) which specifies the API identifier of the Login API.

### Access the Database API

Let's now ask the Login API to give us access to the Database API. We do it with this command:
```
{"id":888,"method":"call","params":[1,"database",[]]}  
```
You will receive a database API identifier in this format:
```
{"id":888,"result":2}
```
So in this case our Database API identifier is eqaul `2` but it might be different in your case. You need to use this identifier, by replacing the `DATABASE_API_ID` placeholder, when you run the following API calls:
```
{"id":888,"method":"call","params":[DATABASE_API_ID,"get_config",[]]}  
{"id":888,"method":"call","params":[DATABASE_API_ID,"get_chain_id",[]]}  
{"id":888,"method":"call","params":[DATABASE_API_ID,"list_assets",["",10]]}  
```

