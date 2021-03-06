    BSIP: 0014
    Title: Annual Membership price determined by the registrar
    Authors: Jakub Zarembinski <jakub.zarembinski@neura.sx>
    Status: Draft
    Type: Protocol
    Created: 2016-02-08
    Discussion: https://bitsharestalk.org/index.php/topic,21366.0.html
    Worker: t.b.d.
    
    
> The subject of this proposal is Annual Membership (AM). However, if a shorter membership (e.g. monthly) was introduced, this proposal could easily be extended to cover shorter subscriptions.


# Abstract
Currently the Annual Membership (AM) price is set globally by the committee and there is a 50% cut reserved for the network. The remaining 50% goes to the registrar and the referrer (the split between them is defined by the registrar).

This arrangement is not flexible. A registrar targeting price-sensitive customers is limited by the minimum price required by the network. The purpose of this proposal is to remove this limitation and allow a registrar (i.e. hosted wallet service) to offer very low transfer fees by selling AM at any price the customers are willing to pay (including the price of zero).

What is proposed here, is a very simple change: the price of AM to be determined not globally by the committee but locally by the regional registrars (i.e. hosted wallet services). This way, in regions requiring low fees, AM can be given away for free (or almost for free), which translates into access to extremely low transfer fees, as an AM means 50% discount on those fees for its holder.


# Motivation
The primary motivation of this proposal is to reconcile two opposing interests:

1. Allow BitShares to become competitive on markets which might require very low transfer fees.

2. And at the same time preserve those two important aspects:
    * preserve the default transfer fee at the current level (i.e. around $0.10), which is high enough to support the referral program, as it was initially announced in June 2014,
    * preserve the network's income stream at the current level.


# Rationale
By allowing the AM price to be determined locally be the registrar, effective transfer fee can be easily slashed by 50% by selling AM at a very low price or giving it away for free to all newly registered accounts.

To align the network's interests with the registrar's interests, we propose to make the network's income proportional to the registrar's income from AM sales but not bigger than certain threshold defined by the committee.


# Specifications
* The committee sets two values:  
(a) Maximum Income `MI` - it's the maximum income the network *would like* to receive from each AM sold,  
(b) Network's Share `NS` - it's the network's share in the registrar's income from AM sales.  
The registrar is obliged to share with the network `NS` of its income but not more than `MI`. In other words, the network gets the smaller value of these two: `MI` and `NS` * AM price. Thus it's not guaranteed to receive `MI`, but this mechanism will attempt this, if the AM price is high enough.

* The price of AM is determined by the registrar who has originally registered a given account. When registering a new account, the registrar sets the future price of AM in terms of percentage of `MI`.

* The split between the registrar and the referrer is defined by the registrar, as we have it now.
 
Let's consider two examples to illustrate how it works:  
### Example I
* `MI` = $4.00
* `NS` = 50%
* AM price = 300%
* referrer share = 60%

AM price for the customer is $12.00 [= 300% * $4.00]. 
* the network gets $4.00 [= min($4.00, 50% * $12.00)]
* the business gets $8.00: the registrar gets $3.20 and the referrer gets $4.80

### Example II
* `MI` = $4.00
* `NS` = 50%
* AM price = 20%
* referrer share = 80%

AM price for the customer is $0.80 [= 20% * $4.00]. 
* the network gets $0.40 [= min($4.00, 50% * $0.80)]
* the business gets $0.40: the registrar gets $0.08 and the referrer gets $0.32


# Discussion
This is the most crucial question: if the price of AM has no lower limit, how can we make sure the network's income is preserved? Will there be lost revenue for the network as a result of this proposal?

We must all agree, that a revenue can be considered as lost *only* when a user ends up paying less than s/he was willing to pay. Otherwise no revenue is actually lost, because no sale would have happened anyway. 

If there was a perfect competition between hosted wallet services, the issue of lost revenue could have some ground, as the price of AM would be constantly undermined by the market participants. However, it will take a long time for the BitShares ecosystem to reach this state and by the time this state is eventually reached, the referral program will be redundant anyway and this mechanism will no longer be needed.

The main assumption we make is that a registrar is a business-oriented entity aiming to maximize its profits. Thus it will utilize the opportunity of selling AM very cheaply or for free, *only* if it operates on a market where an average user is not willing to pay a better price. This way the price of AM is naturally adjusted to the average price acceptable by customers on a given market. On some markets this value can be close to zero, but most of those customers, who will acquire AM for zero, would not have bought this subscription for any other price, so no income is actually lost.

The other assumption we make is that if a registrar offers a reasonable price for AM (e.g. $5), most customers are not going to be bothered to hunt for a better bargain and then attempt to migrate their accounts. For most customers, money saved this way is not worth the effort. This is especially true for shorter subscriptions.


# Copyright
This document is placed in the public domain.