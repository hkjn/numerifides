# A Proposal for Proof of Authority Rooted on Decentralized Blockchains

# Foreward

Since the earliest conventions of early humans gathering into tribes, people have
put their trust in centralized authority.  Whether that authority is a seasoned
witchdoctor who is trained to spot illnesses and deliver healing, a Certificate
Authority organization that attests to the authenticity of their record, or a
government that derives its power from the will of the people—the fact remains
people delegate and revoke authority based on nebulous, inconsistent, and often
unfair and sometimes unjust rules.  Actors in an unjust system must exercise their
inherent power as components of the system to exit such a system as quickly as
possible so as not to be complacent in the system’s inherent injustice.  The
injustice of authority exercising power unevenly, or using its granted authority
in a way that harms the users of the system is the result of the underlying rules
governing how people delegate trust. It is said that justice is blind.  
Considering the rules inherent in our systems of justice, she must be, but we
need not trust the concept of justice to match its execution.

Rather than deriving justice and authority from a system that’s not supposed to
look but too often does, I propose a DECENTRALIZED CONSENSUS PROTOCOL that
enables a system of decentralized authority, whereby an user or actor can assert
identity, existence and authority on a public piece of data, on an open blockchain
and any independent, skeptical user or actor operating the consensus protocol
can verify any other actor’s statement of authority in a decentralized, fair and
privacy-protective manner.

I propose this system be called the Numerifides (capitalized) Trust Consensus
Protocol, or Numerifides (Nu-mer-ih-fih-dees) for short.  Transactions using this
system are to be called “numerifide(noun) transactions” (nu-mer-ih-fi’d) or
“numerifides (lowercase)” (nu-mer-ih-fi-dz) and trusted mappings to be referred
to as numerified(verb) numerifides(plural noun) or Numerifides transactions).
This is derived from the latin word numeris for number, and the Roman goddess
Fidēs, the goddess of trust and good faith.

# Outline

I propose a novel new transaction type, dubbed a “numerifide” transaction,
compatible with Bitcoin that allows a transaction to be created that creates
decentralized authority over a given “name” and associates user-supplied data to it.
In this way, a human-meaningful “name” can be registered in a decentralized way
that associates with a “value” that provides some utility, such as (but not limited to):

* Associating a specific username to a Bitcoin address to provide cryptographic
proof of ownership of the username, receiving Bitcoin funds, or optionally using
the Bitcoin key to sign messages.

* Advertising a human recognizable domain name→onion address mapping.

* Authenticating an “alias” for an Lightning Network node→node pubkey

* DNS -> IP mappings

In addition, the user can alter this data in a secure, uncensorable way via
already existing mining mechanisms present in Bitcoin as well as incentive and
disincentive structures I will outline below. Further, incentives are structured
such that “namesquatting” valuable names is disincentivized. This system creates
a fair and just way of reserving, updating, revoking via consensus and abandoning
names in a way that solves Zooko’s triangle of useful names being memorable,
secure and decentralized.

# Terminology used:

* NUMERIFIDES: The decentralized consensus protocol of TRUST described here.

* NUMERIFIDE: A transaction following the consensus rules of Numerifides.

* NUMERIFIED: A piece of data that exists on NUMERIFIDES that is TRUSTED according to consensus rules.

* TRUST: The method whereby a general mapping of names to data formalized in a “numerifide” transaction can be independently verified to be UNDISPUTED.

* TRUSTED: A “numerified” “numerifide” Numerifides transaction.  A valid, trusted, name mapping.

* MUST, MAY, SHOULD, SHOULD NOT, MUST NOT: Used as defined in RFC-21192

* CONSENSUS: The method by which the decentralized Numerifides consensus protocol agrees to the state of Numerifides

* CSV: CheckSequenceVerify – an encumbrance on spending funds until a certain number of Bitcoin (measured in blocks in a blockchain) have passed.

* UPDATE: The process whereby a name→data mapping can be changed or RENEWED by the owner of the private key locking the funds. As with usual cryptocurrency transactions, the funds are fully spent

* RELEASE(D)/EXPIRE(D): A name that is no longer TRUSTED due to expiry of the encumbering CSV.

