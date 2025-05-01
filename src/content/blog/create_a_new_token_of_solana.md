---
title: 'Create a New Token of Solana'
description: 'Learn how to create a new token on the Solana blockchain using the SPL Token program.'
tags: ['solana', 'token', 'supply']
pubDate: 'May 01 2025'
heroImage: '/h_create_a_new_token_of_solana.png'
---

## Create WorkSpace

- create dir
```shell
mkdir solana-token
cd solana-token
```

## Create Solana KeyPairs (**Wallet Address**)
- Generate Solana KeyPairs, and use is KeyPairs to sign all transactions
```shell
# gen keypair, 
➜  solana-token: solana-keygen grind --starts-with bos:1
Searching with 10 threads for:
	1 pubkey that starts with 'bos' and ends with ''
Searched 1000000 keypairs in 1s. 0 matches found.
Searched 2000000 keypairs in 3s. 0 matches found.
Wrote keypair to bosAWJC8axC1gTFaKEzv7PfL5tn8pRP2DmU2qZnCdLb.json

# use is keypair to sign all transactions
solana config set --keypair bosAWJC8axC1gTFaKEzv7PfL5tn8pRP2DmU2qZnCdLb.json
```

- change network(devnet)
```shell
➜  solana-token: solana config set --url devnet
```

- get some SOL to `bosAWJC8axC1gTFaKEzv7PfL5tn8pRP2DmU2qZnCdLb` address from [faucet](https://faucet.solana.com/), then check balance with below command
```shell
➜  solana-token: solana balance
5 SOL
```

- create a token account user to store token
```shell
➜  solana-token: solana-keygen grind --starts-with mnt:1
Searching with 10 threads for:
	1 pubkey that starts with 'mnt' and ends with ''
Searched 1000000 keypairs in 1s. 0 matches found.
Searched 2000000 keypairs in 3s. 0 matches found.
Searched 3000000 keypairs in 5s. 0 matches found.
Wrote keypair to mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp.json
```

## Create a New **Token**
- create a new Token
```shell
➜  solana-token: spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb --enable-metadata mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp.json
Creating token mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp under program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb
To initialize metadata inside the mint, please run `spl-token initialize-metadata mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp <YOUR_TOKEN_NAME> <YOUR_TOKEN_SYMBOL> <YOUR_TOKEN_URI>`, and sign with the mint authority.

Address:  mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp
Decimals:  9

Signature: BUkQciaMmtDaw4AxNGzSU1tM9VBZykwnfV8EjJD4ZEKXvSyY7nKTLUjuKG9uzmQi5UH2jrb7CxySCJ3bd1YhR7a
```
Success! `mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp` is an address of new token.

- Set a some metadata for Token
```shell
spl-token initialize-metadata <TOKEN_MINT_ADDRESS> <YOUR_TOKEN_NAME><YOUR_TOKEN_SYMBOL> <YOUR_TOKEN_URI>
```
The token URI is normally a link to offchain metadata you want to associate with the token. You can find an example of the JSON format [here](https://raw.githubusercontent.com/solana-developers/opos-asset/main/assets/DeveloperPortal/metadata.json).

```shell
➜  solana-token: spl-token initialize-metadata mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp 'Grey' 'GYPL' https://raw.githubusercontent.com/solana-developers/opos-asset/main/assets/DeveloperPortal/metadata.json
```

## Create a New **Token Account**
- making a `PAD`
```shell
➜  solana-token: spl-token create-account mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp
Creating account HWqSkjVoYRDhcY28EtF2zT6p1K9R7MPUW4q88dZJ3K1w

Signature: 59k7KkzPTMfWMmoAxWoordynFTNdtymn38muJqj1U82vNenv7841bcwhbthdbzZ2MRD6LGHqAcdgspvhP4svtdWY
```

- mint token (we mint 998 token)
```shell
➜  solana-token: spl-token mint mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp 998
Minting 998 tokens
  Token: mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp
  Recipient: HWqSkjVoYRDhcY28EtF2zT6p1K9R7MPUW4q88dZJ3K1w

Signature: 2xeScNSVsrdC8zt3A1w3xeFn6N7ReDekvExYMd7b4AeEyC74VW7MUDP7rAgrHYewhF5TaQv4BpqoECZ5PXRwtVAt
```

## Account Description
- `bosAWJC8axC1gTFaKEzv7PfL5tn8pRP2DmU2qZnCdLb`：Wallet Address
- `mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp`：Unique Token Flag
- `HWqSkjVoYRDhcY28EtF2zT6p1K9R7MPUW4q88dZJ3K1w`： Token Account Address, associated Wallets and Token.
> 
    - Token Account: A Token Account tracks the individual ownership of tokens for a specific mint account.
    - Mint Account: A mint account represents a unique token on the network and stores global metadata such as total supply.
    - Associated Account (PDA): An Associated Token Account is a token account created using an address derived from the address of the owner and mint account. 

For a wallet to own units of a certain type of token, it needs to create a token account for the specific type of token (minted) that designates the wallet as the owner of the token account. **A wallet can create multiple token accounts for the same type of token, but each token account can only be owned by one wallet and can only hold units of a particular type of token**. 



![](/b_create_a_new_token_of_solana_1.png)
![](/b_create_a_new_token_of_solana_2.png)

## Transfer Token
```shell
spl-token transfer mntKKmMvLPq8zhceYtScSWaeEgBdsUrfeAic35aGgJp 9 C1ezD8HHK5wj97ZL3NV4gABWqkqwf1fGKgDgJuwfWAWm --fund-recipient
```


- `C1ezD8HHK5wj97ZL3NV4gABWqkqwf1fGKgDgJuwfWAWm`: Wallet Address
- `--fund-recipient`: Find PDA address for Token Account address from Wallet Address