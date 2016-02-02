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


 