* LOCK(ED): Funds that are unspendable until AFTER a specific period of time has passed measured in Bitcoin blocks.

* BEFORE, AFTER/SUBSEQUENT: Time as perceived by the succession of blocks on Bitcoin

* VALID: A numerifide that follows all consensus rules

* USER/ACTOR/NODE: A participant in the Numerifides consensus protocol that independently verifies protocol rules are being followed.

* PROOF OF WORK: A method of evaluating a resulting hash to be able to prove a specific amount of work was done to produce the hash.

* HODL: A drunken misspelling of HOLD.

# Technical proposal
In order to secure a human readable authoritative “name” out of the possible
“namespaces”, a user constructs and signs a transaction from an unspent output
address funding a “numerifide” transaction with the below Script:

```
<blocks> OP_CHECKSEQUENCEVERIFY OP_DROP <C> OP_CHECKSIG
```

Where <blocks> is between 144 (1 day) and 52560 (1 year) and <C> is a hash of a contract constructed like so:

1.  Generate a secret private key p = random() and the public key P = p * G.
2.  Encode the Numerifides command (see below)
3.  Compute the pay-to-contract public key: C = P + h(P || command) * G.  This has corresponding private key c = p + h(P || command) that only the user knows.
4.  Generate a P2WSH to the script: <52560 blocks> OP_CHECKSEQUENCEVERIFY OP_DROP <C> OP_CHECKSIG
5.  Pay to that P2WSH on the Bitcoin network.
6.  Broadcast command, P, and the txid+outnum of the UTXO that pays to the P2WSH above, to the Numerifides network after some number of blocks

(Numerifides transactions such as this SHOULD also enable Replace-by-fee (RBF) on the funding transaction for reasons outlined below.)

The "command" is a name->data mapping that also includes a second extra portion for a nonce.

Example: "google.com:127.0.0.1:nonce=11111111111111111"

The nonce is incremented and many transactions are produced and signed until the
resulting TXID meets a minimum proof of work acceptable to the user. Since Proof
of Work and Proof of Hodling are BOTH used to determine the level of TRUST, a user
registering a popular or contentious name should probably lock up significant funds
and ALSO "mine" their name out of the reach of anyone else.

The nonce is NOT evaluated as part of the mapping when checking for duplicates.
For example, although the nonces are the same, the data is different and thus the mapping is different.

"google.com:127.0.0.1:nonce=1111"

"google.co:127.0.0.1:nonce=1111"

A user will broadcast the transaction, wait for it to be mined for a few blocks,
and then announce it and the mapping they registered later so that miners cannot
censor registrations.

# NUMERIFIDES RULES OF CONSENSUS

Numerifides mappings use the following formula to determine TRUST, with the winning
transaction being FULLY TRUSTED and any other transactions being UNTRUSTED (through
the user MAY be notified about the untrusted mappings outside of the protocol itself)

```
( T * P ) + ( T * B ) = TRUST
```

Where:
```
T = Timelock length (CheckSequenceVerify)
P = Proof of Work
B = Bitcoin locked up.
```

# EXAMPLE REGISTRATIONS

A user that locks up 1BTC for 144 blocks and provides a Proof of Work of 2 would then
have a TRUST level of 432.

`( 144 * 2 ) + ( 144 * 1 ) = 432`

A user that wishes to "take over" the name (keeping the locktime the same),
but doesn't have as much Bitcoin could mine until a 3 and "unseat the name" with
any amount of bitcoin above 1 (0.5 in this example):

`( 144 * 3 ) + ( 144 * 0.5 ) = 504`

In this way, names are secured primarily by locktime and Proof of Work, but secondarily
by amount of Bitcoin locked up.

If the previous user wanted to secure their name with the same Proof of Work and
amount locked up, she simply could increase the locktime to the max of 1 year:

`( 52,560 * 2 ) + ( 52,560 * 1 ) = 157,680`

The previous user, if wishing to unseat this name would need to also increase the locktime
of their funds to 1 year, beat the Proof of Work and then provide any amount of Bitcoin.

`( 52,560 * 3 ) + ( 52,560 * >0 ) = 157,680+`

