# Creating a multi-sig account
### Prerequisites
* We assume that you have `cli_wallet` running and connected to an exiting witness node.

* We assume that you have set up a wallet in the CLI and imported a private key of a LTM account with some BTS funds in it. We will refer to this account as `base account`.

* We assume you already created two other accounts that will act as the milti-sig approving accounts. We will refer to these accounts as `approving-account-1` and `approving-account-2`.


### Get account IDs
Run this command:
```
get_account <base-account-name>
```
And the response will be similar to this:
```
{
  "id": "1.2.41",
  "membership_expiration_date": "1969-12-31T23:59:59",
  ...
}
```
Thus in this case the initial account's ID is `1.2.41`. Naturally, yours will be different.  
Similarly, find out the account IDs of the approving accounts:
```
get_account <approving-account-1-name>
get_account <approving-account-2-name>
```


### Get keys for the multi-sig account
To generate a new pair of public and pirvate keys, we'll run this command:
```
suggest_brain_key
```
And the response will look similar to this:
```
{
  "brain_priv_key": "SAPHENA AMUSEE UNSOUR IMPOT BESHOD CHATTER IMPIOUS LINEA COGROAD DECORUM GLOVEY PICUL GLUM FORTIN SPECUS MELOS",
  "wif_priv_key": "5HqjT9ZeDPNGmnAEfc7SPM9QKtP32PnWtS2st6XrWLHep7b69pi",
  "pub_key": "BTS74pa6mL6a4FZpLrnU66KBz2nb8Cfio9qT3uLZCM1zxvbWQaJga"
}
```
Thus in this case the public key is `BTS74pa6mL6...bWQaJga` and the private key is `5HqjT9Ze...LHep7b69pi`. Naturally, yours will be different.

### Initialize transaction builder
We start be initializing the transaction builder by using this command:
```
begin_builder_transaction
```
As a response you'll recieve an ID of the builder process - let's call it `<builder-handle-ID>`.

### Define the multi-sig account
We will now define the multi-sig account we aim to create.  
Our variables include:  
`<base-account-ID>` - the id of your base account, e.g. `1.2.41`  
`<muliti-sig-account-name>` - the name of the multi-sig account to be created, e.g. `nathan-multi-sig`  
`<muliti-sig-account-public-key>` - the public key of the multi-sig account to be created, e.g. `BTS74pa6mL6...bWQaJga`  
`<approving-account-1-ID>` - the id of the first approving account, e.g. `1.2.129`  
`<approving-account-2-ID>` - the id of the second approving account, e.g. `1.2.130`  

We assume we want the multi-sig account to be controlled by two approving accounts, each of them has 50% control, and the vote required is 100%.

Run this command to define the new multi-sig account:
```
add_operation_to_builder_transaction <builder-handle-ID> [ 5, { \
"registrar": "<base-account-ID>", \
"referrer": "<base-account-ID>", \
"referrer_percent": 0, \
"name": "<muliti-sig-account-name>", \
"owner": {"weight_threshold": 1, \
"account_auths": [], \
"key_auths": [["<muliti-sig-account-public-key>", 1]], \
"address_auths": [] }, \
"active": {"weight_threshold": 100, \
"account_auths": \
[["<approving-account-1-ID>",50], \
["<approving-account-2-ID>",50]], \
"key_auths": [], "address_auths": [] }, \
"options": { "memo_key": \
"<muliti-sig-account-public-key>", \
"voting_account": "<base-account-ID>", \
"num_witness": 0, "num_committee": 0, \
"votes": [], "extensions": [] }, "extensions": []} ]
```

### Pay the transaction fee
To pay the transaction fee associated with creating the multi-sig account, run this command: 
```
set_fees_on_builder_transaction <builder-handle-ID> BTS
```

### Sign the builder transaction
To complete the process and sign and broadcast the above transactions, run this command:
```
sign_builder_transaction 0 true
```
Your new multi-sig account is now created.