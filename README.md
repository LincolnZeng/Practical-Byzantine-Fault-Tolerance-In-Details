# Practical-Byzantine-Fault-Tolerance-In-Details
Practical-Byzantine-Fault-Tolerance-In-Details

Baics Concepts

• Verifier: block verifier participant.<br/>
• Proposer: a block verification participant that is chosen to propose block in a consensus round.<br/>
• Proposal: A new block creation proposal which is undergoing consensus processing.<br/>
•Sequence: Sequence number of a proposal. A sequence number should be greater than all previous sequence numbers. Currently each proposed block height is its associated sequence number.<br/>
• Round: consensus cycle. A round starts with a proposer create a block proposal and ends with a block commitment or round change.<br/>
•	Round state: Consensus messages of a specific sequence and round, including pre-prepare message, prepare message, and commit message.<br/>
•	Consensus proof: The commitment signatures of a block that can prove the block has gone through the consensus process.
•	Snapshot: The validator voting state from last epoch.<br/>