# PROPOSED DATA ENCODING TABLE:

Mappings should be [datatype][name][separator][data].

( Separator is proposed to be 0xFF but others could be used )

Constant length 2+-byte data field | Proposed Data Standard                 | Separator | Description
-----------------------------------|----------------------------------------|-----------|-------------
00                                 | Private Usage                          | 0xFF      | The data associated with the name has no explicitly stated purpose. The data SHOULD NOT be used to verify any other consensus-approved data standard.
01                                 | Bitcoin Address                        | 0xFF      | The data associated with the name SHOULD BE used to TRUST that the Bitcoin address is controlled by the name it is associated to.
02                                 | Lightning Node Public Key              | 0xFF      | The data associated with the name SHOULD BE used to TRUST an advertised ALIAS of a Lightning node.
03                                 | GPG Public Key                         | 0xFF      | The data associated with the name SHOULD BE used to TRUST a GPG Public Key
04                                 | Domain-validated Certificate           | 0xFF      | The data SHOULD BE a domain certificate for the specific name in in DOMAIN.TLD format.
05                                 | DNS mapping                            | 0xFF      | The data SHOULD BE a valid IP address.
06-FF                              | Future use decided upon via consensus. | 0xFF      | Future use

# Implementations
Implementations of the Numerifides Trust Consensus Protocol will fall into two categories:
"Full" nodes and "Light" nodes.

A Full node will have all of the known mappings, and be able to perform lookups locally.

A Light node will query Full nodes for the transactions and mappings, and then confirm
via a lookup on the Bitcoin network that the related transactions are valid according
to consensus rules. Light nodes should query more than one Full node for a lookup,
and likewise query more than one Bitcoin node to confirm the anchoring Numerifides transaction.

Plugins can be written to interface the client into any legacy lookup standard such as
creating a virtual Certificate Authority for the system that vouches for certificates
looked up via Numerifides, or for DNS lookups.

# Economic rationale:

Any user that wishes to register a name must commit scarce resources to do so:

* Making a variable amount of Bitcoin unspendable for a time
* Processing time in the form of Proof of Work

This disincentivzes "namesquatting" because any user that wishes to register a name
they do not intend to use will lose the use of their Bitcoin, and a user more interested
in the name could commit more Bitcoin or more Proof of Work and "steal" the name.

A user that registers a name no one cares to unseat can commit very little Bitcoin
and provide a low Proof of Work, and register the name for a short time if they wish.

A user with a very popular or contentious name mapping (example: DNS for google.com)
should:

1. Do as much Proof of Work as possible.
2. Provide the longest locktime possible.
3. Lock as much Bitcoin as possible

In this order.

Miners can only choose to attempt to censor the whole network, not individual mappings.
Users locking up Bitcoin make the miner's Bitcoin more valuable so unless there is
external pressure, miner's should be incentivized to encourage Numerifides transactions.


# Example Numerifides registration

User wishes to register the name:data mapping of "03:Numerifides:FF:[GPG key]".
User creates the appropriate transaction that:

* Pays back to themselves .001 BTC
* Locks the Bitcoin for a duration of 52,160 blocks (1 year)
* Has an appropriate Proof of Work advertised via the TXID hash

User broadcasts this transaction and it is included in a block.  User waits
6 blocks, and then broadcasts to the Numerifides network the mapping she just
registered.

The user was the first to broadcast this name, so any full or light node that does
a lookup for a GPG record for Numerifides should get this new record.

Since no other user is interested in registering this name, the Proof of Work
is enough to secure the name.

# Known issues

Storage of all mappings is rooted on the blockchain, but gossip about new mappings
could be partly blocked or censored.  This is also true of Bitcoin itself, so once
the network is of sufficient size and fault-tolerance, it should be increasingly
difficult to fully prevent a numerifide from being gossiped about.

The data is like a blockchain itself, but without links between the numerifides
transactions.  If the network forgets about a mapping, it would have to be
gossiped about again.

Storage of the mappings could get quite burdensome as users may want to register
large data mappings.

# REFERENCES
https://en.wikipedia.org/wiki/Zooko%27s_triangle

https://www.ietf.org/rfc/rfc2119.txt
