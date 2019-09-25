# Practical-Byzantine-Fault-Tolerance-In-Details
Practical-Byzantine-Fault-Tolerance-In-Details

## Baics Concepts

### Verifier: block verifier participant.


### Proposer: a block verification participant that is chosen to propose block in a consensus round.
### Proposal: A new block creation proposal which is undergoing consensus processing.

### Sequence: Sequence number of a proposal. A sequence number should be greater than all previous sequence numbers. Currently each proposed block height is its associated sequence number.

### Round: consensus cycle. A round starts with a proposer create a block proposal and ends with a block commitment or round change.
•	Round state: Consensus messages of a specific sequence and round, including pre-prepare message, prepare message, and commit message.
•	Consensus proof: The commitment signatures of a block that can prove the block has gone through the consensus process.
•	Snapshot: The validator voting state from last epoch.



