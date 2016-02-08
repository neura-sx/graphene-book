    BSIP: 0011
    Title: Registrar  Annual Membership
    Authors: Jakub Zarembinski <jakub.zarembinski@neura.sx>
    Status: Draft
    Type: Protocol
    Created: 2016-02-08
    Discussion: https://bitsharestalk.org/index.php/topic,21314.0.html
    Worker: t.b.d.

# Abstract
Currently the Annual Membership (AM) price is set globally by the Committee and there is a 20% cut reserved for the network. The remaining 80% goes to the registrar and the referrer (the split between them is defined by the registrar).

This arragment is not flexible. A registrar, operating on a market which requires very low tranfer fees, is limited to the minimum price required by the network. The purpose of this proposal is to remove this limitation and allow a registrar to offer AM for any price she wants, provided that 20% of her income from AM sale is allocated to the network.

What is proposed is a very simple change: the price of AM to be dteremined not globally by the commitee but locally by the regional registrars. In regions requiring low fees, AM given away for free (or almost for free) translates into low transfer fees, as 80% discount is awarded automaticlly to all AM holders.

# Motivation
The primary motivation of this proposal is to allow BitShares to be competitve on markets which might require very low transfer fees, while taking care of these two important aspects:
* keep the transfer fee at a level high enough to support the referral program, as it was initially annaoucned in June 2014.
* preserve network's income from AM sales


# Rationale
By allowing the AM to be flexibley deteremined locally be a registrar, effective transfer fee can be reduced by 80% by selling AM at a very low price or giving it away for free to all newly registered accounts.


# Specifications
#### The assumptions
The following description uses a fictional asset called BAX but this solution can be applied to all assets (i.e. both public and private SmartCoins, UIAs and FBAs) and all non-stealth transfers. Also, what is described below refers to pay-as-you-go users. For LTM users exactly the same rules apply, except for the fact that in this case the referrer's income is redirected to the user's cash-back fund.

> Please note that values used in the description below are just an initial proposal, the actual values will be determined be the committee by means of global blockchain parameters.

#### The concept
For a transfer of BAX, the transfer fee can be paid either in BTS or BAX.

1. If the user chooses to pay the fee in BTS:
  * The transfer fee is equal to 1% of the BTS value of the BAX amount being transferred, adjusted by a floor (i.e. minimum fee) and a cap (i.e. maximum fee): 6 BTS and 300 BTS respectively. So effectively the transfer fee is 1% but it is never less than 6 BTS and never more than 300 BTS.
  * To establish the BTS value of the transfer, CER of BAX is used.
  * The fee paid by the user is split as follows: 6 BTS goes to the network, any excess above 6 BTS goes to the referrer.
  * The issuer does not earn anything.
2. If the user chooses to pay the fee in BAX:
Let *F* be the transfer fee (expressed in BTS) which was calculated as described in (1).
  * To establish the BAX equivalent of *F*, CER is used.
  * The issuer receives the entire fee paid by the user in BAX.
  * The following amounts are deducted from the asset fee pool: 6 BTS goes to the network, and (*F* - 6) BTS goes to the referrer.
  * The issuer makes a profit or loss, depending on CER being above or below the actual BTS/BAX exchange rate in the same way as it is currently implemented in the flat fee model.

#### The main issue
What happens when the issuer attempts to artificially lower transfer fees on his asset by pretending his asset is not worth much and deliberately keeping CER below the actual BTS/BAX exchange rate?
- for the user: it's cheaper for him to pay the fee in BTS rather than in BAX
- for the issuer: he makes a profit on all those transfers for which the user, for whatever reasons, decides to pay the more expensive fee in BAX (instead of the cheaper one in BTS)
- for the referrer: no matter if the user pays the fee in BAX or BTS, he is deprived of a large part of the referral reward, as due to distorted CER, transfers are interpreted by the system as involving smaller value than they actually are.

#### How is this issue solved?
By keeping CER artificially low, the issuer will effectively discourage users from using his own asset because transfer fees will only be low when paid in BTS, so the process of transferring the asset will be dependent on users having BTS in their wallets to cover the fees. Furthermore, if the issuer distorts CER too much, he'll not be able to benefit from it himself as users will start avoiding paying the fees in BAX. Also, it will be bad PR for the issuer to value his own asset below the market rate. At least in case of SmartCoins (both public & private) issuers are not likely to be willing to sabotage their own assets in this way.

#### Additional abuse prevention mechanism
However, if it turns out that a large number of issuers try to game the system, in the future we can introduce the following mechanism aimed at preventing abuse on the part of issuers:

* To enable percentage-based transfer fees, the issuer will be obliged to keep an open ask order on the BTS:BAX market and thus allow anybody to buy BAX at e.g. 120% of CER. As a result, if CER is out of sync with the market rate by more than 20%, the issuer will incur a significant loss.
* If the issuer is not willing to comply with the above requirement, the default flat transfer fee structure will be applied.

