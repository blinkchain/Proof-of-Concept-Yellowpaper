
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

## Terminology 
- n Epoch is the epoch conducting election for n+1 Epoch
- n+1 Epoch is the new production epoch
- n-1 Epoch is previous epoch to n epoch
- n-2 epoch is previous epoch to n-1 epoch

## Timeline 

1. nth block of the epoch, when its confirmed (immutable), the epoch election commences
2. nth Block is typically taken from 9,000 - 10,0000 it should be kept very close to next epoch's production due to influence attacks possible when time is given. Hence a confirmed block very near to next epoch at the same time allows computation.
3. Vote of Confidence results are published along with requirement (criteria)
4. Bandwidth Proofs are selected after validation from VoC criteria
   1. Block Size and Time is fixed
   2. Next Vote of Confidence voting begins
   3. Escrow rate for next epoch published
   4. Next epoch's minimum collateral per token per block/packet is published
5. Node Weights are calculated from Bandwidth proofs for the public key 
6. Total Packets allocation is found per public key
7. Leaders are allocated


> **Quick Info** 💡
> 
> **Producer Arrival** represents new nodes arrival to contest in the Epoch election to produce new blocks. For Arrival a specific slot is allocated range of x to y block heights in every epoch for the next epoch production. Existing and new producers carry out same process, but new nodes tend to have a criteria to be fulfilled, whereas the existing nodes does infact proved passing the criteria for the previous epoch.


## Selection of Bandwidth Proof

> **Quick Info** 💡
> 
> **Bandwidth Proofs** - to be proposed in a new paper "Blink Proofs" will provide a zero knowledge proof to the verfiers (nodes) to calculate its authenticity and ability to contest in the new election to directly influencing to change the block size of the epoch.

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*

- Whitepaper Section - Nil, Passive Program, Level : Node
- Bandwidth Proofs in the ledger in a range of block height X to Y (Denoting Slot opening and ending for Arrival of New nodes) are selected and validated to carry on further steps. Since proofs are available directly in the ledger, it is easier to select the proofs. Only recent proofs are taken and the requirement should be fulfilled.
- The requirement will be published on after every epoch's Vote of Confidence motion.
- The implementation can be done effectively with optimized language suitable.

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

Generate a Json file with  PublicKey with verified bandwidth proofs

1. *Input_Json is the json file with all the blocks*
2. *Output_json is the json file with only the info of the verified bandwidth proofs*
3. *Validate_proof is a function that validated the proof returns true or false*


```
**Algorithm**

1. 



**Pseudocode**

Function Bandwidth_proof_selection (range_begin,range_end,Input_Json) 
{
  In_Data<----Input_Json // only data within the given range
  Out_Data<---- Output_Json 
  For i = range_end, i>=range_begin , i --
  {
  If In_Data[i] position has bandwidth proof 
      If Out_Data not have PublicKey && Validate_proof(PublicKey)
        Write Out_Data with [UTXO index,Transaction ID,Block height,wallet ID]<----In_Data[i]
      End if
  End if
  }
Return Output_Json
}
```

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*

- For Validation of Proofs, it is found if a proof is valid, passes the criteria
- The Criteria (Mean Value after VoC Result) is published
- The VoC passed bandwidth proofs (Public Keys) can publish an increased proof or should be able to post bandwidth more than the criteria.
- The criteria is mandatory for new nodes arrival (new Public Keys)

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

1. 



**Pseudocode**

```


## Node Weight & Total Packets Calculation

- Whitepaper Section 3.2 - 3.2.1, Active, Level: Node
- Node Weights represent, each node's current honesty weight. For every epoch along with the Bandwidth Proof, nodes are required to attach their updated weight which can be validated if true.
- $NodeWeight(N_w)=\sum N(W_n) + Bandwidth (Bits/sec)$
- All of selected bandwith proof's node's weights are summed up to find total packets each node can produce
- $Total Packets (P) = \frac{10,000 \times N_w}{\sum_{1}^{n} (N_w)_n}$

- If Total packets allocated < 10000, then the remaining packets are allocated to nodes with highest positive weight excluding the bandwidth proof.
- Challenges will be script development for Node weight snapshot and validation of it, which also can be effectively achieved.

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

1. 



**Pseudocode**

```

## Allocation of Leaders

