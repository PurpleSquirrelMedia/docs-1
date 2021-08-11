# Code Required Hook

The CodeRequiredHook contract allows lock creators to restrict key purchases.

Note that this has not yet been launched, but coming soon!

## About

Simply put, with this feature we can require a secret is known in order to purchase keys. Here are a few use cases this enables:

### Access code

Example: `Enter 'this is a beta' to gain access`

You might want to limit access to a smaller group, e.g. maybe you are beta testing a new feature and want to invite everyone from your Telegram channel. You could offer a key for free, but require they explicitly acknowledge the risks by entering this code.

Obviously someone could Tweet the code, spreading it future than intended. However it still effectively limits access to "insiders".

### Whitelist

Example: `KYC required`

A whitelist can be created with this feature as well. This allows a trusted operator to confirm which ETH accounts may purchase a key. e.g. maybe you need to check IDs first.

Once approved, the operator shares the signature required to purchase. This allows only that account to purchase and the secret itself can remain secure \(with the operator\).

## How it works

The CodeRequiredHook requires knowing a secret. The secret can be anything \(most commonly a string\) but then it's converted to a private key \(and we have recommendations for that process\).

Given a private key, we have a public key as well - an Ethereum address \(with 0 funds\). We call this the `codeAccount`. We can store the `codeAccount.address` publicly without risking exposing the secret itself on-chain.

In order to purchase, we generate a signature with `codeAccount.sign(sha(keyOwner))` where `keyOwner` is the address of the account that's approved to purchase a key. Including the `keyOwner` ensures that another account cannot simply replay the same transaction to purchase for themselves.

When issuing a transaction to purchase, the signature created by the `codeAccount` is included in the `bytes _data` field. The lock forwards this information to the CodeRequiredHook for confirmation. If the signature is missing or not correct then the entire transaction reverts.

### Creating the codeAccount

todo

### Generating the signature

todo

### Configuring locks

todo

### Front end integration

todo

For the discount code we don't need to expose this as 2 separate locks. Ideally the frontend abstracts this away from users. The user can enter a code - if they do confirm it's valid and then present a discounted purchase link. If no code is entered, use the full price purchase link.

## Future improvements

* Simplify the hook registration process
* Support multiple codes per lock
* Support discounts or refunds \(instead of requiring multiple locks for many scenarios\)

