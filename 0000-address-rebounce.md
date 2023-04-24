- **TEP**: [0](https://github.com/ton-blockchain/TEPs/pull/0) *(don't change)*
- **title**: Address Rebounce
- **status**: Draft
- **type**: Meta
- **authors**: [Oleg Andreev](https://github.com/oleganza)
- **created**: 24.04.2023

# Summary

This proposal specifies "bounceable" value when formatting addresses for wallets and decentralized apps.
* "Ordinary wallets" should always format their addresses as "non-bounceable" ("UQ..." on mainnet) in order to accept coins even when not initialized.
* "Apps" should always  format their addresses as "bounceable" ("EQ..." on mainnet) in order to ensure the message is processed properly.

This proposal attempts to define these two categories of smart contracts and provide the guidelines for wallets, apps and infrastructure providers.

# Prior matters

In TON contracts may exist in two states: initialized ("init") and uninitilized ("uninit").
Uninit contract silently receives TONs and accumulates them on its balance, while initialized contract processes the incoming message and performs arbitrary actions.

For safety, the sender may specify "bounceable" flag when sending a message: if it is set, then the message will "bounce back" in case the recipient is uninit or throws an exception.

[Original guidelines](https://github.com/ton-blockchain/ton/blob/master/doc/smc-guidelines.txt) from TON developer team:

>**3. Using non-bounceable messages**
>
>Almost all internal messages sent between smart contracts should be bounceable, 
>i.e., should have their "bounce" bit set. Then, if the destination smart contract
>does not exist, or if it throws an unhandled exception while processing this message, 
>the message will be "bounced" back carrying the remainder of the original value
>(minus all message transfer and gas fees). The bounced message will have the same body, 
>but with the "bounce" flag cleared and the "bounced" flag set. Therefore, all smart
>contracts should check the "bounced" flag of all inbound messages and either silently
>accept them (by immediately terminating with a zero exit code) or perform some special
>processing to detect which outbound query has failed. The query contained in the body
>of a bounced message should never be executed.
>
>On some occasions, non-bounceable internal messages must be used. For instance, new accounts
>cannot be created without at least one non-bounceable internal message sent to them.
>Unless this message contains a StateInit with the code and data of the new smart contract,
>it does not make sense to have a non-empty body in a non-bounceable internal message.
>
>It is a good idea not to allow the end user (e.g., of a wallet) to send unbounceable
>messages carrying large value (e.g., more than five Grams), or at least to warn them
>if they try this. It is a better idea to send a small amount first, then initialize
>the new smart contract, and then send a larger amount.


# Guide

Explain this document in simple language, as if you were teaching it to another developers. Give examples how your feature will work in real life.

# Specification

This section describes your feature formally. It contains requirements, which must be followed in order to implement your TEP. To keep things formal, it is convenient to follow [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt). You should include following text at the beginning of this section:

> The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

# Drawbacks

Why should we *not* do this?

# Rationale and alternatives

- Why is this design the best in the space of possible designs?
- What other designs have been considered and what is the rationale for not choosing them?
- What is the impact of not doing this?

# Prior art

Discuss prior art, both the good and the bad, in relation to this proposal. How the problem stated in "Motivation" section was solved in another blockchains? This section encourages you as an author to learn from others' mistakes. Feel free to include links to blogs, books, Durov's whitepapers, etc.

# Unresolved questions

If there are some questions that have to be discussed during review process or to be solved during implementation of this TEP, write it here.

# Future possibilities

Do you have ideas, which things can be implemented on top of this TEP later? Write possible ideas of new TEPs, which are related to this TEP.
