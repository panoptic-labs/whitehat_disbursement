#### User fund snapshot methodology

> Block `t` is 1 block before the block where the first whitehat transaction on mainnet executed

To get the snapshot of funds each user had in the protocol at the time of the rescue, we calculate the following at block `t`, for each account with open positions or deposits in a Panoptic pool:
1. The net value of the account's options portfolio in token0 and token1, calculated by [`getNetLiquidationValue`](https://github.com/panoptic-labs/panoptic-v1-helper/blob/main/src/PanopticQuery.sol#L849) with arguments as follows:
    * pool - self explanatory
    * account - self explanatory
    * `includePendingPremium` - true
    * `positionIdList` - using open positions from subgraph at block `t`
    * `atTick` - using panoptic pool's TWAP tick at block `t`.
2. Calculate the share price at block `t` for collateral0 and collateral1 for each Panoptic pool (e.g. `share_price0 = `[`collateral0.totalAssets`](https://github.com/panoptic-labs/panoptic-v1-core/blob/df4dc38dee4fe29fd889cffaa8097dccc561e572/contracts/CollateralTracker.sol#L347-L351) `/` `collateral0.totalSupply`).
3. Fetch the user's collateral0 and collateral1 share balances at the block.
4. Convert the user's collateral balances to assets using the share price (`shares * share_price`).
5. Net resulting token0 and token1 assets to get each user's total funds at the time of the rescue.

For Ethereum mainnet, block `t` is 23242435, because the first whitehat transaction occured on block 23242436.

For Base, block `t` is 34814389, the Base block closest in time to Ethereum mainnet block 23242435

For Unichain, block `t` is 25669765, the Unichain block closest in time to Ethereum mainnet block 23242435