> Please note that this additional abuse prevention mechanism will not be implemented with this worker proposal. If there is a need for it in the future, we will consider it as a separate worker proposal.

#### This is an opt-in feature
As the issuer is the only entity that actually controls CER, the percentage-based transfer fee is meant to be an opt-in feature decided by the issuer. It is safe to assume that most issuers will find it beneficial for their assets because:

* assets which are worth something will benefit from having a reasonable pricing structure for transfers
* assets which are worth nothing or almost nothing (i.e. most UIAs) will be able to be transferred almost for free (for 6 BTS instead of the current 30 BTS).

However, if an issuer for some reasons does not find the above features beneficial, he will be able to keep the existing flat fee structure.

> Please note that the choice of the fee model is not meant to be permanent. The issuer is always free to change his mind and switch back and forth between flat and percentage-based mode.

#### The intended workflow

Action | Result
--- | ---
An issuer sets his asset to the percentage-based transfer fee mode. | After the change, fees for all transfers on that asset will be calculated with the new algorithm.
An issuer sets his asset to the flat transfer fee mode. | After the change, fees for all transfers on that asset will be flat.
The Committee adjusts the fee parameters. | After the change, for transfers on all assets which have applied percentage-based transfer fee mode, fees will be calculated with the new parameters.
The Committee sets the percentage based fee mode for a SmartCoin. | After the change, fees for all transfers on that asset will be calculated with the new algorithm.
The Committee sets the flat fee mode for a SmartCoin. | After the change, fees for all transfers on that asset will be flat.
A user makes a transfer on an asset assigned to the flat fee mode.  | The user will be charged flat fee, 20% of the fee goes to the network, 80% of the fee goes to the referral program and is split among the parties inside the referral program (registrar, referrer etc).
A user makes a transfer on an asset assigned to the percentage based fee mode. | The user will be charged a percentage fee, the minimum fee goes to network, the rest (if there is anything left) goes to the referral program and is split among the parties inside the referral program.

# Discussion
Being voluntary for issuers, the above proposal is actually targeted to the referral businesses: do they perceive it as a beneficial change for the ecosystem and a fair deal for them? If we stick to the parameter values used above, they will need to forgo the referral income on transfers below the equivalent of $2 but will substantially increase their income on transfers above the equivalent of $10. In the range between $2 and $10 they will get on average half of the income they have now. Nevertheless, the main benefit will be indirect: it's much easier to sell a reasonably priced product.

As for the BitShares network itself, the overall impact of this proposal will be positive, as the network's resources will be better utilized. With the current flat fees, most of the blocks are empty. Percentage-based fees will not fix this waste immediately, but will be a step in the right direction and a real opportunity for businesses based on small tips and micro-payments.

# Areas covered
This proposal covers the entire implementation of percentage-based fees.  
In particular, the following aspects will be covered:

* coding
* code review by CNX
* unit testing and testnet testing
* deployment
* full documentation & guidelines for similar projects
* GUI support

# Known issues and possible solutions
These are the identified issues (or limitations) and available solutions that address them:

Known issue (or limitation) | Possible solution
--- | ---
The values of the minimum and maximum limits are denominated in BTS (as all fees in the system). If those limits are not updated on a regular basis, we might end up with discrepancies between their current fiat value and the intended initial value. | There is another project (run by `xeroc`) aiming to address this issue, by introducing an automated way to keep all fees synced to fiat-defined targets. So if we combine our proposal with `xeroc`'s project, we should end up with a complete, fiat-stable solution.
The percentage-based fee scheme cannot be applied to stealth transfers. | There is no solution available. Due to the very nature of stealth transactions, it's impossible to determine the amount transferred and thus apply a percentage-based transfer fee.
Due to technical reasons, the percentage-based fee scheme cannot be easily applied to the core currency i.e. BTS. | Together with CNX, we are actively looking at ways to find a solution to this problem - one of the options considered is changing the ownership of BTS from `null-account` to `committee-account`. (We are aware that this might be highly controversial, so please treat it as just one of several options considered.)
There might be a negative UX in a very rare situation: when CER is being updated by the issuer between the moment the user hits `Transfer` and the moment s/he hits `Confirm`. In this situation, the user might receive an error message about insufficient funds to cover the transfer fee and will have to redo the transfer. | Thanks to [this suggestion](https://bitsharestalk.org/index.php/topic,21080.msg275323.html#msg275323) by `roadscape`, there might be a viable workaround for this issue at the GUI level.
~~In case of committee-controlled SmartCoins, occasionally there might be a small discrepancy between the current value of CER and the value of CER enclosed in the committee proposal to switch a given SmartCoin to the percentage-based mode.~~ |  We will most probably be able to offer a patch to the code-base, which will make it possible to update an asset without changing CER, and thus entirely fix this issue.
If the committee decides to apply percentage-based fees to SmartCoins, this might affect third-party partners (e.g. exchanges, gateways etc), especially their pricing policies. | Our partners need to be notified in advance, so that they have time to adjust their processes.

# Copyright
This document is placed in the public domain.
