# Practical-Byzantine-Fault-Tolerance-In-Details
Practical-Byzantine-Fault-Tolerance-In-Details

<h1>Basic Concepts:</h1>

• Verifier: block verifier participant.<br/>
• Proposer: a block verification participant that is chosen to propose block in a consensus round.<br/>
• Proposal: A new block creation proposal which is undergoing consensus processing.<br/>
•Sequence: Sequence number of a proposal. A sequence number should be greater than all previous sequence numbers. Currently each proposed block height is its associated sequence number.<br/>
• Round: consensus cycle. A round starts with a proposer create a block proposal and ends with a block commitment or round change.<br/>
•	Round state: Consensus messages of a specific sequence and round, including pre-prepare message, prepare message, and commit message.<br/>
•	Consensus proof: The commitment signatures of a block that can prove the block has gone through the consensus process.
•	Snapshot: The validator voting state from last epoch.<br/>


<h1>Core (consensus)</h1>

<h2>Fault Tolerance</h2>
Seele BFT inherits from the original PBFT by using 3-phase consensus, PRE-PREPARE, PREPARE, and COMMIT. The system can tolerate at most of F faulty nodes in a N verifier nodes network, where N = 3F + 1. We only need at least 2F + 1 honest commits because (2F + 1)/ (3F + 1) > 2F / 3F = 2/3. <br/>

<h2>Process</h2>
Before each round, the verifiers will pick one of them as the proposer, by default, in a round robin fashion. <br/>

1).The proposer will then propose a new block proposal and broadcast it along with the PRE-PREPARE message.<br/>
2).Upon receiving the PRE-PREPARE message from the proposer, verifiers enter the state of PRE-PREPARED and then broadcast PREPARE message. This step is to make sure all verifiers are working on the same sequence and the same round.<br/>
3).While receiving 2F + 1 of PREPARE messages, the verifier enters the state of PREPARED and then broadcasts COMMIT message. This step is to inform its peers that it accepts the proposed block and is going to insert the block to the chain.<br/>
4).Lastly, verifiers wait for 2F + 1 of COMMIT messages to enter COMMITTED state and then insert the block to the chain.<br/>

<h2>Forks</h2>
Blocks in Seele BFT protocol are final, which means that there are no forks and any valid block must be somewhere in the main chain. To prevent a faulty node from generating a totally different chain from the main chain, each verifier appends 2F + 1 received COMMIT signatures to extraData field in the header before inserting it into the chain. Thus blocks are self-verifiable and light client can be supported as well. </br>

However, the dynamic extraData would cause an issue on block hash calculation. Since the same block from different verifiers can have different set of COMMIT signatures, the same block can have different block hashes as well. To solve this, we calculate the block hash by excluding the COMMIT signatures part. Therefore, we can still keep the block/block hash consistency as well as put the consensus proof in the block header.<br/>

<h2>States Transition</h2>

Because of orders of blocks and multiply consensus processing steps of one block, Seele BFT implement a state to maintain right order of steps to prevent messing up which may bring up security issues.

Here are what a state and how it works in details:</br>

•	NEW ROUND: Proposer to send new block proposal. Verifiers wait for PRE-PREPARE message.</br>
•	PRE-PREPARED: A verfier has received PRE-PREPARE message and broadcasts PREPARE message. Then it waits for 2F + 1 of PREPARE or COMMIT messages.</br>
•	PREPARED: A verifier has received 2F + 1 of PREPARE messages and broadcasts COMMIT messages. Then it waits for 2F + 1 of COMMIT messages.</br>
•	COMMITTED: A verifier has received 2F + 1 of COMMIT messages and is able to insert the proposed block into the blockchain.</br>
•	SEAL COMMITTED: A new block is successfully inserted into the blockchain and the verifier is ready for the next round.</br>
•	ROUND CHANGE: A verifier is waiting for 2F + 1 of ROUND CHANGE messages on the same proposed round number.</br>


![pbft](https://user-images.githubusercontent.com/29580346/65635071-7bff6600-df94-11e9-90c6-02012099f6f1.png)

