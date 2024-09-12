This repository shows how to obtain the USD prices of Valoro Tokens.

## Main Valoro Smart Contract (Proxy SC)

- Mainnet:

    `erd1qqqqqqqqqqqqqpgqqvj2zrdfv4lsc38p8cvh4e0yd4av6njfu7zsj7ztzl`

- Devnet:

    `erd1qqqqqqqqqqqqqpgqpg2f64hdvwupszxf0jedcc06787w9n6du7zshu55yu`


# How to obtain prices

## 1. Get all Valoro Token SCs:

The SC mapper is:

```rust
#[view(getIndexTokenScs)]
#[storage_mapper("indexTokenScs")]
fn index_token_scs(&self) -> UnorderedSetMapper<ManagedAddress>;
```

Every SC represents one Valoro Token (Fungible ESDT). The tokens have 18 decimals.

## 2. For every SC (Valoro Token):

### Either get separately:

- ### Token Identifier

```rust
#[view(getIndexTokenId)]
fn get_index_token_id(&self) -> TokenIdentifier
```

- ### USD Price

```rust
#[view(getIndexTokenPrice)]
fn get_index_token_price(&self) -> BigUint
```

### Or get at once:

- ### IndexTokenInfo

```rust
#[view(getIndexTokenInfo)]
fn get_index_token_info(&self) -> IndexTokenInfo<Self::Api> 
```

```rust
pub type IndexTokenInfo<M> = MultiValue11<
    ManagedAddress<M>, // Fund Manager
    TokenIdentifier<M>, // USDC Identifier
    TokenIdentifier<M>, // Token Identifier
    ManagedBuffer<M>, // Fund Name
    u32, // Token Decimals
    BigUint<M>, // NAV (Net Assets Value)
    BigUint<M>, // SUPPLY (Tokens currently minted)
    bool, // is initialized
    bool, // is paused
    ManagedVec<M, u64>, // Fees
    ManagedVec<M, FundTokenStructure<M>>, // Fund tokens
>;
```

Simply, `NAV` divided by `SUPPLY` results the Token price:
```
Price = NAV / SUPPLY
```