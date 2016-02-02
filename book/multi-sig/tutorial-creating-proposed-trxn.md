# Creating a proposed transfer

### Prerequisites
* We assume that you have `cli_wallet` running and connected to an exiting witness node.

* We assume that you have set up a wallet in the CLI and imported a private key of an account with some BTS funds in it. We will refer to this account as `your-base-account`.

* We assume that you have already created and registered a multi-sig account with some BTS funds in it. We will refer to this account as `muliti-sig-account`.

* We assume that you have already created and registered an account which is meant be the recipient of the proposed transfer. We will refer to this account as `destination-account`.


### Get account IDs
To get the account IDs of `muliti-sig-account` and `destination-account`, run these commands:
```
get_account <muliti-sig-account-name>
get_account <destination-account-name>
```
We will need those IDs in subsequent steps.

### Initialize transaction builder
We start by initializing the transaction builder with this command:
```
begin_builder_transaction
```
As a response, you'll recieve an ID of the builder process - let's call it `<builder-handle-ID>`. This ID will be used in all subsequent commands involving the transaction builder.

### Add transfer to the builder
We will now define the transfer we want to propose. Our variables are as follows:  
* `<muliti-sig-account-ID>` - the ID of `muliti-sig-account`, e.g. `1.2.127`,
* `<destination-account-ID>` - the ID of `destination-account`, e.g. `1.2.128`,
* `<transfer-amount>` - the amount (in satoshi) to be sent, e.g. if you want to send BTS 14.00 use `1400000`,    
* `<transfer-asset-id>` - the ID of the asset in which the transfer will be denominated, e.g. for BTS use `1.3.0`.

Run the `add_operation_to_builder_transaction` command to define the proposed transfer:
```
add_operation_to_builder_transaction <builder-handle-ID> [0, \
{"from": "<muliti-sig-account-ID>", \
"to": "<destination-account-ID>", \
"amount": {"amount": <transfer-amount>, \
"asset_id": "<transfer-asset-id>"}}]
```
> Note that the value `0`, used in the first line, is specific for the `transfer_operation`, which we need for this purpose.

### Pay the transaction fee
To create the required transaction fee, associated with the proposed transfer, run this command: 
```
set_fees_on_builder_transaction <builder-handle-ID> BTS
```

### Propose the transfer
We will now define the transfer proposal itself. Our variables are as follows:  
* `<your-base-account-name>` - the name of `your-base-account` (this account will pay the fee for creating the proposal),
* `<expiry-timestamp>` - the timestamp (in UTC) defining when the proposal expires, e.g. `2016-02-02T00:30:00`.

Run the `propose_builder_transaction2` command to define the proposal:
```
propose_builder_transaction2 <builder-handle-ID> <your-base-account-name> \
"<expiry-timestamp>" 0 false
```
> You need to have access to `propose_builder_transaction2` command in your CLI. As of now, using the default command (i.e. `propose_builder_transaction` without `2` at the end) will not succeed due to unresolved bugs.  
Also, it's important not to broadcast the transaction at this stage, so make sure the broadcast flag (i.e. the last parameter) is set to `false`.

### Pay the transaction fee
To create the required transaction fee, associated with creating the proposed itself, run this command: 
```
set_fees_on_builder_transaction <builder-handle-ID> BTS
```

### Sign the builder transaction
To complete the process, i.e. sign and broadcast the above builder transactions, run this command:
```
sign_builder_transaction <builder-handle-ID> true
```
If you recieve no error, it means your proposed transfer has been successfully created and broadcast, and it's waiting approval.