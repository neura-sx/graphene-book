# Approving a proposed transfer

### Prerequisites
* We assume that you have `cli_wallet` running and connected to an exiting witness node.

* We assume that you have set up a wallet in the CLI and imported the active private key of an account with some BTS funds in it. We will refer to this account as `your-base-account`.

* We assume that you have already created and broadcast (using `your-base-account`) a proposed transfer awaiting to be approved by two accounts, whose private keys you control. We will refer to these accounts as `approving-account-1` and `approving-account-2`.

* We assume you have imported to your CLI the private keys for `approving-account-1` and `approving-account-2`. 
 
### Get the proposed transfer ID
The easiest way to find out the proposed transaction ID, is to browse the history of the account that has created the proposal:
```
get_account_history <your-base-account-name> <limit>
```
> For the `<limit>` value put a number like 10 or 100, depending how many transactions you expect to have made **after** creating the proposed transfer. If you're not sure, make the number bigger.

As a result, in the list of transactions making up the account history, you should be able to locate the proposed transfer, which should look similar to this example:
```
> 2016-01-31T00:23:30 proposal_create_operation neura-sx fee: 20 BST result: 1.10.14
```
In this particular case the proposed transfer ID is `1.10.14` but yours will obviously be different.

### Approve the proposed transfer
We will use `your-base-account` as the account, which will pay the transaction fees for the approvals. We could've used any other funded account, whose private key you control.
```
approve_proposal <your-base-account-name> <proposed-transfer-ID> \
{"active_approvals_to_add" : ["<approving-account-1-name>"]} true

approve_proposal <your-base-account-name> <proposed-transfer-ID> \
{"active_approvals_to_add" : ["<approving-account-2-name>"]} true
```
> Make sure the above approvals are performed **before** the expiry timestamp of the proposed transfer (the time-zone used there is UTC).

If you receive no error, it means the approvals have gone through successfully, and as a result, the proposed transfer has been immediately executed on the blockchain.

### Verify the result
You can confirm the proposed transfer execution by checking the account history of the multi-sig account:
```
get_account_history <multi-sig-account-name> 10
```
