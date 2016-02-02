# Updating an account to multi-sig
### Prerequisites
* We assume that you have `cli_wallet` running and connected to an exiting witness node.

* We assume that you have set up a wallet in the CLI and imported the active private key of an account with some BTS funds in it. We will refer to this account as `your-base-account`.

* We assume that you have already imported the active private key of a account, which you want to turn into a multi-sig account. We will refer to this account as `future-multi-sig-account`. Also, we'll need some BTS funds on this account to cover transaction fees.

* We assume that you have already created and registered two other accounts, which will act as the milti-sig approving accounts. We will refer to these accounts as `approving-account-1` and `approving-account-2`.

### Get account IDs
To get the account IDs of `future-multi-sig-account`, `approving-account-1` and `approving-account-2`, run these commands:
```
get_account <future-multi-sig-account-name>
get_account <approving-account-1-name>
get_account <approving-account-2-name>
```
We will need those IDs in subsequent steps.

### Initialize transaction builder
We start by initializing the transaction builder with this command:
```
begin_builder_transaction
```
As a response, you'll recieve an ID of the builder process - let's call it `<builder-handle-ID>`. This ID will be used in all subsequent commands involving the transaction builder.

### Add update to the builder
We will now define the account update operation we want to execute. Our variables are as follows:  
* `<future-multi-sig-account-ID>` - the ID of `future-multi-sig-account` ,  
* `<approving-account-1-ID>` - the ID of the first approving account, e.g. `1.2.129`,  
* `<approving-account-2-ID>` - the ID of the second approving account, e.g. `1.2.130`,
* `<approving-account-1-power>` - the approval power of the first approving account, e.g. `50`,
* `<approving-account-2-power>` - the approval power of the second approving account, e.g. `30`,
* `<multi-sig-threshold>` - threshold required to access the funds, e.g. `75`.

Run the `add_operation_to_builder_transaction` command to define the proposed transfer:
```
add_operation_to_builder_transaction 0 [ 6, \
{"account": "<future-multi-sig-account-ID>", "active": \
{"weight_threshold": <multi-sig-threshold>, \
"account_auths": [["<approving-account-1-ID>",<approving-account-1-power>],\
["<approving-account-2-ID>",<approving-account-2-power>]], \
"key_auths": [], "address_auths": [] }, "extensions": []} ]
```
> Note that the value `6`, used in the first line, is specific for the `account_update_operation`, which we need for this purpose.

### Pay the transaction fee
To create the required transaction fee, associated with the proposed transfer, run this command: 
```
set_fees_on_builder_transaction <builder-handle-ID> BTS
```


 


