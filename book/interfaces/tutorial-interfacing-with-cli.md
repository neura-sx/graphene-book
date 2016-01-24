# Interfacing with a Command Line Interface
### Prerequisites
* We assume that you have both `witness_node` and `cli_wallet` already compliled (or downloaded from [the offical respository](https://github.com/bitshares/bitshares-2/releases/latest)).

* We assume that you have active private keys for two existing accounts and at least one of them has some funds. Please refer to [FAQs](FAQs.md#retrieving-private-keys) if you are not sure how to retrieve private keys from the GUI.

### Run the witness node
To run the witness node use this command:
```
witness_node --rpc-endpoint 127.0.0.1:11011
```

### Run the CLI
To run the CLI use this command:
```
cli_wallet --server-rpc-endpoint=ws://127.0.0.1:11011
```

### Create a new wallet
First you need to create a new password for your wallet. This password is used to encrypt all the private keys in the wallet. For this tutorial we will use the password `supersecret` but obviously you are free to come up with your own combination of letters and numbers.   
Use this command to create the password:
```
>>> set_password supersecret
```
Now you can unlock the newly created wallet:
```
unlock supersecret
```

### Import the private keys
We will now import your existing accounts into our wallet.  
Let's assume one of your accounts is this:
* its name is `nathan`
* its private key is `5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3`

And the other one is this:
* its name is `alpha`
* its private key is `5JDh3XmHK8CDaQSxQZHh5PUV3zwzG68uVcrTfmg9yQ9idNisYnE`



Use this command to import your accounts:
```
import_key nathan 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
import_key alpha 5JDh3XmHK8CDaQSxQZHh5PUV3zwzG68uVcrTfmg9yQ9idNisYnE
```
You can view these accounts by using this command:
```
get_account nathan
get_account alpha
```
...and their balance by using this command:
```
list_account_balances nathan
list_account_balances alpha
```

### Transfer funds between accounts
We will now transfer some money from `nathan` to `alpha` (assuming there are some funds on `nathan`).
For transferring funds we use the `transfer` command:
```
transfer nathan alpha 20000000 BTS "here is some cash" true
```
> The text `here is some cash` is an arbitrary memo you can attatch to a transfer. If you don't need it, just use `""` instead. Also, make sure the amount transferred is within the funds available for this account. The value `20000000` translates into 200 BTS as in case of BTS the last 5 figures are just decimal points.

And now you can verify that `alpha` has indeed received the money:
```
list_account_balances alpha
```

### Create another account

> If none of your accounts has a lifetime member (LTM) status, skip this step as you will not be able to create a new account in this situation. You can verify the LTM status of an account by running the `get_account` command and checking the timestamp next to the `membership_expiration_date` entry. If the value is `1969-12-31T23:59:59` then the account is LTM, otherwise it is not.

We will now create another account named `alpha2` and assume that `nathan` is the account with a LTM status and it has enough funds to pay the registration fee.

Creating a new account is always done by using an existing account - we need it because someone (i.e. the registrar) has to fund the registration fee. We will use `nathan` for this purpose as it has the required LTM status.

First we need to get hold of the public key for the new account. We do it by using the `suggest_brain_key` command:
```
suggest_brain_key
```
And the response should be something similar to this:
```
suggest_brain_key
{
  "brain_priv_key": "UPLOCK RUNE MORTAR TOASTER TWIGGED POMONIC GORING SUNNY SPEAN BOCARDO GODHEAD ABLY WRAXLE OHOY EMERGE JERQUE",
  "wif_priv_key": "5JK1HAUjCh5H7Ubc1pa8WU7ydEiPkmQjAVF3L9oYdLWBBP5v2jN",
  "pub_key": "BTS7XRAcWjD8xZKRR5u8MY34xU6tZDFVyvDthoXJ1LbtgLLft8TMu"
}
```
So in this example:
* the public key is `BTS7XRAcWjD...LbtgLLft8TMu`
* the private key is `5JK1HAUj...YdLWBBP5v2jN`
* and let's assume our new account will be called `alpha2`

Copy those keys as we will need them soon.

> Your public and private keys will obviously be different (as the result of the `suggest_brain_key` comamnd is random) so make sure you use those. Also, you are free to choose any other name different from `alpha2`.

The `register_account` command allows you to register an account using only a public key.
```
register_account alpha2 BTS7XRAcWjD8xZKRR5u8MY34xU6tZDFVyvDthoXJ1LbtgLLft8TMu BTS7XRAcWjD8xZKRR5u8MY34xU6tZDFVyvDthoXJ1LbtgLLft8TMu nathan nathan 0 true
```
> Make sure you replace `BTS7XRAcWjD...LbtgLLft8TMu` with your version of it.

The new account has been created but it's not in your wallet at this stage. We need to import it using the `import_key` command and `alpha2`'s private key:
```
import_key alpha2 5JK1HAUjCh5H7Ubc1pa8WU7ydEiPkmQjAVF3L9oYdLWBBP5v2jN
```
> Make sure you replace `5JK1HAUj...YdLWBBP5v2jN` with your version of it.

At this stage you should have three accounts in your wallet. You can run this command to verify this:
```
list_my_accounts
```
