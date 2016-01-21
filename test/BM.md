## Primary rules for BitShares operations

1. All values relevant to the display of the transaction history should be included in the operation.  Namely, the fee must be calculated in advance and explicitly included in the transaction.
2. Existing operations should not be changed if this requires adding additional fields.
3. As long as the FEE * CORE_EXCHANGE_RATE > MIN_NETWORK_FEE_IN_BTS then things should be fine.

The MIN fee is important to prevent spamming of UIAs that have no value.

https://bitsharestalk.org/index.php/topic,21080.msg273185.html#msg273185