# DFlow AMM interface

Rust crate for the `Amm` trait to integrate with dflow-core.

## Integration Overview

`Amm` trait implemenation:

1. **Create the AMM instance**: `from_keyed_account` parse the pool/market account and create AMM for pool
2. **Provide pool metadata**: `label`, `program_id`, `key`, and `get_reserve_mints` to identify pool
3. **Handle state updates**: `get_accounts_to_update` list accounts needed for quotes, then `update` to deserialize and cache market/pool states
4. **Provide quotes**: `quote` to calculate swap output amounts based on current market/pool state
5. **Generate swap instructions**: `get_swap_and_account_metas` to return the swap instruction type and required accounts


## Important Conventions

### Quote Method

`quote.in_amount <= quote_params.amount`.

When implementing the `quote` method, use input capping if the requested input amount cannot be fully consumed (e.g., due to liquidity limits or other constraints). Return a `Quote` with `in_amount` less than the requested amount rather than returning an error or panicking.
