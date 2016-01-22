# Interacting with a CLI
### Prerequisites
* We assume that you have both `witness_node` and `cli_wallet` already compliled (or downloaded from [the offical respository](https://github.com/bitshares/bitshares-2/releases/latest)).  
* Also, we assume that you have the private keys and the name of some funded account.

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
Fist you need to create a new password for your wallet. This password is used to encrypt all the private keys in the wallet. For this tutorial we will use the password `supersecret` but obviously you are free to come up with your own combination of letters and numbers.   
Use this command to create the password:
```
>>> set_password supersecret
```
Now you can unlock the newly created wallet:
```
unlock supersecret
```

### Gain access to the genesis stake
In Graphene, balances are contained in accounts. To import an account into your wallet, all you need to know its name and its private key.  
We will now import into the wallet an account called `nathan` using the `import_key` command:
```
import_key nathan 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
```
> Note that `nathan` happens to be the account name defined in the genesis file. If you had edited your `my-genesies.json` file just after it was created, you could have put a different name there. Also, note that `5KQwrPbwdL...P79zkvFD3` is the private key defined in the `config.ini` file.

Now we have the private key imported into the wallet but still no funds assocciated with it. Funds are stored in genesis balance objects. These funds can be claimed, with no fee, using the `import_balance` command: 
```
import_balance nathan ["5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"] true
```
As a result, we have one account (named `nathan`) imported into the wallet and this account is well funded with BTS as we have claimed the funds stored in the genesis file.  
You can view this account by using this command:
```
get_account nathan
```
...and its balance by using this command:
```
list_account_balances nathan
```

### Create another account

We will now create another account (named `alpha`) so that we can transfer funds back and forth between `nathan` and `alpha`.

Creating a new account is always done by using an existing account - we need it because someone (i.e. an registrar) has to fund the registration fee.  
Also, there is the requirement for the registrar account to have a lifetime member (LTM) status. Therefore we need to upgrade the account `nathan` to LTM, before we can proceed with creating other accounts.  
To upgrade to LTM, use the `upgrade_account` command:
```
upgrade_account nathan true
```
> Due to a known [caching issue](https://github.com/cryptonomex/graphene/issues/530), you need to restart the CLI at this stage as otherwise it will not be aware of `nathan` having been upgraded. Stop the CLI by pressing `ctrl-c` and start it again by using exactly the same command as before, i.e.
```
cli_wallet --wallet-file=my-wallet.json --chain-id 8b7bd36a146a03d0e5d0a971e286098f41230b209d96f92465cd62bd64294824 --server-rpc-endpoint=ws://127.0.0.1:11011
```

Verify that `nathan` has now a LTM status:
```
get_account nathan
```
In the response, next to `membership_expiration_date` you should see `1969-12-31T23:59:59`. If you get `1970-01-01T00:00:00` something is wrong and `nathan` has not been successfully upgraded.

We can now register an account by using `nathan` as registrar. But first we need to get hold of the public key for the new account. We do it by using the `suggest_brain_key` command:
```
suggest_brain_key
```
And the resposne should be something similar to this:
```
suggest_brain_key
{
  "brain_priv_key": "MYAL SOEVER UNSHARP PHYSIC JOURNEY SHEUGH BEDLAM WLOKA FOOLERY GUAYABA DENTILE RADIATE TIEPIN ARMS FOGYISH COQUET",
  "wif_priv_key": "5JDh3XmHK8CDaQSxQZHh5PUV3zwzG68uVcrTfmg9yQ9idNisYnE",
  "pub_key": "BTS78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5"
}
```
So in this example:
* the public key is `BTS78CuY47V...WPr1zRL5`
* the private key is `5JDh3XmH...9idNisYnE`
* and let's assume our new account will be called `alpha`

Copy those keys as we will need them soon.

> Your public and private keys will obviously be different (as the result of the `suggest_brain_key` comamnd is random) so make sure you use those. Also, you are free to choose any other name different from `alpha`.

The `register_account` command allows you to register an account using only a public key.
```
register_account alpha BTS78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5 BTS78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5 nathan nathan 0 true
```
> Make sure you replace `BTS78CuY4...WPr1zRL5` with your version of it.

The new account has been created but it's not in your wallet at this stage. We need to import it using the `import_key` command and `alpha`'s private key:
```
import_key alpha 5JDh3XmHK8CDaQSxQZHh5PUV3zwzG68uVcrTfmg9yQ9idNisYnE
```
> Make sure you replace `5JDh3XmH...9idNisYnE` with your version of it.

### Transfer funds between accounts
As a final step, we will transfer some money from `nathan` to `alpha`. For that we use the `transfer` command:
```
transfer nathan alpha 2000000000 BTS "here is some cash" true
```
> The text `here is some cash` is an arbitrary memo you can attatch to a transfer. If you don't need it, just use `""` instead. 

And now you can verify that `alpha` has indeed received the money:
```
list_account_balances alpha
```