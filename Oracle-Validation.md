# Client-Side Oracle Validation

# Table of Contents

* [Abstract](#abstract)
* [General Requirements](#general-requirements)
* [Oracle-Discovery](#oracle-discovery)
* [Oracle-Validation](#oracle-validation)


## Abstract

The DLC protocol relies on trust-minized oracles mapping real-world events to signatures, those ones
then confidentialy re-mapped by participants to a range of contract outcomes. A participant expected
execution of the DLC is thus function of the oracle well-behaving as announced before event
realization. In this current protocol version, oracle behavior is assumed to be trusted. This level
of trust is expressed by the client oracle selection policy, and should faithfully reflect user
oracle preferences.

Oracle signature usage requires confidence that the event associated public key is owned by the
correct remote subject (person, system or organization). The ownership is asserted by having the
client verifying a signature which is bound to an identity anchor (domain name, LN node pubkey, ...).The evaluation of identity anchor correctness is beyond the scope of this protocol. A DLC client may
use this identity anchor as a descriptor for a reputation system.

# Oracle Discovery

Actually, no oracle discovery protocol support is mandated. The following requirements are generic
requirements, which should be swayed from only if protocol has higher built-in privacy mechanisms.

## Requirements

A node:
* MUST fetch oracle pubkey.
* MUST fetch any oracle pubkey available.
* MUST persist its keyring across restart/shutdown.

## Rationale

A DLC client should desynchronize any oracle pubkey fetching from DLC contract setting, otherwise
the fetched entity may deduce an ongoing usage of oracle events. 

A DLC client should fetch any oracle pubkey available to mask better its actual orage usage.

Persistenting keyring is minimizing oracle pubkey fetching which is likely a privacy hindering task.

# Oracle Event Authentication

Oracle event authentication happens at `offer_dlc` reception. It ensures the offered DLC event is
effectively to be announced by a previously identified oracle.

## Requirements

A node:
* MUST verify `oracle_info` signature against the claimed oracle pubkey
* MUST verify `oracle_announcement` signature against the negotiated oracle pubkey

TODO: does `oracle_info`/`oracle_announcement` contain oracle identifier/pubkey/signature ?

## Rationale

Identifying oracle pubkey against an identity anchor prevents usage of event signer disallowed by
the client oracle selection policy.

Authenticating oracle event prevents time-value DoS where a DLC is entered relying on a known honest oracle which doesn't have committed to the DLC event signing.


TODO: sybil attack where honest connections to Olivia are censored until `refund_tx` is valid ?

# Authors

Antoine Riard <antoine.riard@gmail.com>

TODO: License
