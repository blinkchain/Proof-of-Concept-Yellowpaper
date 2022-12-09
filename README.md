
# Blinkchain - Proof of Concept for Development

Contributors: [@jobyreuben](https://www.github.com/jobyreuben),  [@I-Corinthian](https://github.com/I-Corinthian)

## Development Map

Download PDF version [here](https://blinkchain.org/map.pdf)


# Objectives
1. Whitepaper Section, Passive or Active Program \& Level
   - Active: Constantly running, looking for triggers
   - Passive: Active per time-frame or triggered
   - Chain - Ledger,Consensus and Core Implementations
   - Script - UTXO scripts/proofs construction and attesting
   - OffChain - Client Side construction/propagation
   - Node - Validation, Ledger Outlook \& Parameter construction
2. Process, Algorithm and Mathmatical Data
3. Existing Implementations and Documentation References
4. Feasibility of Development \& Notes
5. Technical \& Non-Technical Challenges
6.  Alternatives Offered \& Outcomes

# Time Architecture
*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*
- Whitepaper Section [2.1], Level : Node
- The Time Architecture in Blinkchain is segregated into Epoch = 10,000 blocks; Slot = 400 blocks ; Packet = 1 block.
- These time frames are not correlated to the ledger, as it only knows block heights. It is only taken in the following area
- Election conducted every epoch (10,000 blocks)
- Announcing Leaders for every Epochs, Slots and Packets
- Taking Variable Data to form constraints in the consensus e.g., Total Volume in an Epoch, Each individual block time in an epoch/slot, etc
- Cardano, a UTXO based blockchain uses these timeframes, thus it is implemented and running https://developers.cardano.org/docs/stake-pool-course/introduction-to-cardano/#slots-and-epochs
- Its feasibility is proved with previous implementations and it does not affects or changes consensus protocols. As block heights are only taken for constraints, these time frames - Epoch, Slots and Packets are quasi and can be much more human readable. The alternatives would be reciting all constraints in block heights which cannot be developer friendly. The outcome can be achieved seamlessly.

<!---*[@username-here](https://www.github.com/[username-here]) "Role-here" Comment* 
--->

# Epoch Election

Epoch Election conducted every 10,000 blocks (1 Epoch) to announce packet leaders who are eligible to produce their blocks. Block producers are announced before the epoch starts. The following steps starts from attesting proofs to participate in the election to getting selected to mint a block to commencing the epoch.


## Producer Arrival

- Producer arrival represents new nodes arrival to contest in the Epoch election to produce new blocks. For Arrival a specific slot is allocated range of x to y block heights in every epoch for the next epoch production. 
- Existing and new producers carry out same process, but new nodes tend to have a criteria to be fulfilled, whereas the existing nodes does infact proved passing the criteria for the previous epoch.

### Vote of Confidence Result


### Selection of Bandwidth Proof

*Bandwidth Proofs* - to be proposed in a new paper "Blink Proofs" will provide a zero knowledge proof to the verfiers (nodes) to calculate its authenticity and ability to contest in the new election to directly influencing to change the block size of the epoch.

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*

- Whitepaper Section - Nil, Passive Program, Level : Node
- Bandwidth Proofs in the ledger in a range of block height X to Y (Denoting Slot opening and ending for Arrival of New nodes) are selected and validated to carry on further steps. Since proofs are available directly in the ledger, it is easier to select the proofs. Only recent proofs are taken and the requirement should be fulfilled.
- The requirement will be published on after every epoch's Vote of Confidence motion.
- The implementation can be done effectively with optimized language suitable.

*[@I-Corinthian](https://github.com/I-Corinthian) Contributed Algorithm*

```
algorithm here;
```

### Node Weight Calculation 

- Whitepaper Section 3.2 - 3.2.1, Active, Level: Node
- Node Weights represent, each node's current honesty weight. For every epoch along with the Bandwidth Proof, nodes are required to attach their updated weight which can be validated if true.
![nodeweightsum](https://github.com/blinkchain/Proof-of-Concept-for-Development/blob/main/equations/nodeweightsum.jpg)

- All of selected bandwith proof's node's weights are summed up to find total packets each node can produce
![nodeweightsum](https://github.com/blinkchain/Proof-of-Concept-for-Development/blob/main/equations/totalpackets.jpg)

- If Total packets allocated < 10000, then the remaining packets are allocated to nodes with highest positive weight excluding the bandwidth proof.
- Challenges will be script development for Node weight snapshot and validation of it, which also can be effectively achieved.

*[@I-Corinthian](https://github.com/I-Corinthian) Contributed Algorithm*

```
algorithm here;
```


### Allocation of Leaders

- Whitepaper Section 3.2.2 - 3.2.3, Passive, Level: Node

   
## Block Size \& Time 

Short Intro

### Block Size per sec Fixing (Chain)

1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
### Block Time Fixing (Chain)
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
### Per Block Size Fixing (Node)
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives

## Vote of Confidence 

- Whitepaper Section 3.1
- To scale the blink-network and avoid latencies due to incompetance to propagate transactions faster, the network decides on a vote to kick un-fit nodes as per its requirement. Vote of Confidence involves selection, voting, and elimination by ignoring it's bandwidth proofs for becoming a contestant in the election.

### Selection of Un-fit Nodes (Node)

- Whitepaper Section 3.1.2, Passive, Level : Node
- Previous Epoch election's bandwidth proofs are taken to find their mean (average) value, from which the minimum requirement is passed/determined by the current producers. Voting can begin once, the minimum bandwidth value is calculated.
1. How it can be achieved
2. Existing Implementation reference
3. Feasibility
4. Technical/Nontechnical Challenges
5. Alternatives
### Participation by Voting (Script)
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
### Elimination \& Result (Chain)
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives


# Mempool Active Validation
## Un-Confirmed Tx Propagation
### Propagation to Leader's Global Mempool
## Global Mempool Tx Validation
### Client-Witness Validation
### Vanity Validation
### Segregation of Un-confirmed Tokens
### Allocation per Packet Leader
### Direct Messaging
## Local Mempool Tx Validation
### Transaction Nature Identification
#### Stable Tax Transaction
#### Normal Transaction
#### Stable Token Transaction
#### Dust Purging Transaction
#### Layered Tax Transaction
#### Node Weight Update Transaction
### Vanity \& Client-Witness Validation
### Fee Validation
### Tax Validation
### Finite Script Validation


# Snips Construction
## Common-Active Functions
### Clock Hash-Concate
### Fee, Oracle Rate Assignment \& Update
## Collateral Snip
### Segregation of Stake UTXOs
### Construction of Collateral Tx
### Noting Authorized Tokens
## Transaction Snip
### Validated-authorized Tx from Local Mempool
### Tax Assignment
### Tax Tx Construction
## Coinbase Snip
### Accepted Token Tx Construction
### Non-Accepted Token Tx Construction
## Propagation
### Direct-Messaging

# Snips Validation
## Common Validation
### Genesis Clock Spaces
### Fee, Oracle Rate Assignment \& Update
### Snip Pool Graphing
## Collateral Snip
### Position Update
### Authorized Token Assignment
## Transaction Snip
### Authorized Token Validation
### Tx Validation
## Coinbase Snip
### Fee Validation
### Hash-reward Validation
### CT Deal Validation
### Position Update
### Commission Validation
## Kamikaze Snip
### Validation
### Attesting
## Propagation
### Direct-Messaging
### Random Rumoring


# Pruning UTXOs
## Expiration \& Fingerprint Replacement
## Centralized Storage Boilerplate

# Scripts \& Proofs
## Stake UTXO
### Conditions & Postion Updates
### Creation
#### Accepted Tokens
#### Non-Accepted Tokens
### Slot Period Attesting
## Oracle Data UTXO
## Oracle Fund UTXO
## Oracle Reputation UTXO
## Bandwidth Proofs
### Bandwidth Proof
### Node Weight Proof

### Arrival Nodes Bandwidth Proof

### Attesting Bandwidth Proof (Script)

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*
- Whitepaper Section - Nil, Passive Program, Level : Script
- After creation of bandwidth proof, it should be attested as a transaction in the ledger, where validators can verify and take it for epoch election
- Similar to wallet clients constructing a transaction, the node itself constrcuts the proof, attest it to a transaction and propagates it
- Since it is a common process found in various applications, it can be developed efficiently. The bandwidth proof creation shall be instructed in the new papers.

<!---*[@username-here](https://www.github.com/[username-here]) "Role-here" Comment* 
--->
## IHR Proofs
## Kamikaze Proof
## Age Proof
### Pattern Identification


# Oracles



- [Blinkchain - Proof of Concept for Development](#blinkchain---proof-of-concept-for-development)
  - [Development Map](#development-map)
- [Objectives](#objectives)
- [Time Architecture](#time-architecture)
- [Epoch Election](#epoch-election)
  - [Producer Arrival](#producer-arrival)
    - [Vote of Confidence Result](#vote-of-confidence-result)
    - [Selection of Bandwidth Proof](#selection-of-bandwidth-proof)
    - [Node Weight Calculation](#node-weight-calculation)
    - [Allocation of Leaders](#allocation-of-leaders)
  - [Block Size \& Time](#block-size--time)
    - [Block Size per sec Fixing (Chain)](#block-size-per-sec-fixing-chain)
    - [Block Time Fixing (Chain)](#block-time-fixing-chain)
    - [Per Block Size Fixing (Node)](#per-block-size-fixing-node)
  - [Vote of Confidence](#vote-of-confidence)
    - [Selection of Un-fit Nodes (Node)](#selection-of-un-fit-nodes-node)
    - [Participation by Voting (Script)](#participation-by-voting-script)
    - [Elimination \& Result (Chain)](#elimination--result-chain)
- [Mempool Active Validation](#mempool-active-validation)
  - [Un-Confirmed Tx Propagation](#un-confirmed-tx-propagation)
    - [Propagation to Leader's Global Mempool](#propagation-to-leaders-global-mempool)
  - [Global Mempool Tx Validation](#global-mempool-tx-validation)
    - [Client-Witness Validation](#client-witness-validation)
    - [Vanity Validation](#vanity-validation)
    - [Segregation of Un-confirmed Tokens](#segregation-of-un-confirmed-tokens)
    - [Allocation per Packet Leader](#allocation-per-packet-leader)
    - [Direct Messaging](#direct-messaging)
  - [Local Mempool Tx Validation](#local-mempool-tx-validation)
    - [Transaction Nature Identification](#transaction-nature-identification)
      - [Stable Tax Transaction](#stable-tax-transaction)
      - [Normal Transaction](#normal-transaction)
      - [Stable Token Transaction](#stable-token-transaction)
      - [Dust Purging Transaction](#dust-purging-transaction)
      - [Layered Tax Transaction](#layered-tax-transaction)
      - [Node Weight Update Transaction](#node-weight-update-transaction)
    - [Vanity \& Client-Witness Validation](#vanity--client-witness-validation)
    - [Fee Validation](#fee-validation)
    - [Tax Validation](#tax-validation)
    - [Finite Script Validation](#finite-script-validation)
- [Snips Construction](#snips-construction)
  - [Common-Active Functions](#common-active-functions)
    - [Clock Hash-Concate](#clock-hash-concate)
    - [Fee, Oracle Rate Assignment \& Update](#fee-oracle-rate-assignment--update)
  - [Collateral Snip](#collateral-snip)
    - [Segregation of Stake UTXOs](#segregation-of-stake-utxos)
    - [Construction of Collateral Tx](#construction-of-collateral-tx)
    - [Noting Authorized Tokens](#noting-authorized-tokens)
  - [Transaction Snip](#transaction-snip)
    - [Validated-authorized Tx from Local Mempool](#validated-authorized-tx-from-local-mempool)
    - [Tax Assignment](#tax-assignment)
    - [Tax Tx Construction](#tax-tx-construction)
  - [Coinbase Snip](#coinbase-snip)
    - [Accepted Token Tx Construction](#accepted-token-tx-construction)
    - [Non-Accepted Token Tx Construction](#non-accepted-token-tx-construction)
  - [Propagation](#propagation)
    - [Direct-Messaging](#direct-messaging-1)
- [Snips Validation](#snips-validation)
  - [Common Validation](#common-validation)
    - [Genesis Clock Spaces](#genesis-clock-spaces)
    - [Fee, Oracle Rate Assignment \& Update](#fee-oracle-rate-assignment--update-1)
    - [Snip Pool Graphing](#snip-pool-graphing)
  - [Collateral Snip](#collateral-snip-1)
    - [Position Update](#position-update)
    - [Authorized Token Assignment](#authorized-token-assignment)
  - [Transaction Snip](#transaction-snip-1)
    - [Authorized Token Validation](#authorized-token-validation)
    - [Tx Validation](#tx-validation)
  - [Coinbase Snip](#coinbase-snip-1)
    - [Fee Validation](#fee-validation-1)
    - [Hash-reward Validation](#hash-reward-validation)
    - [CT Deal Validation](#ct-deal-validation)
    - [Position Update](#position-update-1)
    - [Commission Validation](#commission-validation)
  - [Kamikaze Snip](#kamikaze-snip)
    - [Validation](#validation)
    - [Attesting](#attesting)
  - [Propagation](#propagation-1)
    - [Direct-Messaging](#direct-messaging-2)
    - [Random Rumoring](#random-rumoring)
- [Pruning UTXOs](#pruning-utxos)
  - [Expiration \& Fingerprint Replacement](#expiration--fingerprint-replacement)
  - [Centralized Storage Boilerplate](#centralized-storage-boilerplate)
- [Scripts \& Proofs](#scripts--proofs)
  - [Stake UTXO](#stake-utxo)
    - [Conditions \& Postion Updates](#conditions--postion-updates)
    - [Creation](#creation)
      - [Accepted Tokens](#accepted-tokens)
      - [Non-Accepted Tokens](#non-accepted-tokens)
    - [Slot Period Attesting](#slot-period-attesting)
  - [Oracle Data UTXO](#oracle-data-utxo)
  - [Oracle Fund UTXO](#oracle-fund-utxo)
  - [Oracle Reputation UTXO](#oracle-reputation-utxo)
  - [Bandwidth Proofs](#bandwidth-proofs)
    - [Bandwidth Proof](#bandwidth-proof)
    - [Node Weight Proof](#node-weight-proof)
    - [Arrival Nodes Bandwidth Proof](#arrival-nodes-bandwidth-proof)
    - [Attesting Bandwidth Proof (Script)](#attesting-bandwidth-proof-script)
  - [IHR Proofs](#ihr-proofs)
  - [Kamikaze Proof](#kamikaze-proof)
  - [Age Proof](#age-proof)
    - [Pattern Identification](#pattern-identification)
- [Oracles](#oracles)