- Whitepaper Section 3.2.2 - 3.2.3, Passive, Level: Node
- Since each node's total packets production rate is achieve for the next epoch, each packet's height has to be determined for which node to produce in a determinable randomized structure. Thus, packet leaders are announced, or determined by each node on the network from only the randomized parameters
- First Packet shall be produced by the highest number of packet allocated node i.e., the strongest & most honest node
- Likewise, for every 400 blocks, the strongest of it shall be announced as slot leader, who does not have first packet slot privilleges, but assigned to propagate transactions properly.
- Allocation works like Bitcoin's Difficulty rate and nonce. For each packet a $K$ Output is given in MD160 (Same as PKH address) produced from a Merkle Root of 100 blocks $(R)$ taken in backwards starting from the epoch election commencing block which will be random.
- $K=MD160(SHA256((SHA256(R))+Packet_n+Slot_n+Epoch_n))$
- Any node's PublicKeyHash $(N_x)$ lesser than $K$, shall be selected as the Packet's Producer. $Leader = K_p > N_x$
- $K$ is rehashed during same leader continuous producer assignment or if no lesser $N_x$ value than $K$ is found.
- After the packets are allocated and confirmed, the delegators can verify and collateralize for their tokens for the specific packets
- Challenges will be node's having a different $R$ value due to forks, but can be achieved if a confirmed block's merkle root is taken. If any one node fails to achive common consensus on packet leaders, its propagation/minting will not be accepted and it will get dishonesty weightage.

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

1. 



**Pseudocode**

```
   
## Block Size \& Time Fixing

- Whitepaper Section (2.3.1, 2.3.4, 2.3.5) , Passive, Level: Node
- Bandwidth Proofs taken for next epoch shall provide the network's handling throughput bits per second. From these proofs, the block time and size is calculated and posted for the next epoch block production.
- According to Whitepaper we have to take median bandwidth, but since Vote of confidence kicks un-fit nodes, it can improve requirements for new node arrival which will have increased bandwidth. Hence the median bandwidth is not taken.
- Instead, taking the lowest bandwith shall provide the minimum propagation possibility per second. This process can be done right after the [Selection of Bandwidth Proof](#-selection-of-bandwidth-proof)
- $Epoch_{n+1}(BlockMaxSize_{bits}/sec)=Min(B_1,B_2,....B_n)$
- Per Block time is calculated from Median Time from previous epoch i.e., n-2 epoch for the newly produced epoch
- $Epoch_{n+1} BlockTime (\tau) = Epoch_{n-1} Median BlockTime (\tau)$
- From the per block time, we can calculate per block size as the Block size per second (1) is calculated previously.
- $Epoch_n(BlockSize)=BlockMaxSize_{bits}/sec \times BlinkTime$ 
-  Note : Per second in Blinkchain is 1 Legate = Legacy Hardware Single Thread H/s. Hence, Block Time in seconds denotes legates i.e., 1 sec = 1 legate. 

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

1. 



**Pseudocode**

```

## Vote of Confidence (Requirement)

> **Quick Info** 💡
> 
> **Vote of Confidence** - To scale the blink-network and avoid latencies due to incompetance to propagate transactions faster, the network decides on a vote to kick un-fit nodes as per its requirement. Vote of Confidence involves selection, voting, and elimination by increasing requirements for kicked nodes to participate in the election as a contestant.

- Whitepaper Section 3.1.2, Active, Level: Node
- Vote of confidence can improve requirement therby increasing the difficulty rate to join the producer cluster. Thus way better and bandwidth stronger nodes capable for faster propagation can join.
- To eliminate nodes, the mean value of all selected bandwidth of nodes shall be taken as a threshold to vote on elimination
- Each node shall have a rate of elimination percentage for which it can vote. This is to offload decentralization ethics towards node-operators. Minimum the rate of elimination, maximum the decentralization of the network.
- Each node can vote on un-fit nodes below its rate of elimination of the mean value, till the next selection of bandwidth-proofs. The VoC result will be published before the Selection of Bandwidth Proofs.

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

1. 



**Pseudocode**

