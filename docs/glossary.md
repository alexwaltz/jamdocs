# Glossary

Unfortunately, there is no way to talk about bitcoin privacy and the intricacies
of JoinMarket without a little bit of jargon.

This glossary explains some of the concepts and terms used in Jam.

---

<center>
[
[A](#a) ·
[B](#b) ·
[C](#c) ·
[D](#d) ·
[E](#e) ·
[F](#f) ·
G ·
H ·
I ·
[J](#j) ·
K ·
L ·
[M](#m) ·
N ·
[O](#o) ·
[P](#p) ·
Q ·
R ·
[S](#s) ·
[T](#t) ·
[U](#u) ·
V ·
W ·
X ·
Y ·
Z
]
</center>

---

## A

### Address

A bitcoin address—also called bitcoin invoice address[^bip179]—is a string of
characters that you send to someone else to receive funds.

There are currently three invoice address formats in use:

- [P2PKH](#p2pkh) starting with `1`
- [P2SH](#p2sh) starting with `3`
- [Bech32](#bech32) starting with `bc1`

For more details please refer to the [Bitcoin Wiki][wiki-addr]. Jam defaults to
Bech32 addresses.

[wiki-addr]: https://en.bitcoin.it/wiki/Invoice_address

[^bip179]: The term *invoice* is proposed in [BIP-179](https://github.com/bitcoin/bips/blob/master/bip-0179.mediawiki) as an alternative.

### Address Reuse

Address reuse refers to the practice of reusing a single bitcoin [invoice
address](#address) for multiple transactions. It is highly discouraged since it
harms the privacy of yourself and others. You should never reuse addresses.

[:octicons-arrow-right-24: Bitcoin Wiki: Address Reuse][reuse]

[reuse]: https://en.bitcoin.it/wiki/Address_reuse

### Anonymity Set

The only way to hide in plain sight is by hiding in a crowd. The anonymity set
is simply the size of this crowd.

If you are putting on a hat and sunglasses to hide in a crowd of people that are
wearing the same hat and sunglasses, your anonymity set is the size of this
crowd. If said crowd is 50 people, an attacker could mistake you for anyone out
of these 50 people. Similarly, when doing a [collaborative
transaction](#collaborative-transaction) in bitcoin, you are creating an
[onchain](#onchain) footprint that is equivalent to the onchain footprint of
others.

## B
### Base Layer

Bitcoin's base layer (or _Layer 1_) refers to any transaction
that touches the [timechain](#timechain), i.e. all [onchain](#onchain)
transactions. The base layer is concerned with global consensus and settlement.

Higher layers build ontop the settlement assurrances of the base layer. One
example of a _Layer 2_ system is the Lightning Network. All money is layered,
including gold and fiat monies. Read _Layered Money_ by Nik Bhatia to understand
this in more detail.


[:octicons-arrow-right-24: Layered Money][layered-money]

[layered-money]: https://bitcoin-resources.com/books/layered-money

### Batch Transaction

A batch transaction is a transaction that combines multiple real-world
transactions into one on-chain transaction. It has multiple inputs and multiple
outputs. Different parties may control one or multiple inputs and receive one or
multiple outputs.

[:octicons-arrow-right-24: Privacy Fundamentals][fundamentals]

[fundamentals]: privacy/01-fundamentals.md

### Bech32

Bech32 is an [invoice address](#address) format. It was specified in [BIP
173][bip173] and is used for both SegWit and Taproot addresses.

[bip173]: https://en.bitcoin.it/wiki/BIP_0173

### Blockchain

A mostly meaningless marketing term used to confuse newcomers. While "[block
chain][block-chain]" is still the main term used to refer to Bitcoin's linked
data structure of blocks, it has increasingly become meaningless over the years
as it got co-opted by grifters, charlatans & marketers. A better, more
descriptive term of said data structure is [timechain](#timechain).

[block-chain]: https://developer.bitcoin.org/devguide/block_chain.html

## C
### Chain Analysis

Chain analysis is is the practice of applying heuristics to a
[timechain](#timechain)'s public transaction graph. The goal of chain analysis
is to link the pseudonymous identities created by bitcoin software with "real"
identities corresponding to natural persons or entities.

Chain analysis is based on flawed assumptions, because ownership in bitcoin is
defined by secret knowledge, not possession. Identities can only be linked
probabilistically, not provably. One of the main assumptions used by chain
analysis companies is the [common input ownership
heuristic](#common-input-ownership-heuristic). Collaborative transactions break
this heuristic.

### CoinJoin

A CoinJoin is a collaborative transaction that combines inputs from multiple
parties. The purpose of a CoinJoin is to combine inputs and create outputs in
ways that improves the financial privacy of participants, without relying on a
trusted third party for custody. When done correctly, a CoinJoin breaks any
deterministic links between transactions, moving the process of [chain
analysis](#chain-analysis) from quasi-deterministic with high certainty to
probabilistic with low certainty.

CoinJoins break the [common input ownership
heuristic](#common-input-ownership-heuristic) that is used by chain analysis
companies to de-anonymize actors. The concept was introduced in 2013 by Gregory
Maxwell.[^gmaxwell1][^gmaxwell2]

All CoinJoin transactions are collaborative transactions. The two main types of
CoinJoin transactions are [equal-output](#equal-output-coinjoin) and
unequal-output CoinJoins. Other differences might be interactivity (or lack
thereof) and number of participants.

[^gmaxwell1]: [I taint rich!](https://bitcointalk.org/?topic=139581) Maxwell, Jan. 2013
[^gmaxwell2]: [CoinJoin: Bitcoin privacy for the real world](https://bitcointalk.org/?topic=279249) Maxwell, Aug. 2013


### Collaborative Transaction

A collaborative transaction is a bitcoin transaction that is initiated and
signed by multiple participants. A collaborative transaction involves two or
more parties and is thus always a [batch transaction](#batch-transaction). All
CoinJoins are collaborative transactions.

### Change

When using physical cash, spending a $100 bill to pay for a $25 item, you will
get $75 back in change. The reason for this is that you can't spend just a part
of the bill, because ripping off a quarter of it for payment will invalidate the
bill. Consequently, when bills change hands, the whole bill has to be spent, and
an appropriate amount of change goes back to the spender.

Bitcoin works the same way. When sats change hands, the spender has to spend the
whole [UTXO](#utxo). Bitcoin creates the appropriate amount of change
automatically. Because all transactions are recorded transparently and publicly on the [timechain](#timechain),

As of this writing,[^744811] a [simple spend](#simple-spend) is
the most common transaction type, which describes the example given above: one
input ($100 bill) produces two outputs: $25 for the merchant, and $75 in change.
The act of an outside observer guessing whether you bought something for $25 or
$75 (or: who was the customer of the transaction, and who was the merchant) is
what is called [change detection](#change-detection).

[^744811]: Block 744,811

### Change Detection

When it comes to [chain analysis](#chain-analysis), change detection is the name
of the game. The goal of chain analysis companies is to link identities to
transactions, and to do that, one has to detect whether funds changed hands or not.

Technically speaking, change detection is trying to figure out which output of a
transaction is a change output. Change detection is based on various heuristics.
False positives will always exist, even if the transaction under scrutiny is a
[simple spend](#simple-spend). Consequently, it is more an art than a science.

To quote one special investigations team speaking on chain analysis:

> Attributing ownership, however, is often nuanced because outside observers can
> only infer it depending on factors such as availability and quality of the
> evidence. Evidence means proof that indeed an address belongs to an individual
> or entity. Unless you own an address yourself, it is very difficult to say
> with absolute certainty who an address is owned by. This is why it’s more
> fitting to consider blockchain analytics more of an art than science.
>
> <cite>[CSIT](https://archive.ph/1WtkW)</cite>

Keep in mind that ownership can change without any on-chain transaction
happening, e.g. by passing on a private key directly. It is also possible that
amount to be paid lines up perfectly with a single UTXO, meaning that what looks
like a self-spend is actually a payment.

One can only conclude, as is also mentioned in the report linked above, that "an
external observer cannot possibly gain a full picture or claim 100% confidence
in ownership attribution."

[:octicons-arrow-right-24: Privacy Fundamentals: The Bitcoin Transaction][fundamentals-tx]

[:octicons-arrow-right-24: Bitcoin Wiki: Change Detection][change-detection]

[fundamentals-tx]: privacy/01-fundamentals.md#the-bitcoin-transaction
[change-detection]: https://en.bitcoin.it/wiki/Privacy#Change_address_detection

### CIOH

Short for [Common Input Ownership Heuristic](#common-input-ownership-heuristic).

### Common Input Ownership Heuristic

The common input ownership heuristic assumes that all inputs of a transaction
are controlled by a single entity. This assumption is clearly wrong, because
[collaborative transactions](#collaborative-transaction) exist.
[CoinJoin](#coinjoin) transactions are designed to break this heuristic.

## D
### Doxxic Change

"Doxxic" change is any leftover change that is going back to you when
participating in an [equal-output CoinJoin](#equal-output-coinjoin). Doxxic
change is problematic because it can potentially destroy any privacy benefits
gained from a CoinJoin.  

The word is a combination of "toxic" and "doxxing." Doxxing is the act of finding
out the legal identity (or similar identifiying information) of a pseudonymous
entity. Bitcoin is a pseudonymous system and does not require the *True
Names*[^true-names] of participants.

[^true-names]: Vernor Vinge, 1981, [True Names](https://bitcoin-resources.com/books/true-names)

You can use the [sweep](/interface/04-sweep) functionality to avoid doxxic
change in Jam.

## E
### Eclipse Attack

An eclipse attack is an attack in networked systems in which an attacker targets
a node to convince it of a false network state. The attacker surrounds the node
in order to conceal or block off accurate information, leaving the attacked
node(s) in the dark in terms of the global network state.

An eclipse attack is similar to a [Sybil attack](#sybil-attack).

[:octicons-arrow-right-24: Bitcoin Stack Exchange: Eclipse Attack vs. Sybil Attack](https://bitcoin.stackexchange.com/a/61154/93857)

### Equal-Output CoinJoin

An equal-output CoinJoin is a collaborative transaction that produces outputs of
equal denomination, i.e. outputs of equal value when measured in sats. For this
reason, these types of transactions are also referred to as equal-amount or
equal-value CoinJoin transactions.

Equal-output CoinJoins are [batch transaction](#batch-transaction) that are
easily identifiable [on-chain](#onchain) due to their uniform output structure.

Like any collaborative transaction, an equal-output CoinJoin breaks the [Common
Input Ownership Heuristic](#common-input-ownership-heuristic). Participating in
such a transaction allows you to gain privacy by "hiding in the crowd."

Jam defaults to collaborative transactions when [sending][send] and
[sweeping][sweep], creating equal-output CoinJoins.

[send]: /interface/02-send

## F
### Fidelity Bond

A fidelity bond is an insurance policies which protects the policyholder from
wrongful acts committed by others. The term comes from the world of business and
finance, thus the policyholders are usually companies, and the other parties are
usually employees.

In JoinMarket, a fidelity bond is a mechanism which ensures that market actors
act honestly. It is a protection mechanism against [Sybil
attacks](#sybil-attack), because a fidelity bond makes the creation of
cryptographic identities costly.

!!! warning
    It is impossible to move or spend funds that are locked in a fidelity bond
    before the bond expires. They **cannot be used in collaborative transactions**
    as fidelity bonds are time-locked by the Bitcoin protocol.

Fidelity bonds improve the privacy guarantees of the whole system and increase
your chance of being chosen as a [market maker](#maker) drastically.

[:octicons-arrow-right-24: What Are Fidelity Bonds and How Do They Work in JoinMarket?][se-fb]

[:octicons-arrow-right-24: JoinMarket: Fidelity Bonds][jm-fb]

[:octicons-arrow-right-24: JoinMarket: Financial Mathematics of Fidelity Bonds][jm-fb-math]

[:octicons-arrow-right-24: Creating a Fidelity Bond in Jam][jam-fb]

[se-fb]: https://bitcoin.stackexchange.com/a/106189/93857
[jm-fb]: https://github.com/JoinMarket-Org/joinmarket-clientserver/blob/master/docs/fidelity-bonds.md
[jm-fb-math]: https://gist.github.com/chris-belcher/87ebbcbb639686057a389acb9ab3e25b
[jam-fb]: /interface/fidelity-bonds/


## J
### Jam

Jam is both [the name][name] of the project and the verb we use when a
collaborative transaction is in progress.

> Sir! The radar, sir! It appears to be... [Jam starts flowing through the
> computer screen] jammed!
>
><cite>Spaceballs</cite>

[:octicons-arrow-right-24: About][name]

[:octicons-arrow-right-24: jamapp.org][jamapp-org]

[name]: about.md
[jamapp-org]: https://jamapp.org

### Jar

In Jam, a "jar" is a container that holds some [sats](#sats) of yours.[^mixdepths] Jars
exist to segregate your sats into multiple buckets that are disconnected from
each other, which aids privacy. To not risk any privacy degradation, you can
only spend from one jar at a time.

There are 5 jars by default. The default jar to receive funds is **Jar A**.

[^mixdepths]: What we call "jars" are usually called "mixdepths" in JoinMarket. They are also referred to as "pockets" and "accounts" in some of the older parts of the JoinMarket documentation.

## M
### Maker

A market maker is someone who offers bitcoin liquidity to the market, to be used by
others for collaborative transactions. You can create an offer via the ["Earn"
tab][earn] and become a market maker.

[:octicons-arrow-right-24: Earn Screen][earn]

[:octicons-arrow-right-24: The Maker Role][jm-maker]

[earn]: interface/03-earn.md
[jm-maker]: https://github.com/openoms/bitcoin-tutorials/blob/master/joinmarket/joinmarket_private_flow.md#the-maker-role

### Mempool

Short for 'memory pool.' A pool of valid bitcoin [transactions](#transaction)
held by each node, that are not yet confirmed in the [timechain](#timechain).

[:octicons-arrow-right-24: Mempool][mempool]

[mempool]: /market/mempool/

## O
### Offchain

An offchain (or off-chain) transaction is any transaction that does not happen
[onchain](#onchain). Examples include transactions on the Lightning network, the
handing over of physical private keys, as well as transactions on centralized
ledgers such as exchanges and other trusted third parties.

Some offchain transactions, such as handing over private keys on USB sticks or
similar, are outside of the scope of [chain analysis](#chain-analysis).
Consequently, chain analysis can never be completely accurate.

### Onchain

An onchain (or on-chain) transaction is a bitcoin transaction that is settled on
the bitcoin [timechain](#timechain). The analysis of these transactions is
called [chain analysis](#chain-analysis), which is the spying on and
de-anonymizing of bitcoin's users by trying to find patterns in the onchain
data.

## P
### P2PKH
Short for pay-to-public-key-hash.

[:octicons-arrow-right-24: Learn Me A Bitcoin: P2PKH][p2pkh]

[p2pkh]: https://learnmeabitcoin.com/technical/p2pkh

### P2SH
Short for pay-to-script-hash.

[:octicons-arrow-right-24: Learn Me A Bitcoin: P2SH][p2sh]

[p2sh]: https://learnmeabitcoin.com/technical/p2sh

### PayJoin

A PayJoin is a collaborative transaction between two parties that facilitates an
actual payment for a good or service. The merchant agrees to provide one or
multiple inputs to the transaction, increasing the privacy of both parties.

A PayJoin has different [onchain](#onchain) characteristics than an
[equal-output CoinJoin](#equal-output-coinjoin) and is not detectable via [chain
analysis](#chain-analysis) alone. A PayJoin transaction has multiple
interpretations, all of which are equally valid. In addition to breaking the
[common input ownership heuristic](#common-input-ownership-heuristic) it poisons
the basis of all [chain analysis](#chain-analysis), because of its
undetectability.

[:octicons-arrow-right-24: Bitcoin Wiki: PayJoin][wiki-payjoin]

[:octicons-arrow-right-24: Bitcoin Wiki: PayJoin Adoption][wiki-payjoin]

[wiki-payjoin]: https://en.bitcoin.it/wiki/PayJoin
[wiki-payjoin-adoption]: https://en.bitcoin.it/wiki/PayJoin_adoption

## S
### Sats
Short for *satoshis*, plural of *sat* (satoshi).

A sat the smallest fraction of a bitcoin that can be expressed
[on-chain](#onchain). There are `100,000,000` sats in a bitcoin. There are
multiple currency symbols for a sat emerging. In Jam, the *sat
symbol*[^satsymbol] is used:
<i class="fak fa-satoshisymbol-solidtilt"></i>

[^satsymbol]: [satsymbol.com](https://satsymbol.com/)

### Scheduler

The 'scheduler' is a component of JoinMarket which schedules multiple
[collaborative transactions](#collaborative-transaction) in a row. It will use
random intervals and amounts to make timing analysis and de-anonymization via
[chain analysis](#chain-analysis) more difficult.

The '[Sweep](#sweep)' screen uses the scheduler to sweep funds to one or
multiple external wallets under your control.

### Simple Spend

A transaction that splits one input into two outputs: payment and change.

The "simple spend" is the most common bitcoin transaction type to date, since
most wallet implementations do not optimize for privacy. For most simple spend
transactions it is possible to discern which output is the actual payment or
which one is the change. This [change detection](#change-detection) makes
the tracing of funds possible.

[:octicons-arrow-right-24: Privacy Fundamentals: The Bitcoin Transaction][fundamentals-tx]


### SNICKER
Simple Non-Interactive Coinjoin with Keys for Encryption Reused.

[:octicons-arrow-right-24: SNICKER blog post][snicker]

[snicker]: https://archive.ph/UlvPn

### Sybil Attack

A Sybil attack is a special kind of attack in peer-to-peer networked computing.
The victim is surrounded by malicious entities, each of which act as if they are
a separate entity. The victim believes that everything is in order, that he is
getting an accurate state of the network from multiple independent peers. In
actuality, the Sybil attacker controls all entities surrounding the victim.
Consequently, the attacker is able to trick the victim into accepting a
malicious network state.

JoinMarket uses [fidelity bonds](#fidelity-bond) to protect users from Sybil attacks.

[:octicons-arrow-right-24: Design for Improving JoinMarket's Resistance to Sybil Attacks Using Fidelity Bonds][fb-design]

[:octicons-arrow-right-24: Wikipedia: Sybil Attack][w-sybil]

[w-sybil]: https://en.wikipedia.org/wiki/Sybil_attack
[fb-design]: https://gist.github.com/chris-belcher/18ea0e6acdb885a2bfbdee43dcd6b5af

### Sweep

A 'sweep' send will transfer all funds of a jar or all funds of your wallet. To
sweep all funds from your wallet multiple transactions are created using the
[scheduler](#scheduler).

[:octicons-arrow-right-24: 'Sweep' Screen][sweep]

[sweep]: interface/04-sweep.md

## T
### Taint

Taint is an external property defined by [chain analysis](#chain-analysis)
surveillance firms. It is an attempt to trace the origin and ownership of
[UTXOs](#utxo), classifying their owners and building lists of "good" and "bad"
money.

All taint in bitcoin always is and always will be arbitrarily defined and
probabilistically applied. Just like [change detection](#change-detection), it
is more an art than a science. It can never be done exactly. False positives and
false negatives will always arise.

The bitcoin network is indifferent to taint. Miners will include any transaction
in a valid block that will reward them economically. Any application of taint on
the block production level is equivalent to imposing additional rules via a
soft-fork, which fully validating nodes can reject.

### Taker

A market taker is someone who buys bitcoin liquidity from the market, taking up
[market makers](#maker) on their offers. You can see active offers in the [order
book][orderbook].

You will automatically take offers when running the [scheduler](#scheduler) via
the ["Sweep"][sweep] functionality.

[:octicons-arrow-right-24: Sweep][sweep]

[:octicons-arrow-right-24: Earn Screen][earn]

[:octicons-arrow-right-24: The Taker Role][jm-taker]

[orderbook]: market/orderbook.md
[jm-taker]: https://github.com/openoms/bitcoin-tutorials/blob/master/joinmarket/joinmarket_private_flow.md#the-taker-role

### Timechain
The data structure of Bitcoin's [base layer](#base-layer). It represents a timestamped and linked list of blocks. Anyone can create a new block, but each block must have
sufficient proof of work, making it costly to create new blocks.

[:octicons-arrow-right-24: Bitcoin is Time][time]

[time]: https://dergigi.com/time

### Timelock
A UTXO can be locked up by a script which defines that said UTXO can only be spent in a block that
is higher than a certain value. This makes UTXOs unspendable before a specific time.

### Transaction

A bitcoin transaction describes the movement of [sats](#sats). It is structured
data that describes inputs and outputs, among other things. A valid bitcoin
transaction has at least one input and at least one output. Every transaction
input refers to the output of a previous transaction. We say that an input
"consumes" an output. If an output is not consumed yet, we speak of an unspent
transaction output, or [UTXO](#utxo), for short.

[:octicons-arrow-right-24: Privacy Fundamentals: The Bitcoin Transaction][fundamentals-tx]

[:octicons-arrow-right-24: Bitcoin Wiki: Transaction][wiki-tx]

[:octicons-arrow-right-24: Learn Me a Bitcoin: Transactions][lmabtc-tx]

[wiki-tx]: https://en.bitcoin.it/wiki/Transaction
[lmabtc-tx]: https://learnmeabitcoin.com/beginners/transactions

### TX
Short for [transaction](#transaction).
## U
### Unspent Transaction Output
The tip of the chain of signatures which originates in a coinbase output. It's a "coin" that
has not yet been spent and can still be spend.
### UTXO
Short for [Unspent Transaction Output](#unspent-transaction-output).
