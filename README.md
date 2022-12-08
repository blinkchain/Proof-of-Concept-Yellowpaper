
# Blinkchain - Proof of Concept for Development

Contributors: [@jobyreuben](https://www.github.com/jobyreuben), 

## Development Map

Download PDF version [here](https://blinkchain.org/map.pdf)

![Development Map](https://github.com/blinkchain/Proof-of-Concept-for-Development/blob/main/map.png?raw=true)


- [Blinkchain - Proof of Concept for Development](#blinkchain---proof-of-concept-for-development)
  - [Development Map](#development-map)
- [Objectives](#objectives)
- [Time Architecture](#time-architecture)
- [1.Epoch Election](#1epoch-election)
  - [1.1 Bandwidth Proof](#11-bandwidth-proof)
    - [1.1.1 Selection of Bandwidth Proof (Node)](#111-selection-of-bandwidth-proof-node)
  - [1.2 Vote of Confidence (Removal of Nodes)](#12-vote-of-confidence-removal-of-nodes)
    - [Selection of Un-fit Nodes (Node)](#selection-of-un-fit-nodes-node)
    - [Participation by Voting (Script)](#participation-by-voting-script)
    - [Elimination \& Result (Chain)](#elimination--result-chain)
  - [Producer Arrival](#producer-arrival)
    - [Repeated (Bandwidth Proof)](#repeated-bandwidth-proof)
    - [Selection of Proofs (Node)](#selection-of-proofs-node)
    - [Contestant Results (Chain)](#contestant-results-chain)
  - [Allocation of Leaders](#allocation-of-leaders)
  - [Stake UTXO Creation](#stake-utxo-creation)
  - [Block Size \& Time](#block-size--time)
    - [Proof Selection (Node)](#proof-selection-node)
    - [Block Size per sec Fixing (Chain)](#block-size-per-sec-fixing-chain)
    - [Block Time Fixing (Chain)](#block-time-fixing-chain)
    - [Per Block Size Fixing (Node)](#per-block-size-fixing-node)
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
  - [Oracle Data UTXO](#oracle-data-utxo)
  - [Oracle Fund UTXO](#oracle-fund-utxo)
  - [Oracle Reputation UTXO](#oracle-reputation-utxo)
  - [Bandwidth Proofs](#bandwidth-proofs)
    - [Attesting Bandwidth Proof (Script)](#attesting-bandwidth-proof-script)
  - [IHR Proofs](#ihr-proofs)
  - [Kamikaze Proof](#kamikaze-proof)
    - [Pattern Identification](#pattern-identification)
- [Oracles](#oracles)

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

# 1.Epoch Election
Epoch Election conducted every 10,000 blocks (1 Epoch) to announce packet leaders who are eligible to produce their blocks. Block producers are announced before the epoch starts. The following steps starts from attesting proofs to participate in the election to getting selected to mint a block to commencing the epoch.
## 1.1 Bandwidth Proof
Bandwidth Proofs - to be proposed in a new paper "Blink Proofs" will provide a zero knowledge proof to the verfiers (nodes) to calculate its authenticity and ability to contest in the new election to directly influencing to change the block size of the epoch.

### 1.1.1 Selection of Bandwidth Proof (Node)

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*

1. Whitepaper Section - Nil, Passive Program, Level : Node
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
## 1.2 Vote of Confidence (Removal of Nodes)
Short Intro
### Selection of Un-fit Nodes (Node)
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
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
## Producer Arrival
Short Intro
### Repeated (Bandwidth Proof)
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
### Selection of Proofs (Node)
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
### Contestant Results (Chain)
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
## Allocation of Leaders
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
## Stake UTXO Creation
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
## Block Size \& Time
Short Intro
### Proof Selection (Node)
1. Whitepaper Section + Passive/Active + Level
2. Short Info
3. How it can be achieved
4. Existing Implementation reference
5. Feasibility
6. Technical/Nontechnical Challenges
7. Alternatives
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
## Oracle Data UTXO
## Oracle Fund UTXO
## Oracle Reputation UTXO
## Bandwidth Proofs
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
### Pattern Identification


# Oracles