```

## Vote of Confidence Result

- Vote of Confidence result will be calulated at first before the selection of bandwidth proofs. From the Voting commences till the epoch election starts, the producers will be able to post their votes.
- 51% of the majority as per each public key/ nodes weight of their proofs are taken in need to get eliminated
- After the elimination of certain bandwidth proofs and their publickeys, the mean value is found and published for requirement for selection of proofs
- The Result is found before the selection of proofs, where its mean value can alter actively, hence the node's bandwidth proofs selections are randomized and should be predicted actively

[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

1. 



**Pseudocode**

```

## Escrow Rate (Requirement)

> **Quick Info** 💡
> 
> **Escrow Rate** - To restrict spending of blinkcoins, blinkchain's native coin during recessions, bear markets similar to a central bank that icreases borrowing rates to reduce spending. Escrow rates are levied to delegators on their blinkcoins every epoch to retrict certain supply for a fixed amount of period i.e., 500 epochs (5,000,000 blocks) or approx 21 days. This rate is known as SERC (Staking Escrow Rates for Collateral) which will be hiked, lowered according to market conditions determined by the network per epoch.

- Whitepaper 3.3.4, Passive, Level:Node
- At Gensis Epochs, the minimum SERC is set at 1%, which is equal to 10 pulses. SERC hikes are expressed in pulses where each pulse denotes 0.1%.
- First 3 (n-2,n-1,n) epochs are known as Genesis epochs, after which the SERC rate is calcualated variably. For 4th Epoch (n+1), the first 2 epoch's (n-2,n-1) closing oracle rate (400th slot oracle rate - for every slot the oracle rate is updated) is taken to find the change or number of pulses
- $Pulse Change_{\epsilon_{(n+1)}}=\frac{\epsilon_{(n-2)O_{cr}}- \epsilon_{(n-1)O_{cr}}}{\epsilon_{(n-1)O_{cr}}} \times 100$
- After the pulse is found, it changes the SERC rate for the new epoch
- $New_{SERC rate} = Current_{SERC rate} + (Pulse Change \times 0.1) \geq 1\%$

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

1. 



**Pseudocode**

```

## Per Token Collateral (Requirement)

- Whitepaper 3.3.3, Passive, Level: Node
- Every token will have different requirement in oracle rate that is to be collaterilzed per block
- Its requirement can be dertermined by simple math, which every node has to find, before commencing the epoch to validate if the token in the specific block is collateralized in full
- For n+1 epoch, its n-1 epoch is taken to find its collateral requirement which is given in oracle rate.
- $Token(k) Stake/packet= MedianVolume(\epsilon_{n-1}) + Escrow Rate \text{ in blinkcoins}$
- Each block in n-1 epoch is taken from which each transaction's outputs are focussed and it's initial oracle value is found
- $O_i = valueLocked \times oracleRate$
- The total oracle value of all transaction's outputs are summed up for a block and every block's median is taken.
- The Median value is the staking requirement, along with it the escrow rate is added. During validation, for a token to be authorized inside a block, it should have more than the minimum requirement, including the escrow utxos.

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

1. 



**Pseudocode**

```

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
#### Bandwidth Proof Tx Validation + Node Weight
#### Vote of Confidence Vote Transaction
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

# Genesis Epochs



- [Blinkchain - Proof of Concept for Development](#blinkchain---proof-of-concept-for-development)
  - [Development Map](#development-map)
- [Objectives](#objectives)
- [Time Architecture](#time-architecture)
- [Epoch Election](#epoch-election)
  - [Terminology](#terminology)
  - [Timeline](#timeline)
  - [Selection of Bandwidth Proof](#selection-of-bandwidth-proof)
  - [Node Weight \& Total Packets Calculation](#node-weight--total-packets-calculation)
  - [Allocation of Leaders](#allocation-of-leaders)
  - [Block Size \& Time Fixing](#block-size--time-fixing)
  - [Vote of Confidence (Requirement)](#vote-of-confidence-requirement)
  - [Vote of Confidence Result](#vote-of-confidence-result)
  - [Escrow Rate (Requirement)](#escrow-rate-requirement)
  - [Per Token Collateral (Requirement)](#per-token-collateral-requirement)
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
      - [Bandwidth Proof Tx Validation + Node Weight](#bandwidth-proof-tx-validation--node-weight)
      - [Vote of Confidence Vote Transaction](#vote-of-confidence-vote-transaction)
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
- [Genesis Epochs](#genesis-epochs)
