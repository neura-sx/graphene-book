# Creating a multi-sig account

### Prerequisites
* We assume that you have `cli_wallet` running and connected to an exiting witness node.

* We assume that you have set up a wallet in the CLI and imported the active private key of a LTM account with some BTS funds in it. We will refer to this account as `your-base-account`.

* We assume that you have already created and registered two other accounts, which will act as the multi-sig approving accounts. We will refer to these accounts as `approving-account-1` and `approving-account-2`.


### Get account IDs
To get the ID of `your-base-account`, run this command:
```
get_account <your-base-account-name>
```
The response should be similar to this:
```
{
  "id": "1.2.41",
  "membership_expiration_date": "1969-12-31T23:59:59",
  ...
}
```
Thus in this case `your-base-account`'s ID is `1.2.41`. Naturally, yours will be different.

> Make sure `membership_expiration_date` is `1969-12-31T23:59:59`, which should be the case, if `your-base-account` is LTM.

Similarly, find out the account IDs of the two approving accounts:
```
get_account <approving-account-1-name>
get_account <approving-account-2-name>
```
They do not need to be LTM.


### Get keys for the multi-sig account
To generate a new pair of public and pirvate keys, run this command:
```
suggest_brain_key
```
The response should look similar to this:
```
{
  "brain_priv_key": "SAPHENA AMUSEE UNSOUR IMPOT BESHOD CHATTER IMPIOUS LINEA COGROAD DECORUM GLOVEY PICUL GLUM FORTIN SPECUS MELOS",
  "wif_priv_key": "5HqjT9ZeDPNGmnAEfc7SPM9QKtP32PnWtS2st6XrWLHep7b69pi",
  "pub_key": "BTS74pa6mL6a4FZpLrnU66KBz2nb8Cfio9qT3uLZCM1zxvbWQaJga"
}
```
Thus in this case, the public key is `BTS74pa6mL6...bWQaJga` and the private key is `5HqjT9Ze...LHep7b69pi`. Naturally, yours will be different. This key pair will be used in the next steps when you define the multi-sig account and then import it to your wallet.

### Initialize transaction builder
We start by initializing the transaction builder with this command:
```
begin_builder_transaction
```
As a response, you'll receive an ID of the builder process - let's call it `<builder-handle-ID>`. This ID will be used in all subsequent commands involving the transaction builder.

### Define the multi-sig account
We will now define the multi-sig account we aim to create. Our variables are as follows:  
* `<your-base-account-ID>` - the ID of `your-base-account`, e.g. `1.2.41`,  
* `<multi-sig-account-name>` - the name of the multi-sig account to be created, choose any name you want,  
* `<multi-sig-account-public-key>` - the public key of the multi-sig account to be created (use the public key obtained with the `suggest_brain_key` command described above),  
* `<approving-account-1-ID>` - the ID of the first approving account, e.g. `1.2.129`,  
* `<approving-account-2-ID>` - the ID of the second approving account, e.g. `1.2.130`,
* `<approving-account-1-power>` - the approval power of the first approving account, e.g. `50`,
* `<approving-account-2-power>` - the approval power of the second approving account, e.g. `30`,
* `<multi-sig-threshold>` - threshold required to access the funds, e.g. `75`.

> Note that the sum of `<approving-account-1-power>` and `<approving-account-2-power>` has to be equal or greater than `<multi-sig-threshold>`.

Run the `add_operation_to_builder_transaction` command to define our new multi-sig account:
```
add_operation_to_builder_transaction <builder-handle-ID> [ 5, { \
"registrar": "<your-base-account-ID>", \
"referrer": "<your-base-account-ID>", \
"referrer_percent": 0, \
"name": "<muliti-sig-account-name>", \
"owner": {"weight_threshold": 1, \
"account_auths": [], \
"key_auths": [["<muliti-sig-account-public-key>", 1]], \
"address_auths": [] }, \
"active": {"weight_threshold": <multi-sig-threshold>, \
"account_auths": \
[["<approving-account-1-ID>",<approving-account-1-power>], \
["<approving-account-2-ID>",<approving-account-2-power>]], \
"key_auths": [], "address_auths": [] }, \
"options": { "memo_key": \
"<muliti-sig-account-public-key>", \
"voting_account": "<your-base-account-ID>", \
"num_witness": 0, "num_committee": 0, \
"votes": [], "extensions": [] }, "extensions": []} ]
```
> Note that the value `5`, used in the first line, is specific for the `account_create_operation`, which we need for this purpose.

### Pay the transaction fee
To create the required transaction fee, associated with creating the multi-sig account, run this command: 
```
set_fees_on_builder_transaction <builder-handle-ID> BTS
```

### Sign the builder transaction
To complete the process, i.e. sign and broadcast the above builder transactions, run this command:
```
sign_builder_transaction <builder-handle-ID> true
```
If you receive no error, it means your new multi-sig account has been successfully created.

### Import the multi-sig account
To import the newly created multi-sig account into your wallet, run this command:
```
import_key <multi-sig-account-name> <multi-sig-account-private-key>
```
> Note that the private key to be used here comes from the `suggest_brain_key` command described above.

### Try it out
Transfer some funds to the new multi-sig account:
```
transfer <your-base-account-name> <multi-sig-account-name> 10000000 BTS "here is some cash" true
```
Now let's try to pay out some funds from the multi-sig account (make sure it's less than the amount received, so there are funds left to cover the transfer fee):
```
transfer <multi-sig-account-name> <your-base-account-name> 300000 BTS "paying back" true
```
As a result, you should get an error indicating that funds cannot be moved:
```
0 exception: unspecified
3030001 tx_missing_active_auth: missing required active authority
Missing Active Authority 1.2.132
```
This is the expected outcome, as we are dealing with a multi-sig account. In order to pay out any funds from it, we need to propose a transfer instead, and have it approved by `approving-account-1` and / or `approving-account-2`.