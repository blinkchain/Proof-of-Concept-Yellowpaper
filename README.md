# Blinkchain - Proof of Concept (Yellow-paper)
**Contributors**: [@jobyreuben](https://www.github.com/jobyreuben),  [@I-Corinthian](https://github.com/I-Corinthian)

Welcoming contributions from the Core-Team Project Blink

## Resources

- Find Blinkchain *Whitepaper* [here](https://blinkchain.org/blinkchain.pdf) and *Litepaper* [here](https://blinkchain.org/litepaper.pdf)
- For visual learning find YouTube *Short Explanation Series* [here](https://www.youtube.com/playlist?list=PL5herZ3QXwNPYSsYDXTTTMao0LtyJj-2R)
- [Not Updated] Download *Development Map* PDF [here](https://blinkchain.org/map.pdf)


# Objectives
1. Whitepaper Section, Passive or Active Program \& Level
   - Active: Constantly running, looking for triggers
   - Passive: Active per time-frame or run upon triggered
   - Chain - Ledger, Consensus and Core Implementations
   - Script - UTXO scripts and zk-proofs construction
   - OffChain - Client Side construction and propagation
   - Node - Parameter construction and memory access for Validation
2. Process, Algorithm and Mathmatical Data
3. Existing Implementations and Documentation References
4. Feasibility of Development through Pseudocodes
5. Technical & Non-Technical Challenges
6.  Alternatives Offered & Outcomes if any


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
   2. Escrow rate for next epoch published
   3. Next epoch's minimum collateral per token per block/packet is published
   4. Gas unit fee in oracle rate and transfer fee percentage
5. Node Weights are calculated from Bandwidth proofs for the public key 
6. Total Packets allocation is found per public key
7. Leaders are allocated
8. Next Vote of Confidence voting begins


**💡 Producer Arrival** represents new nodes arrival to contest in the Epoch election to produce new blocks. For Arrival a specific slot is allocated range of x to y block heights in every epoch for the next epoch production. Existing and new producers carry out same process, but new nodes tend to have a criteria to be fulfilled, whereas the existing nodes does infact proved passing the criteria for the previous epoch.


## Selection of Bandwidth Proof

**💡 Bandwidth Proofs** - to be proposed in a new paper "Blink Proofs" will provide a zero knowledge proof to the verfiers (nodes) to calculate its authenticity and ability to contest in the new election to directly influencing to change the block size of the epoch.

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*

- Whitepaper Section - Nil, Passive Program, Level : Node
- Bandwidth Proofs in the ledger in a range of block height X to Y (Denoting Slot opening and ending for Arrival of New nodes) are selected and validated to carry on further steps. Since proofs are available directly in the ledger, it is easier to select the proofs. Only recent proofs are taken and the requirement should be fulfilled.
- The requirement will be published on after every epoch's Vote of Confidence motion.
- The implementation can be done effectively with optimized language suitable.

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*
<!--- [TBF]
```
**Algorithm**

--- Algorithm here---
```
--->

```
**Pseudocode**

//This program will be used to generate a Json file with 
//All PublicKey with verified bandwidth proofs 

//RawData_Json is the json file with all the blocks
//Bandwidthproofs_json is the json file with only the info of the verified bandwidth proofs
//Validate_proof is a function that validated the proof returns true or false


Function Bandwidth_proof_selection (range_begin,range_end) {

Raw_Data<----RawaData_json only data within the given range
Bandwidthproof_Data<---- Bandwidthproofs_Json 
loop i = range_end, i>=range_begin , i - -

{
 If Raw_Data[i] position has bandwidth proof 
    If Bandwidthproof_Data not have PublicKey && Validate_proof(PublicKey)
      Write Bandwidthproof_Data with [UTXO index,Transaction ID,Block hight,PublicKey]<----Raw_Data[i]
   End if
End if
}
}
```

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*

- For Validation of Proofs, it is found if a proof is valid, passes the criteria
- The Criteria (Mean Value after VoC Result) is published
- The VoC passed bandwidth proofs (Public Keys) can publish an increased proof or should be able to post bandwidth more than the criteria.
- The criteria is mandatory for new nodes arrival (new Public Keys)

<!--- [TBF]
*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

--- Algorithm here---
```

```

**Pseudocode**

--- Psuedocode here---

```
--->

## Node Weight & Total Packets Calculation

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*


- Whitepaper Section 3.2 - 3.2.1, Active, Level: Node
- Node Weights represent, each node's current honesty weight. For every epoch along with the Bandwidth Proof, nodes are required to attach their updated weight which can be validated if true.
   > $NodeWeight(N_w)=\sum N(W_n) + Bandwidth (Bits/sec)$
- All of selected bandwith proof's node's weights are summed up to find total packets each node can produce
   > $Total Packets (P) = \frac{10,000 \times N_w}{\sum_{1}^{n} (N_w)_n}$

- If Total packets allocated < 10000, then the remaining packets are allocated to nodes with highest positive weight excluding the bandwidth proof.
- Challenges will be script development for Node weight snapshot and validation of it, which also can be effectively achieved.

*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

<!--- [TBF]
```
**Algorithm**

--- Algorithm here---
```
--->

```
**Pseudocode**

//This program will be used to generate a Json file with 
//packets allocated to the public_key according to their node weights

Bandwidthproof_Json - Json file of all the Bandwidth Proof  and public key
Allocation_Json - Json file with packets allocated to each public key


Function nodeweight(bandwidth,total_weight){
node_weight = bandwidth + total_weight 
return node_weight
}

Function nodeweight_sum(){
Data<----Bandwidthproof_Json

Loop every element in Data
{
Allocation_Json[public_key]<---- Data[publickey]

Allocation_Json[weight of each publickey] <---- nodeweight(Data[bandwidth],Data[total_weight])

node_sum += nodeweight(Data[bandwidth],Data[total_weight])
}

Return node_sum
}

Function Allocate_packets(){

sum = nodeweight_sum()
Nodes<---- Allocation_Json

Loop each element in Nodes
{
(int)packets_allocated = (10000 * Nodes[nodeweight]) / sum
Write Nodes[packets] <--- packets_allocated 
sum_packets += packets_allocated
}

Shot Nodes in descending order of weights 

h = 10000 - sum_packets
Loop sum_packets != 10000
{
  Loop from 1 to h for every element on Nodes
{
    Write Node[packets] + 1
    
 }
}
}

```

## Allocation of Leaders

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*


- Whitepaper Section 3.2.2 - 3.2.3, Passive, Level: Node
- Since each node's total packets production rate is achieve for the next epoch, each packet's height has to be determined for which node to produce in a determinable randomized structure. Thus, packet leaders are announced, or determined by each node on the network from only the randomized parameters
- First Packet shall be produced by the highest number of packet allocated node i.e., the strongest & most honest node
- Likewise, for every 400 blocks, the strongest of it shall be announced as slot leader, who does not have first packet slot privilleges, but assigned to propagate transactions properly.
- Allocation works like Bitcoin's Difficulty rate and nonce. For each packet a $K$ Output is given in MD160 (Same as PKH address) produced from a Merkle Root of 100 blocks $(R)$ taken in backwards starting from the epoch election commencing block which will be random.
   > $K=MD160(SHA256((SHA256(R))+Packet_n+Slot_n+Epoch_n))$
   > $R=MerkleRoot(Block_{h-100} : Block_h)$
- Any node's PublicKeyHash $(N_x)$ lesser than $K$, shall be selected as the Packet's Producer. 
   > $Leader = K_p > N_x$
- $K$ is rehashed during same leader continuous producer assignment or if no lesser $N_x$ value than $K$ is found.
- After the packets are allocated and confirmed, the delegators can verify and collateralize for their tokens for the specific packets
- Challenges will be node's having a different $R$ value due to forks, but can be achieved if a confirmed block's merkle root is taken. If any one node fails to achive common consensus on packet leaders, its propagation/minting will not be accepted and it will get dishonesty weightage.

<!--- [TBF]
*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

--- Algorithm here---
```

```

**Pseudocode**

--- Psuedocode here---

```
--->
   
## Block Size \& Time Fixing

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*


- Whitepaper Section (2.3.1, 2.3.4, 2.3.5) , Passive, Level: Node
- Bandwidth Proofs taken for next epoch shall provide the network's handling throughput bits per second. From these proofs, the block time and size is calculated and posted for the next epoch block production.
- According to Whitepaper we have to take median bandwidth, but since Vote of confidence kicks un-fit nodes, it can improve requirements for new node arrival which will have increased bandwidth. Hence the median bandwidth is not taken.
- Instead, taking the lowest bandwith shall provide the minimum propagation possibility per second. This process can be done right after the [Selection of Bandwidth Proof](#-selection-of-bandwidth-proof)
   > $Epoch_{n+1}(BlockMaxSize_{bits}/sec)=Min(B_1,B_2,....B_n)$
- Per Block time is calculated from Median Time from previous epoch i.e., n-2 epoch for the newly produced epoch
   > $Epoch_{n+1} BlockTime (\tau) = Epoch_{n-1} Median BlockTime (\tau)$
- From the per block time, we can calculate per block size as the Block size per second (1) is calculated previously.
   > $Epoch_n(BlockSize)=BlockMaxSize_{bits}/sec \times BlinkTime$ 
-  Note : Per second in Blinkchain is 1 Legate = Legacy Hardware Single Thread H/s. Hence, Block Time in seconds denotes legates i.e., 1 sec = 1 legate. 

<!--- [TBF]
*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

--- Algorithm here---
```

```

**Pseudocode**

--- Psuedocode here---

```
--->

## Vote of Confidence

**💡 Vote of Confidence** - To scale the blink-network and avoid latencies due to incompetance to propagate transactions faster, the network decides on a vote to kick un-fit nodes as per its requirement. Vote of Confidence involves selection, voting, and elimination by increasing requirements for kicked nodes to participate in the election as a contestant.

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*


- Whitepaper Section 3.1.2, Active, Level: Node
- Vote of confidence (VoC) can improve requirement therby increasing the difficulty rate to join the producer cluster. Thus way better and bandwidth stronger nodes capable for faster propagation can join.
- To eliminate nodes, the mean value of all selected bandwidth of nodes shall be taken as a threshold to vote on elimination
- Each node shall have a rate of elimination percentage for which it can vote. This is to offload decentralization ethics towards node-operators. Minimum the rate of elimination, maximum the decentralization of the network.
- Each node can vote on un-fit nodes below its rate of elimination of the mean value, till the next selection of bandwidth-proofs. The VoC result will be published before the Selection of Bandwidth Proofs.

<!--- [TBF]
*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

--- Algorithm here---
```

```

**Pseudocode**

--- Psuedocode here---

```
--->

## VoC Result

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*


- Vote of Confidence result will be calulated at first before the selection of bandwidth proofs. From the Voting commences till the epoch election starts, the producers will be able to post their votes.
- 51% of the majority as per each public key/ nodes weight of their proofs are taken in need to get eliminated
- After the elimination of certain bandwidth proofs and their publickeys, the mean value is found and published for requirement for selection of proofs
- The Result is found before the selection of proofs, where its mean value can alter actively, hence the node's bandwidth proofs selections are randomized and should be predicted actively

<!--- [TBF]
*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

--- Algorithm here---
```

```

**Pseudocode**

--- Psuedocode here---

```
--->

## Escrow Rate

**💡 Escrow Rate** - To restrict spending of blinkcoins, blinkchain's native coin during recessions, bear markets similar to a central bank that icreases borrowing rates to reduce spending. Escrow rates are levied to delegators on their blinkcoins every epoch to retrict certain supply for a fixed amount of period i.e., 500 epochs (5,000,000 blocks) or approx 21 days. This rate is known as SERC (Staking Escrow Rates for Collateral) which will be hiked, lowered according to market conditions determined by the network per epoch.

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*


- Whitepaper 3.3.4, Passive, Level:Node
- At Gensis Epochs, the minimum SERC is set at 1%, which is equal to 10 pulses. SERC hikes are expressed in pulses where each pulse denotes 0.1%.
- First 3 (n-2,n-1,n) epochs are known as Genesis epochs, after which the SERC rate is calcualated variably. For 4th Epoch (n+1), the first 2 epoch's (n-2,n-1) closing oracle rate (400th slot oracle rate - for every slot the oracle rate is updated) is taken to find the change or number of pulses
   > $Pulse Change_{\epsilon_{(n+1)}}=\frac{\epsilon_{(n-2)O_{cr}}- \epsilon_{(n-1)O_{cr}}}{\epsilon_{(n-1)O_{cr}}} \times 100$
- After the pulse is found, it changes the SERC rate for the new epoch
   > $New_{SERC rate} = Current_{SERC rate} + (Pulse Change \times 0.1) \geq 1\%$

<!--- [TBF]
*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

--- Algorithm here---
```

```

**Pseudocode**

--- Psuedocode here---

```
--->

## Token Collateral Requirement

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*

- Whitepaper 3.3.3, Passive, Level: Node
- Every token will have different requirement in oracle rate that is to be collaterilzed per block
- Its requirement can be dertermined by simple math, which every node has to find, before commencing the epoch to validate if the token in the specific block is collateralized in full
- For n+1 epoch, its n-1 epoch is taken to find its collateral requirement which is given in oracle rate.
   > $Token(k) Stake/packet= MedianVolume(\epsilon_{n-1}) + Escrow Rate \text{ in blinkcoins}$
- Each block in n-1 epoch is taken from which each transaction's outputs are focussed and it's initial oracle value is found
  > $O_i = valueLocked \times oracleRate$
- The total oracle value of all transaction's outputs are summed up for a block and every block's median is taken.
- The Median value is the staking requirement, along with it the escrow rate is added. During validation, for a token to be authorized inside a block, it should have more than the minimum requirement, including the escrow utxos.

<!--- [TBF]
*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

--- Algorithm here---
```

```

**Pseudocode**

--- Psuedocode here---

```
--->


## Transaction Fees

**💡 Blink Fees** - In Blinkchain transaction Fees consists of a base gas fee and a transfer fee. Transfer fee is only applied when the UTXO has value. Gas fee is applied on all the UTXO scripts as every script should execute to validate the transaction. Gas Fees are given in per unit fee in oracle rate due to non-native transaction fee model and it is fixed per 8500 epochs (1 year) and can be increased by conducting a vote from producers. Transfer fee is variable percentage fee levied on individual UTXOs, changes per epoch based on volume of outputs - Higher the volume lower the fee and vice versa.

*[@jobyreuben](https://www.github.com/jobyreuben) Author Comment*

- In the first year the gas fee per unit is set at $0.0001. Transfer fee ranges from min 0.005% and max at 0.05%
- For every 8500 epoch all the votes are submitted along with bandwidth proof and its average value is taken as the increase in gas fee per unit.
- During genesis epochs (n-2),(n-1),(n), the transfer fee is set at 0.05%, from n+1 epoch the transfer fee is calculated
- Transfer Fee is based on total volume per epoch in oracle rate, where each epoch's utxo output's initial oracle value is summed. Such that n-2 & n-1 epoch's total volume in oracle rate taken to decide to change transfer fee for the n+1 epoch.
- Standard Deviation of n-2, n-1 epoch's total volume in oracle rate taken, if SD < 0.75 transfer fee doesn't change, if SD > 0.75 then it is decided to calculate change percentage.
- Each change only either +0.0005 or -0.0005, if If Volume of n-2 < Volume of n-1 then there is higher volume of transactions requiring to lower the transfer fee. If vice versa ,it should increase the transfer fee

<!--- [TBF]
*[@I-Corinthian](https://github.com/I-Corinthian) Pseudocode Contribution*

```
**Algorithm**

--- Algorithm here---
```

```

**Pseudocode**

--- Psuedocode here---

```
--->

📍 **Conclusion**
 
Thus, the packet leaders i.e., block producers are assigned randomly according to their weights and bandwidth. Block size and time for the upcoming epoch is published i.e., determined by the network itself on parameters. For the next election, from the VoC votes, requirement (difficulty rate) to join the network will increase thereby increasing the scalability and faster propagation proportionally with stronger nodes for high-throughput blinkchain. The collateral requirement with Staking interest rates are published for further validation with increasing value of each blinkcoin of holders, delegators and investors. Each epoch's Transfer fee and Gas fee per unit is assigned and for every 8500 epochs the gas fee is put on to vote to increase as per CPI.


# Slot Update
## Oracle Rate of Tokens


# Active Mempool Tx Validation
## Client-Witness & Vanity Validation
<!----- [Draft]
- Check if witness signature is attested for every transaction. Witness would be a public key signing the signed transaction of the owners
- Vanity would be checking list of vanity address approved by the node - in consensus
- --->
## Fee & Tax Validation

**💡 Quick Info** 
 
**Blink Taxes** - In Blinkchain taxes are imposed in a transaction level to assist governments to regulate decentralized currency payments. There are three types of taxes as per now, 1. Gains 2. Stable 3. Layered. Gains tax is taken during appreciation of asset upon spending. Here taxes are only taken during spending of UTXOs. Stable Taxes are taken as a seperate transaction for holding stable fiat-backed tokens for a period of time. Layered Taxes are Sales taxes during a merchant purchase which directly pays the government, the Sales tax and ease audits onchain immutably.  

- In a client created transaction of inputs and outputs, the outputs will have a difference without adding fees and taxes in utxos
   > $Input-Output = Difference$
   > $Diff=\sum_0^n UTXO(Gas Fee + Transfer Fee+ Gains Tax+Layered Tax)$
   > $GasFee=Epoch_n(UnitFee) \times UTXO_n(\sum OpcodeUnits)$
   > $Transfer Fee=Epoch_n(X)\% \times UTXO_n(TotalValue)$
   > $GainsTax=$
   > $LayeredTax=Input - (Output + \sum (GasFee + Transfer Fee + Gains Tax))$

## Finite Script Validation
<!----- [Draft]
- Running the script pseudonymously and finding if it ends in either true or false. If infinite it is rejected
- --->
## Stable Tax Transaction
## Dust Purging Transaction
<!----- [Draft]
- Transaction that contains 2 or more inputs and only one output with the difference of gains taxes only shall be found as dust purging transactions.
- It is fee-free
- --->
## Bandwidth Proof Validation
## VoC Vote Transaction
## Gas Base fee increase vote


# Common Snips Construction
## Clock Hash-Concate
<!----- [Draft]
- Running a single thread-hash clock that runs a sha256 hash function repetedly constraint in single thread
- Any external data or un-confirmed transaction's serialized data will be hashed and concated and again hashed with the pre-images
- Each hash will be a snip, at minimum
- First snip's header will be by a random VRF function's Hash. from next snips it will be rehashed to graph the following snips.
--->

# Collateral Snip Construction
## Segregation of Stake UTXOs
<!----- [Draft]
- Find stake utxos staked for the public key of the node
- Add total value of all UTXOs based on token id
- If Requirement in oracle value < Token ID sum in oracle value
- then take select stake utxos that will be near to requirement
- It notes authorized tokens
- --->
## Construction of Collateral Tx
<!----- [Draft]
- Position Update of Stake UTXOs
--->
# Transaction Snip Construction
## Validated Tx with Tax
<!----- [Draft]
- Txs from local mempool, that are pre-validated are taken and attested to the snip
- Txs are prevalidated and each transaction snip is pre created, with only header hash to add for graphing
- In each snip, it should end with the tax outputs leaving only fee differnece for each snip.
- If a snip doesn't contain taxes on it, then it will be rejected
- Differences will include Gains Tax, Fee, Layered Tax
- Fees will be determined and it is neglected
- Gains tax can be determined and it is created for the authorized government's wallet identified from its vanity address
- Layered Taxes are done for merchants point of sale taxes which is alos determined by the rest of difference other than the gains and fees. It is send to the payee's government wallet.
- With validated txs, the last of txs in a snip will have outputs of gains and layered taxes
--->
# Coinbase Snip Construction
## Accepted Token Tx Construction
<!----- [Draft]
- All fee outputs are segregated by accepted and non-accepted tokens
- --->
## Non-Accepted Token Tx Construction


# Snips Validation
## Snips Graphing & Spacing
<!----- [Draft]
- Actively each snips are graphed and looks for next snip to be graphed, if its delayed by x hashes, the snip is rejected and the kamikaze snip should be added
- --->
## Colalteral Snip
<!----- [Draft]
- after each poistion update is executed, nodes should identify the collaterilzed tokens. If any other tokens are added to further snips, it is rejected immediately
- --->
## Tx & Tax Validation
<!----- [Draft]
- Find if Difference is only leaved in fees, and the tax outputs are given in last transactions - gains and layered taxes
- --->
## Coinbase Verification
<!----- [Draft]
- Fee outputs are calculated, poistion update, etc
- --->
## Kamikaze Proof


# Pruning UTXOs
## Expiration \& Fingerprint Replacement
<!----- [Draft]
- Recent 2 epochs should not be pruned
- Before epochs spent utxos, expired utxos are searched
- UTXOs are replaced by a single fingerprint  of the hex's hash
- If every UTXO is pruned by fingerprint in a transaction, then whole transaction is replaced by a fingerprint hash
- When 16 transactions are pruned completely it is replaced by a node hash
- Thus whole of epochs can be pruned.
- --->
## Centralized Storage Boilerplate
<!----- [Draft]
- Download from a Node whole history
- Create additional fingerprints for every utxo, every transaction, to construct proofs
- Have third party verifiers for verification of proof for fact checking
- --->
## Recovery Proof
<!----- [Draft]
- Proof construction from centralized storage with pre-image script to execute and verify and create a new utxo with updated renting time
- --->

# Scripts \& Proofs
## Stake UTXO
<!----- [Draft]
- Have positions and each position have conditions locked and unlocked
- Each stake utxo will only accept blinkcoins from public
- Each stake utxo will have a moderator i.e., the node's public key
- Each stake utxo will have the asset id for which its staking for
- Moderator can add blinkcoins in Stake UTXO. and deposit asset-id only in Collateral UTXO only if the stake utxo is not created by the moderator i.e., non-accepted token
- Deposit condition available in Stake for public, In collateral UTXO for moderator
- During deposit, the contract gives us LP tokens of the specific stake utxo
- Withdraw can be poosible in all positions - Stake, Payback immediately but in collateral utxo if its created only after certain blocks from which it is created
- Position update can be only done by moderator confined by snips rules on node level only to accepted position update at ceratin time-frame. Moderator's public key hash is given when creating the stake UTXO
- If moderator and creator == same, then it is an accepetd token, but if its not same it is a un-accepted token.
- For un-accpeted token, the moderator can create lp tokens without deposit, but with constraints on how much he can create.
--->
## Oracle Data UTXO
## Oracle Fund UTXO
## Oracle Reputation UTXO
## Bandwidth Proofs + Updated Node Weight
## IHR Proofs
## Kamikaze Proof


# Opcode Gas Units




# Leader Responsibilities

## Epoch Leader
<!----- [Draft]
- Client-Witness & Vanity Validation
- Segregation of Un-confirmed Tokens
- Allocation per Packet Leader
- Direct Messaging
- --->
## Slot Leader
<!----- [Draft]
- Unconfirmed new transaction to Epoch Leader
- --->
## Packet Leader
<!----- [Draft]
- Unconfirmed new transaction to Epoch Leader
- --->

# Messaging Protocol
## Priority Peer List
## Direct-Messaging
## Distributed Rumouring 