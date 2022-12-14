# Blinkchain - Proof of Concept (Yellow-paper)
**Contributors**: [Joby Reuben](mailto:joby@blinkchain.org),  [Ajay Joshua](https://github.com/I-Corinthian)

Welcoming contributions from the Core-Team Project Blink

## Resources

- Find Blinkchain *Whitepaper* [here](https://blinkchain.org/blinkchain.pdf) and *Litepaper* [here](https://blinkchain.org/litepaper.pdf)
- For visual learning find YouTube *Short Explanation Series* [here](https://www.youtube.com/playlist?list=PL5herZ3QXwNPYSsYDXTTTMao0LtyJj-2R)
- [Not Updated] Download *Development Map* PDF [here](https://blinkchain.org/map.pdf)

## Development

1. **Node Level**
   1. Epoch Election
      - Removing Proof of Work
      - Announcing Specific Block Height Producers
   2. Transaction Validation
      - Pre-Validation upon arrival on Mempool
      - Grouping Transactions into Bundles as Pre-snips
   3. Snips Construction
      - Hash-clocks Running & concatenating
      - Active construction of collateral snip
      - Attaching Pre-snips bundles as per snip rules and order
      - Construction of Coinbase snip
   4. Other Active Programs
      - Per Token Oracle Rate
      - Client Reputations
      - Oracle Reputations
      - Node Weights
      - Vanity Additions and Change
   5. Snips Validation
      - Snip Pool graphing, Validation of Tx, Tax, Fees.
      - Snip Consensus rules validation
      - Approval/Rejection of Snips based on Block-time
      - Attesting Kamikaze Proof
   6. Pruning
      - Creation of Local Chain Branches
      - Updating Expired, Spent UTXOs with Fingerprints
      - Merkle Chain construction
   7. Messaging Protocol 
      - Removing current Gossip Protocol
      - Beeline-Gossip Protocol Update 
      - Origin-Destination Path with Onion Proofs for Type of Data e.g., Snip, Un-confirmed Tx, etc
   8.  OP_CODES
       - Mapping Bitcoin Script OP_CODES with Units
       - Creating new opcodes 1. DELEGATE CALL  
2. **Script Level**
   1. UTXO Scripts
      1. Stake UTXO
      2. Oracle UTXOs - Data, Fund, Reputation
      3. Client-Reputation UTXO
      4. Vanity Add/Change UTXO
   2. UTXO Proofs
      1. Bandwidth Proof
         - Includes Bandwidth, Node weight,  Gas vote, VoC vote, Rent Rate 
      2. IHR Proof
      3. Kamikaze Proof
         - Null Packet, Timeout, Denial of Transaction Attack, Queue Cutting Attack, Un-Authorized Token, Bad Tx, etc. 
3. **Client Level**
   1. Wallet Client
      - Updating Balances actively subtracting taxes
      - Construction with Fees, Taxes difference per output
      - Fetch Layered Tax, Target Region and transaction construction
      - Client Witness, Target Block, Fall-back Block Heights, Path construction from Node.
   2. Delegation Client
   3. Oracle Client
   4. Archive Client
      - Centralized Node Storage with built in APIs to read Blinkchain History with verification from producer nodes 



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
*[Joby Reuben](https://www.github.com/jobyreuben)*
- Whitepaper Section [2.1], Level : Node
- The Time Architecture in Blinkchain is segregated into Epoch = 10,000 blocks; Slot = 400 blocks ; Packet = 1 block.
- These time frames are not correlated to the ledger, as it only knows block heights. It is only taken in the following area
- Election conducted every epoch (10,000 blocks)
- Announcing Leaders for every Packets
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


**???? Producer Arrival** represents new nodes arrival to contest in the Epoch election to produce new blocks. For Arrival a specific slot is allocated range of x to y block heights in every epoch for the next epoch production. Existing and new producers carry out same process, but new nodes tend to have a criteria to be fulfilled, whereas the existing nodes does infact proved passing the criteria for the previous epoch.


## Selection of Bandwidth Proof

**???? Bandwidth Proofs** - to be proposed in a new paper "Blink Proofs" will provide a zero knowledge proof to the verfiers (nodes) to calculate its authenticity and ability to contest in the new election to directly influencing to change the block size of the epoch.

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Whitepaper Section - Nil, Passive Program, Level : Node
- Bandwidth Proofs in the ledger in a range of block height X to Y (Denoting Slot opening and ending for Arrival of New nodes) are selected and validated to carry on further steps. Since proofs are available directly in the ledger, it is easier to select the proofs. Only recent proofs are taken and the requirement should be fulfilled.
- The requirement will be published on after every epoch's Vote of Confidence motion.
- The implementation can be done effectively with optimized language suitable.

*[Ajay Joshua](https://github.com/I-Corinthian)*


```

///This program will be used to generate a Json file with 
//All PublicKey with verified bandwidth proofs 

//RawData_Json is the json file with all the blocks
//Bandwidthproofs_json is the json file with only the info of the verified bandwidth proofs
//Validate_proof is a function that validated the proof returns true or false


Function Bandwidth_proof_selection (range_begin,range_end) {

   Raw_Data<----RawaData_json only data within the given range
   Bandwidthproof_Data<---- Bandwidthproofs_Json 
   FOR i = range_end, i>=range_begin , i - -
   IF Raw_Data[i] position has bandwidth proof 
      IF Bandwidthproof_Data not have PublicKey && Validate_proof(PublicKey)
         Write Bandwidthproof_Data with [UTXO index,Transaction ID,Block height,PublicKey]<----Raw_Data[i]
      End IF
   End IF
   End-FOR
}
```

*[Joby Reuben](https://www.github.com/jobyreuben)*

- For Validation of Proofs, it is found if a proof is valid, passes the criteria
- The Criteria (Mean Value after VoC Result) is published
- The VoC passed bandwidth proofs (Public Keys) can publish an increased proof or should be able to post bandwidth more than the criteria.
- The criteria is mandatory for new nodes arrival (new Public Keys)



## Node Weight & Total Packets Calculation

*[Joby Reuben](https://www.github.com/jobyreuben)*


- Whitepaper Section 3.2 - 3.2.1, Active, Level: Node
- Node Weights represent, each node's current honesty weight. For every epoch along with the Bandwidth Proof, nodes are required to attach their updated weight which can be validated if true.
   > $NodeWeight(N_w)=\sum N(W_n) + Bandwidth (Bits/sec)$
- All of selected bandwidth proof's node's weights are summed up to find total packets each node can produce
   > $Total Packets (P) = \frac{10,000 \times N_w}{\sum_{1}^{n} (N_w)_n}$

- If Total packets allocated < 10000, then the remaining packets are allocated to nodes with highest positive weight excluding the bandwidth proof.
- Challenges will be script development for Node weight snapshot and validation of it, which also can be effectively achieved.

*[Ajay Joshua](https://github.com/I-Corinthian)*



```

//This program will be used to generate a Json file with 
//packets allocated to the public_key according to their node weights

Bandwidthproof_Json - Json file of all the Bandwidth Proof  and public key
Allocation_Json - Json filPsuedocodee with packets allocated to each public key


Function nodeweight(bandwidth,total_weight)
{
    node_weight = bandwidth + total_weight 
    return node_weight
}

Function nodeweight_sum(){
    Data<----Bandwidthproof_Json

WHILE every element in Data
    Allocation_Json[public_key]<---- Data[publickey]

    Allocation_Json[weight of each publickey] <---- nodeweight(Data[bandwidth],Data[total_weight])

    node_sum += nodeweight(Data[bandwidth],Data[total_weight])
End-WHILE

    RETURN node_sum
}

Function Allocate_packets(){

    sum = nodeweight_sum()
    Nodes<---- Allocation_Json

WHILE each element in Nodes
    (int)packets_allocated = (10000 * Nodes[nodeweight]) / sum
    Write Nodes[packets] <--- packets_allocated 
    sum_packets += packets_allocated
End-WHILE

    sorted(Nodes[weight],reverse=TRUE ) //Sort Nodes in descending order of weights 

    h = 10000 - sum_packets
    WHILE sum_packets != 10000
          FOR i = 0 to h do
             Write Node[packets] + 1
          End-FOR
    End-WHILE
}
```

## Leaders Announcement

*[Joby Reuben](https://www.github.com/jobyreuben)*


- Whitepaper Section 3.2.2 - 3.2.3, Passive, Level: Node
- Since each node's total packets production rate is achieve for the next epoch, each packet's height has to be determined for which node to produce in a determinable randomized structure. Thus, packet leaders are announced, or determined by each node on the network from only the randomized parameters
- Allocation works like Bitcoin's Difficulty rate and nonce. For each packet a $K$ Output is given in MD160 (Same as PKH address) produced from a Merkle Root of 100 blocks $(R)$ taken in backwards starting from the epoch election commencing block which will be random.
   > $K=MD160(SHA256((SHA256(R))+Packet_n+Slot_n+Epoch_n))$
   > $R=MerkleRoot(Block_{h-100} : Block_h)$
- Any node's PublicKeyHash $(N_x)$ lesser than $K$, shall be selected as the Packet's Producer. 
   > $Leader = K_p > N_x$
- $K$ is rehashed during same leader continuous producer assignment or if no lesser $N_x$ value than $K$ is found.
- After the packets are allocated and confirmed, the delegators can verify and collateralize for their tokens for the specific packets
- Challenges will be node's having a different $R$ value due to forks, but can be achieved if a confirmed block's merkle root is taken. If any one node fails to achieve common consensus on packet leaders, its propagation/minting will not be accepted and it will get dishonesty weightage.


   
## Block Size \& Time Fixing

*[Joby Reuben](https://www.github.com/jobyreuben)*


- Whitepaper Section (2.3.1, 2.3.4, 2.3.5) , Passive, Level: Node
- Bandwidth Proofs taken for next epoch shall provide the network's handling throughput bits per second. From these proofs, the block time and size is calculated and posted for the next epoch block production.
- According to Whitepaper we have to take median bandwidth, but since Vote of confidence kicks un-fit nodes, it can improve requirements for new node arrival which will have increased bandwidth. Hence the median bandwidth is not taken.
- Instead, taking the lowest bandwidth shall provide the minimum propagation possibility per second. This process can be done right after the [Selection of Bandwidth Proof](#-selection-of-bandwidth-proof)
   > $Epoch_{n+1}(BlockMaxSize_{bits}/sec)=Min(B_1,B_2,....B_n)$
- Per Block time is calculated from Median Time from previous epoch i.e., n-2 epoch for the newly produced epoch
   > $Epoch_{n+1} BlockTime (\tau) = Epoch_{n-1} Median BlockTime (\tau)$
- From the per block time, we can calculate per block size as the Block size per second (1) is calculated previously.
   > $Epoch_n(BlockSize)=BlockMaxSize_{bits}/sec \times BlinkTime$ 
-  Note : Per second in Blinkchain is 1 Legate = Legacy Hardware Single Thread H/s. Hence, Block Time in seconds denotes legates i.e., 1 sec = 1 legate. 

*[Ajay Joshua](https://github.com/I-Corinthian)*


```
//This program will be used to generate a Json file with next Epochs Block size
//Find the next Epochs per block size


//For the understaning of the Devlopers the nameing of the Epoch Json file is done by 
     n  - represents the current Epoch 
     n1 - represents the next Epoch to be produced
     pn - represents the previous Epoch
     pn2- represents teh n-2th Epoch

//Blockreq_Json is the json file with all the block req For every epoch
//Bandwidthproofs_json is the json file with only the info of the verified bandwidth proofs
//n epoch_Json is the json file of the current Epoch or the lastly produced Epoch
//pn epoch_Json is the json file of the previous epoch


function Median(epoch)
{
    Epoch<---epoch
    sorted(Epoch[blocktime]) sorting Epoch in assending order of blocktime
    return Epoch[blocktime] in position  position len(Epoch)/2
}


function Set_BlockSize(n,pn)
{
    Data<---Bandwidthproff_Json
    Epoch_n<---read(n+"epoch_Json")
    Epoch_pn<---read(pn+"epoch_Json")
    Blockreq<---Blockreq_Json
    max_size = min(Data[bandwidth]) min of bandwidth
    Epoch_blocktime = Median(Epoch_pn)
    block_size = max_size * Epoch_blocktime
    Write block_size in Blockreq[block_size]
    Write Epoch_blocktime in Blockreq[Epoch_blocktime]
}
```



## Vote of Confidence

**???? Vote of Confidence** - To scale the blink-network and avoid latencies due to incompetance to propagate transactions faster, the network decides on a vote to kick un-fit nodes as per its requirement. Vote of Confidence involves selection, voting, and elimination by increasing requirements for kicked nodes to participate in the election as a contestant.

*[Joby Reuben](https://www.github.com/jobyreuben)*


- Whitepaper Section 3.1.2, Active, Level: Node
- Vote of confidence (VoC) can improve requirement therby increasing the difficulty rate to join the producer cluster. Thus way better and bandwidth stronger nodes capable for faster propagation can join.
- To eliminate nodes, the mean value of all selected bandwidth of nodes shall be taken as a threshold to vote on elimination
- Each node shall have a rate of elimination percentage for which it can vote. This is to offload decentralization ethics towards node-operators. Minimum the rate of elimination, maximum the decentralization of the network.
- Each node can vote on un-fit nodes below its rate of elimination of the mean value, till the next selection of bandwidth-proofs. The VoC result will be published before the Selection of Bandwidth Proofs.



## VoC Result

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Vote of Confidence result will be calulated at first before the selection of bandwidth proofs. From the Voting commences till the epoch election starts, the producers will be able to post their votes.
- 51% of the majority as per each public key/ nodes weight of their proofs are taken in need to get eliminated
- After the elimination of certain bandwidth proofs and their publickeys, the mean value is found and published for requirement for selection of proofs
- The Result is found before the selection of proofs, where its mean value can alter actively, hence the node's bandwidth proofs selections are randomized and should be predicted actively


## Escrow Rate

**???? Escrow Rate** - To restrict spending of blinkcoins, blinkchain's native coin during recessions, bear markets similar to a central bank that icreases borrowing rates to reduce spending. Escrow rates are levied to delegators on their blinkcoins every epoch to retrict certain supply for a fixed amount of period i.e., 500 epochs (5,000,000 blocks) or approx 21 days. This rate is known as SERC (Staking Escrow Rates for Collateral) which will be hiked, lowered according to market conditions determined by the network per epoch.

*[Joby Reuben](https://www.github.com/jobyreuben)*


- Whitepaper 3.3.4, Passive, Level:Node
- At Gensis Epochs, the minimum SERC is set at 1%, which is equal to 10 pulses. SERC hikes are expressed in pulses where each pulse denotes 0.1%.
- First 3 (n-2,n-1,n) epochs are known as Genesis epochs, after which the SERC rate is calcualated variably. For 4th Epoch (n+1), the first 2 epoch's (n-2,n-1) closing oracle rate (400th slot oracle rate - for every slot the oracle rate is updated) is taken to find the change or number of pulses
   > $Pulse Change_{\epsilon_{(n+1)}}=\frac{\epsilon_{(n-2)O_{cr}}- \epsilon_{(n-1)O_{cr}}}{\epsilon_{(n-1)O_{cr}}} \times 100$
- After the pulse is found, it changes the SERC rate for the new epoch
   > $New_{SERC rate} = Current_{SERC rate} + (Pulse Change \times 0.1) \geq 1\%$


*[Ajay Joshua](https://github.com/I-Corinthian)*


```

//This program will be use update the Escrow rate of the next Epoch 

//For the understanding of the Developers the naming of the Epoch Json file is done by 
     n  - represents the current Epoch 
     n1 - represents the next Epoch to be produced
     pn - represents the previous Epoch
     pn2- represents teh n-2th Epoch

//Epochreq_Json is the json file with all the epoch req For each epoch
//pn_epoch is the previous epoch
//pn2_epoch is the n-2 th epoch



function Set_EscrowRate()
{
 Epochreq<---Epochreq_Json
 (int) pulsechange = (Oracle_rate(pn2_epoch)-Oracle_rate(pn1_epoch))/Oracle_rate(pn1_epoch)
 escrowRate = current_escrowRate + (pulsechange*0.1)
 IF escrowRate >= 1 
   return escrowRate
 ELSE 
   return 1
 End IF
 Write escrowRate in Epochreq[escrowRate]
}

```

## Token Collateral Requirement

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Whitepaper 3.3.3, Passive, Level: Node
- Every token will have different requirement in oracle rate that is to be collaterilzed per block
- Its requirement can be dertermined by simple math, which every node has to find, before commencing the epoch to validate if the token in the specific block is collateralized in full
- For n+1 epoch, its n-1 epoch is taken to find its collateral requirement which is given in oracle rate.
   > $Token(k) Stake/packet= MedianVolume(\epsilon_{n-1}) + Escrow Rate \text{ in blinkcoins}$
- Each block in n-1 epoch is taken from which each transaction's outputs are focussed and it's initial oracle value is found
  > $O_i = valueLocked \times oracleRate$
- The total oracle value of all transaction's outputs are summed up for a block and every block's median is taken.
- The Median value is the staking requirement, along with it the escrow rate is added. During validation, for a token to be authorized inside a block, it should have more than the minimum requirement, including the escrow utxos.

*[Ajay Joshua](https://github.com/I-Corinthian)*


```
//This program will be help to generate a Json file of all the token and their required collatral to stake

//For the understaning of the Devlopers the nameing of the Epoch Json file is done by 
     n  - represents the current Epoch 
     n1 - represents the next Epoch to be produced
     pn - represents the previous Epoch
     pn2- represents teh n-2th Epoch

//TokenRequirement_Json is the json file with the list of tokens and the collatral needed to stake
//Epoch is the previous epoch
//Oracle_Volume_Json is a helper file with the list of volume of each token in a block


function generate_volume_list(epoch)
{
    Oracle_Volume<--- Oracle_Volume_Json to write the output
    Epoch = epoch

    FOR each slot in a epoch:
        FOR every block in a slot:
            FOR every transacation in a block:
                IF transacation is output AND Oracle_Volume has Epoch[slot][block][transacation][tokenID]:
                    Oracle_value = Epoch[slot][block][transacation][locked_value]*Epoch[slot][block][transacation][exchange_rate]
                    Oracle_Volume[tokenID][Epoch[slot][block][transacation][tokenID]][ith block][volumes] + volume 
                ELSEIF  transacation is output AND NOT(Oracle_Volume has Epoch[slot][block][transacation][tokenID]):
                    volume = Epoch[slot][block][transacation][amount]*Epoch[slot][block][transacation][oracle value]
                    Oracle_Volume[tokenID] add Epoch[slot][block][transacation][tokenID] to the key json file  
                    Oracle_Volume[tokenID][Epoch[slot][block][transacation][tokenID]][ith block][volumes] + volume 
                ENDIF
            ENDFOR
        ENDFOR
    ENDFOR
}



function Set_CollateralRequirement()
{
    Oracle_Volume<--- Oracle_Volume_Json
    Epoch<---read(pn+"epoch_Json") read pervious epoch json
    Tokenreq<---TokenRequirement_Json
    generate_volume_list(Epoch)
    FOR every tokenID in Oracle_Volume:
        Tokenreq[tokenID] = Oracle_Volume[tokenID]
        Tokenreq[requirement] = Median(Oracle_Volume[tokenID][block_number][volumes])
    ENDFOR
    Delete Oracle_Volume
}

```

## Transaction Fee Rates

**???? Blink Fees** - In Blinkchain transaction Fees consists of a base gas fee and a transfer fee. Transfer fee is only applied when the UTXO has value. Gas fee is applied on all the UTXO scripts as every script should execute to validate the transaction. Gas Fees are given in per unit fee in oracle rate due to non-native transaction fee model and it is fixed per 8500 epochs (1 year) and can be increased by conducting a vote from producers. Transfer fee is variable percentage fee levied on individual UTXOs, changes per epoch based on volume of outputs - Higher the volume lower the fee and vice versa.

**???? Wink** - The smallest denomination of a token, similar to sats in Bitcoin to avoid decimal places and carry on with integer values. 1 wink = $10^{-8}$ Token ; 1 Token = $10^8$ Token winks. For e.g., 1 ETH = 100000000 ETH winks in Blinkchain. Every asset value, oracle rate, fees will be given in winks all over blinkchain's constraints.

*[Joby Reuben](https://www.github.com/jobyreuben)*

- In the first year the gas fee per unit is set at $0.0001 = 10000 winks. Transfer fee ranges from min 0.005% and max at 0.05% 
- For every 8500 epoch all the votes are submitted along with bandwidth proof and its average value is taken as the increase in gas fee per unit.
- During genesis epochs (n-2),(n-1),(n), the transfer fee is set at 0.05%, from n+1 epoch the transfer fee is calculated
- Transfer Fee is based on total volume per epoch in oracle rate, where each epoch's utxo output's initial oracle value is summed. Such that n-2 & n-1 epoch's total volume in oracle rate taken to decide to change transfer fee for the n+1 epoch.
- Standard Deviation of n-2, n-1 epoch's total volume in oracle rate taken, if Current Resarch (CR) < 0.75 transfer fee doesn't change, if Current Resarch (CR) > 0.75 then it is decided to calculate change percentage.
- Each change only either +0.0005 or -0.0005, if If Volume of n-2 < Volume of n-1 then there is higher volume of transactions requiring to lower the transfer fee. If vice versa ,it should increase the transfer fee

## Rent Rates Fixing 
**???? Blink Rent** - In Blinkchain UTXOs are rented and not permenantly stored, when it is expired, it is replaced by its fingerprint for which to recover the owner needs to pay a penalty fee along with a proof-of-ownership. Rent time is decided by the transaction fee paid to the network for the specific UTXO since blinkchain imposes fees on individual new output UTXOs created. For each epoch producers vote on their fee per byte per 1000 epochs (approx 41 days), and the maximum fee would be the gas fee paid for P2PKH script in per byte. Every votes are taken and its median is found per epoch and the Rent rate is decided. For users benefits, rates should be cheaper for maximum adoption. As per current centralized storage rate it is $0.0000000002 per byte per year, from this a suitable rent should be fixed in UCurrent Resarch (CR) winks e.g., 1 UCurrent Resarch (CR) wink per byte per 1000 epochs.

*[Joby Reuben](https://www.github.com/jobyreuben)*

- From Selected Bandwidth Proofs take Rent rates in USD winks (Oracle rate)
- Find median and set as the rate per byte per 1000 epochs. 
- For the given byte of script, the total blocks lifetime will be find out for every UTXO script. 

???? **Conclusion**
 
Thus, the packet leaders i.e., block producers are assigned randomly according to their weights and bandwidth. Block size and time for the upcoming epoch is published i.e., determined by the network itself on parameters. For the next election, from the VoC votes, requirement (difficulty rate) to join the network will increase thereby increasing the scalability and faster propagation proportionally with stronger nodes for high-throughput blinkchain. The collateral requirement with Staking interest rates are published for further validation with increasing value of each blinkcoin of holders, delegators and investors. Each epoch's Transfer fee and Gas fee per unit is assigned and for every 8500 epochs the gas fee is put on to vote to increase as per CPI. The Rent rates are also fixed for the epoch taking rent positions from the producers.


# Active Updates
## Oracle Rate of Tokens
## Client Reputations
## Oracle Reputations
## Node Weight
## Vanity Addition/Change


# Transaction Validation (Local Mempool)
These validations are done when transactions arrive in Local-Mempool waiting to be binded in the next block the producer mints.
## Client-Witness & Vanity Validation
**???? Blink Clients** In Blinkchain, to enforce decentralized regulation and proper taxation, in chain level, it will not be advised to store regional verification, tax_slabs etc. To avoid various scalability issues, Blinkchain offloads verification to Client Wallets. Each wallet client have to sign their users transactions to identify themselves. Clients are responsible for fact-checking regionality, type, etc whereas Blinkchain only approves on Client's reputation. There'll be dishonesty approaches on data, tax evasion, these clients reputations are maintained similar to a Oracle Reputation discussed in Oracle section of Whitepaper. Thus when a client looses it's reputation it looses its authority to its user transactions. Users can move to different platforms with their private keys which are reputable. Hence, only reputable client wallet's transactions only will be accepted on chain. These wallets can add tax_slab (gains tax only) for every transaction, while the government can look for client's malpractice in the public ledger when adding faulty tax-slabs. Thus, Blinkchain acts as a settlement layer and provides flexibility to governments to regulate client wallet applications.

**???? Vanity Addresses** In Blinkchain, to identify a regional and type of wallet, the nodes require vanity prefixes that will be predefined and whitelisted denoting each country. Each country will be added upon a proposal that will require selected bandwidth proof owners of the running epoch to sign and publish the transaction which will add new vanity representing its government's wallet address on the prefix. This sign shall include the government wallet id and the DAO's signature.

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Check if witness signature is attested for the transaction. Witness would be a public key signing the signed transaction of the owners
- Validate from the reputation approved Client's Public Keys 
- Reconstruct the Public Key Hash - Base58 Hash
- Identify the Vanity Pattern
- Vanity would be checking list of approved vanity addresses


## Fee & Tax Validation

**???? Blink Taxes** - In Blinkchain taxes are imposed in a transaction level to assist governments to regulate decentralized currency payments. The types of taxes as per now, 1. Gains 2. Layered. Gains tax is taken during appreciation of asset upon spending. Here taxes are only taken during spending of UTXOs. Layered Taxes are Sales taxes during a merchant purchase which directly pays the government, the Sales tax and ease audits onchain immutably.  

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Since, oracle rates and fee rates changes actively, the transactions initial propagated time's (IPT) slot or block height is taken to validate fees and taxes
- From the initial propagated time oracle rates of the token, transfer fee & gas unit fee is found, as the ledger stores everything on script level.
- In a client created transaction of inputs and outputs, the outputs will have a difference without adding fees and taxes in utxos
   > $GasFee=Epoch_n(UnitFee) \times UTXO_n(\sum OpcodeUnits)$
- Gas fee and transfer fee is found for every UTXO output of the transaction
   > $Transfer Fee=Epoch_n(Transfer Fee \% \times 10^{-2}) \times UTXO_n(TotalValue)$
- In Inputs of the transaction, addition to UTXO's index, script, every UTXO will have a tax-slab & an exchange rate attested to it found in the inputs. 
  
```
" asset - id " : xxxxxx
{
"value" : 8000000 ,         // 0.08 Blinkcoins
"script" : " OP_DUP .... 76 a9148c7e252 ... OP_CHECKSIG " ,
"exchange_rate" : 10 ,     // if 1 BLINK wink = 10 UCurrent Resarch (CR) wink
"tax_slab" : 1525 ,        // if 15.25 percent is tax cut (10^2)
"expiry" : 45000000
},
]}
```
- From the exchange rate & tax slab, the total value to be taxed for gains is found and only imposed if the profit is positive
   > $ (currentExchangeRate * total Value)- (exchangeRate * total Value) = Profit > 0$
- The current exchange rate denotes the initial propagated time's (IPT) of the transaction's asset.
- Tax is levied on the profits and the value is found in asset denomination.
   > $GainsTax= (Profit \times (taxSlab \times10^{-4} )) \times currentExchangeRate$
- All of UTXOs as output values and its fees specific to it along with the gains taxes calculated from the inputs of the tx is summed up and the difference is found with Total input value. The difference that is unallocated, unaudited is Layered Tax.
- Since Layered taxes has different slabs/categories/models it is best to avoid it onchain and offload to client applications to construct layered taxes in difference outputs.
   > $LayeredTax=Input - (Output + \sum (GasFee + Transfer Fee + Gains Tax))$
- If the provided Tx Difference > (Gas Fee + Transfer Fee + Gains Tax), it is verified.


## Exchange Rate Validation

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Exchange rate is a variable written to every UTXO in transaction level (not embedded in script). Hence it is validated by nodes and rejected upon.
- Each transaction will have a point of propagation - time of creation of transaction by a producer node.
- Since oracle rates differ for every slots, it is not static and bitcoin UTXO model doesn't updates similar to Ethereum.
- Hence the block's height of point of propagation or initial propagation block height is taken, compared with it's slot's exchange rate for the token.
- Each node will save history of exchange rate which can be computed by the data available in the ledger.
- If its exactly the same it is validated as true
  > $UTXO_n(ExchangeRate) = Slot(ExchangeRate(BlockHeight_{PoP}))$
## Expiry Validation

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Each UTXOs minimum expiry should be 10000000 represented in block height
  > $Expiry=Block_n(Tx)+expiry= n +K \geq 10000000$
- The total byte of a UTXO calculated from its sum of opcodes or bytecodes
- Each Bytes rate per 10000000 blocks are set per epoch, and are taken to find per UTXO expiry

## Pre-snip Creation
- Based on next epoch's block size and time actively from VOC result
- Find next epoch's requirement actively alculating requirement per tokena and by maintaing each token's funded stake UTXOs
- Bundling Transactions Based on FIFO
- Transactions removal of specific tokens from bundles if requirement is not met before target block
- Keeping all bundles less than the block size max.


# Snips Construction
## Snip Headers

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Each snip will have a header hash, which will be hepful in identifying, graphing other snips as a block
- First snip should be SHA256 hash of a random number (VRF)
- Following snips header hashes should continuous hashes of the first hash

## Clock Hash-Concate

**???? Hash-Clocks** - In every producer node client, in every block, hashes are concated with transactions to cryptographically prove a timestamp. A single threaded hash-clock typically SHA256 function is run continously to attach pre-images, external transactions within the snips.

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Continuous SAH256 Hashing from a random VRF number limited to single thread
- Concate Pre-created Serialized transactions with a pre-image and run hash-clock
- Bind Pre-image and transactions and propagate as a snip
- If VRF number is not the first snip's pre-image of concated txs, then add first-preimage with the first snip.

## Collateral Snip

*[Joby Reuben](https://www.github.com/jobyreuben)*

- Find State UTXOs with Values staked for the producer node of the epoch
- Segregate Stake UTXOs based on asset-ids and find if it satisfies the requirement in oracle rate of current slot.
- If requirement is passed, only take Stake UTXOs near to the requirement.
- If requirement is not passed, ignore the Stake UTXOs of the asset-ids

<!----- [Draft]
- Position Update of Stake UTXOs
--->
## Transaction Snip Construction

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

## Coinbase Snip Construction

<!----- [Draft]
- All fee outputs are segregated by accepted and non-accepted tokens
- --->


# Snips Validation
## Snips Graphing & Spacing

*[Joby Reuben](https://www.github.com/jobyreuben)*

- From the first snip (collateral snip), find the header hash and produce continuos hashes
- Find if any snips contain the images of hashes produced and graph it
- From the first snip begin the time-clock and reject further snips if the published per epoch block-time is over
- Add Produced Kamikaze proof for the block and attest to the chain

## Snip Order
- Snips should be ordered in the exact way
  1. Collateral Snip (Stake UTXO to Collateral UTXO only)
  2. Transaction Snips 
  3. Coinbase Snip (Collateral UTXO to Payback UTXO only)
## Tax Verification
To be written (TBW)
## Fee Verification
To be written (TBW)


# Pruning UTXOs
- 
## Fingerprint Replacement
To be written (TBW)
<!----- [Draft]
- Recent 2 epochs should not be pruned
- Before epochs spent utxos, expired utxos are searched
- UTXOs are replaced by a single fingerprint  of the hex's hash
- If every UTXO is pruned by fingerprint in a transaction, then whole transaction is replaced by a fingerprint hash
- When 16 transactions are pruned completely it is replaced by a node hash
- Thus whole of epochs can be pruned.
- --->



# Scripts \& Proofs
## Basic Parent-Child

<!---

- Parent contract can have another parent, multiple parent
- Child only have a single parent
- When child executed, its parent is called
- When parent has another parent it is called
- And if the child can be spent or not is validated

--->

## Stake UTXO

<!----- [Draft]

UTXO 1 - Parent - Conditions and Positions

UTXO 2 - Parent - Moderator

UTXO 3 - Parent - Creator, Asset-id, Limits

UTXO 4 - Child  - Value


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

## Bandwidth Proof
- Node Weight Snapshot (Epoch)
- Gas Vote (8500 Epoch)
- VoC Vote (Epoch)
- Rent Rate (Epoch)
## IHR Proof
## Kamikaze Proof
- Null Packet
- Timeout
- Denial of Transaction Attack
- Queue Cutting Attack
## Wallet-Reputation Proof
## Expiry Recovery Proof
## Vanity Addition/Change

# Opcodes

## Gas Units Mapping

## DELEGATECALL OP_CODE




# Messaging Protocol
## Basic Origin - Destination
## Fresh Tx Propagation
- Tx Routing
- 
<!---
- For direct messaging, lightning hop is used
- until the message is received on the end, it is encrypted
- The invoice will be generated by the details each node provides in bandwidth proof
- Each hop is pre-determined and used for gossiping the tx point to point
- For every forward, backward propagation, specific snips are sent to specific nodes
- To connect faster if known, the sender will have to give a direct peer connection for faster propagation.
- Snips propagation should be faster hence connection requests should be made and approved, as per election results
- For un-confirmed propagation, these hops should attest a confirmed blocks as a proof to know when a transaction is arrived to the network to tax and collect fees based on it. Due to changing fees and txs are constructed full and propagated to the network for parallel validation.
- Since taxes and fees changes every slot - 400 blocks some transactions may get attested to a  block later due to tps per token basis.
- Each epoch leader will have a set of images, which can be used by the client to construct a question which can get the answer in a zero knowledge proof with its first propagating time from first propagating node towards the epoch leader
- same can be done for sending to packet leader also, the packet leader makes a question, for which the answer is found during hops of propagation
- It is similar to lightning network propagation
- It provides the slot which the transaction is propagated, and it takes the fees and oracle rates from it for tax and fee validation.
- -->
## Snip Propagation

# Clients

## Wallet Client

### Updating Balances
### Constructing Transactions
- Layered TX
- Basic Tx (Gains+fee difference)
### Client-Witness Signature
### Propagation to Network

## Delegator Client
## Oracle Client
## Archive Client


