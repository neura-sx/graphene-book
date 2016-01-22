# Interacting with a witness node
### Prerequisites


---

Install Nodejs  
https://nodejs.org/dist/v4.2.5/node-v4.2.5-x64.msi

---

Install Python 2.7 (not 3.5!)  
https://www.python.org/ftp/python/2.7.11/python-2.7.11.msi  
Custom: add to path

---

npm install -g wscat

---

wscat -c ws://127.0.0.1:11011

---

{"id":888, "method":"call", "params":[0,"get_accounts",[["1.2.0"]]]}  

{"id":888,"method":"call","params":[1,"login",["",""]]}  
{"id":888,"method":"call","params":[1,"database",[]]}  

{"id":888,"method":"call","params":[DATABASE_API_ID,"get_config",[]]}  
{"id":888,"method":"call","params":[DATABASE_API_ID,"get_chain_id",[]]}  
{"id":888,"method":"call","params":[DATABASE_API_ID,"list_assets",["",10]]}  



http://docs.bitshares.eu/api/websocket.html