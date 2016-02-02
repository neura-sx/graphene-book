# Creating a proposed transfer

### Prerequisites
* We assume that you have `cli_wallet` running and connected to an exiting witness node.

* We assume that you have set up a wallet in the CLI and imported a private key of an account with some BTS funds in it. We will refer to this account as `your-base-account`.

* We assume that you have already created and registered a multi-sig account with some BTS funds in it. We will refer to this account as `muliti-sig-account`.

* We assume that you have already created and registered an account which is meant be the recipient of the proposed transfer. We will refer to this account as `destination-account`.


### Get account IDs
To get account IDs of `muliti-sig-account` and `destination-account`, run these commands:
```
get_account <muliti-sig-account-name>
get_account <destination-account-name>
```

### Initialize transaction builder
We start by initializing the transaction builder with this command:
```
begin_builder_transaction
```
As a response, you'll recieve an ID of the builder process - let's call it `<builder-handle-ID>`. This ID will be used in all subsequent commands involving the transaction builder.

### Add operation to the builder
We will now define the proposed transfer we aim to create. Our variables are as follows:  
* `<muliti-sig-account-ID>` - the ID of `muliti-sig-account`, e.g. `1.2.127`,
* `<destination-account-ID>` - the ID of `destination-account`, e.g. `1.2.128`,
* `<transfer-amount>` - the amount (in satoshi) to be sent, e.g. if you want to send BTS 14.00 use `1400000`,    
* `<transfer-asset-id>` - the ID of the asset in which the transfer will be denominated, e.g. for BTS use `1.3.0`.

Run the `add_operation_to_builder_transaction` command to define the transfer:
```
add_operation_to_builder_transaction 0 [0, \
{"from": "<muliti-sig-account-ID>", \
"to": "<destination-account-ID>", \
"amount": {"amount": <transfer-amount>, \
"asset_id": "<transfer-asset-id>"}}]
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
If you recieve no error, it means your new multi-sig account has been successfully created.

### Import the multi-sig account
To import the newly created multi-sig account into your wallet, run this command:
```
import_key <muliti-sig-account-name> <muliti-sig-account-private-key>
```
> Note that the private key to be used here comes from the `suggest_brain_key` command described above.

### Try it out
Transfer some funds to the new multi-sig account:
```
transfer <your-base-account-name> <muliti-sig-account-name> 10000000 BTS "here is some cash" true
```
Now try to pay out some funds from the multi-sig account (make sure it's less than the amount received, so there are funds left to cover the transfer fee):
```
transfer <muliti-sig-account-name> <your-base-account-name> 300000 BTS "paying back" true
```
As a result, you should get an error indicating that funds cannot be moved:
```
0 exception: unspecified
3030001 tx_missing_active_auth: missing required active authority
Missing Active Authority 1.2.132
```
This is the expected outcome, as we are dealing with a multi-sig account. In order to pay out any funds from it, we need to propose a transfer instead, and have it approved by `approving-account-1` and / or `approving-account-2`.