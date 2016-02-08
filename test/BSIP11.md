    BSIP: 0011
    Title: Registrar  Annual Membership
    Authors: Jakub Zarembinski <jakub.zarembinski@neura.sx>
    Status: Draft
    Type: Protocol
    Created: 2016-02-08
    Discussion: https://bitsharestalk.org/index.php/topic,21314.0.html
    Worker: t.b.d.

> The subject of this proposal is Annual Membership (AM). However, if a shorter membership (e.g. monthly) was introduced, this proposal could easily be extended to cover shorter subscriptions.

# Abstract
Currently the Annual Membership (AM) price is set globally by the committee and there is a 20% cut reserved for the network. The remaining 80% goes to the registrar and the referrer (the split between them is defined by the registrar).

This arrangement is not flexible. A registrar, operating on a market which requires very low transfer fees, is limited by the minimum price required by the network. The purpose of this proposal is to remove this limitation and allow a registrar (i.e. hosted wallet service) to offer very low transfer fees by selling AM at a price the customers are willing to pay on a given regional market (including the price of zero).

What is proposed here, is a very simple change: the price of AM to be determined not globally by the committee but locally by the regional registrars (i.e. hosted wallet services). This way, in regions requiring low fees, AM can be given away for free (or almost for free), which translates into extremely low transfer fees, as an AM means 80% discount on those fees for its holder.

# Motivation
The primary motivation of this proposal is to allow BitShares to become competitive on markets which might require very low transfer fees, while taking care of those two important aspects:
* preserving the default transfer fee at the current level, which is high enough to support the referral program, as it was initially announced in June 2014,

* preserving the network's income at the current level.


# Rationale
By allowing the AM to be flexibly determined locally be a registrar, effective transfer fee can be reduced by 80% by selling AM at a very low price or giving it away for free to all newly registered accounts.

The all



# Specifications
* The price of AM is determined by the registrar who has originally registered a given account.

* To allign the network's interests with the registrar's intrestes, 20% of registrar's income from AM sale is allocated to the network. The remaining 80% goes to the registrar and the referrer (the split between them is defined by the registrar), as we have it now.

* There is a minimum price limit for AM deteremined by the committee. To achieve the economic effect described in this proposal, this limit will have to be set to zero or almost zero by the committee. There will no upper price limit for AM.

* There is no vesting on cash-backs related to AM sales.

# Discussion
This is the most crucial question: if the price of AM has no lower limit, how can the network's income be preserved? Will there be lost revenue for the network as a result of this proposal.

We must all agree, that a revenue can be considered as lost *only* when a user ends up paying less than s/he was willing to pay. Otherwise no revenue is actually lost, because no sale would have happened anyway. 

If there was a perfect competition between hosted wallet services (or generally all registrars) the issue of lost revenue could have some ground, as the price of AM would be constantly undermined by market participants. However, it will take a long time for the BitShares ecosystem to reach this state and when this state is reached there will be no need to for the referral program anyway.

The main assumption we make is that a registrar is a business oriented entity wanting to maximize its profits. Thus it will utilize the opportunity of selling AM very cheaply or for free, only if it operates on a market where an average user is not willing to pay a better price. This way the price of AM is naturally adjusted to the average price acceptable by customers on a given market. On some markets it can mean something close to zero, but most of those who get AM for zero would not have bought it for any other price, so no income is lost.

The other assumption we make is that if a registrar offers a reasonable price for AM (e.g. $5), most customers are not going to bother to hunt for a better bargain and then attempt to migrate their accounts. For most customers, money saved this way is not worth the effort.


# Copyright
This document is placed in the public domain.
