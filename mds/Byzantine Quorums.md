# Replication for Fault Tolerance - Byzantine Quorums
## Consensus with Byzantine Failures
- **Byzantine Generals Problem (BGP)** this is reliable broadcast
	- **Agreement**: all non-faulty processes deliver the same message
	- **Validity**: if the broadcaster is non-faulty processes deliver the message sent by the broadcaster

There is no solution to the BGP in a system with 3 processes, if one of them may be Byzantine, if messages are not signed

### Byzantine Failures and Cryptographic Signatures
#### Communication without signed messages
![[Pasted image 20220206172823.png]]
- The LHS receiver gets the same messages in both scenarios
	- It should deliver the same message in both scenarios
	- But no message satisfies the desired properties in both cases
	
#### Communicatio with signed messages
![[Pasted image 20220206172840.png]]
- Byzantine receiver cannot properly sign a modified message

## Quorum Consensus with Byzantine Replicas
### Quorum Consensus
- Each (replicated) operation (e.g. read/write) requires a quorum
- Fundamental Property:
	- If the result of one operation depends on the result of another, then their quorums must overlap, i.e. have common replicas
- consider all replicas as peers.

### Byzantine Quorums: Model
**Processes**: Some servers in U may exhibit byzantine failures
**Network**: any two processes (clients or servers) can communicate over an authenticated and reliable point-to-point channel

#### Problem Definition
- Clients perform read and write operations on a variable x replicated at all servers (in U)
- Each server stores a replica (copy) of x and a timestamp t
- Timestamps are assigned by clients when the client writes the replica

**Safety**: any read that is not concurrent with writes returns the last value written in some serialization of the preceding writes
- Operations are assumed to have **begin and end** events that determine a partial order
- Essentially, this means **linearizability** of read/write operations

#### Access Protocol
![[Pasted image 20220206174114.png]]

##### Gifford
**Read** 
1. Collect a read quorum, to find out the current version 
2. 2. Read the object state from an up-to-date replica.

**Write** 
1. Collect a read quorum, to find out the current version 
2. 2. Write the new value with the new version to a write quorum 

##### Asynchrony
- Both in the read and write operations, a client has to get replies from all servers in a quorum

##### Beginning and End Events
- **write** of value v with timestamp t 
	- **begin** when the client initiates the operation 
	- **end** when all correct servers in some quorum have processed the update (v, t)
-  **read**
	-  **begin** when the client initiates the operation 
	- **end** when the Result() function returns, thus determining the read result 
	
- **op1 precedes op2** if op1 ends before op2 
- **op1 and op2 are concurrent** if neither op1 precedes op2, nor op2 precedes op1

## Byzantine Masking Quorums
![[Pasted image 20220206174547.png]]

### Size-based Byzantine Masking Quorums
**M-consistency**
- client always obtains an up-to-date-value

**Assuming all servers are peers**
- Let f be the bound on faulty servers
- Then we need at least f + 1 up-to-date non-faulty servers
- Thus every pair of quorums must intersect in at least 2f + 1 servers
![[Pasted image 20220206174759.png]]

**M-availability**
∀B ∈ B : ∃Q ∈ Q : B ∩ Q = ∅

**Assuming all servers are peers**
- Let f be the upper-bound on byzantine servers 
- Let q be the size of a quorum 
- Let n be the number of servers 
- Then n − f ≥ q

### Non-byzantine Read-Write Quorums Based on Size
w ≥ f + 1 (1) this ensures that writes survive failures 
w + r > n (2) this ensures that reads see most recent write 
n − f ≥ r (3) this ensures read availability 
n − f ≥ w (4) this ensures write availability

- increasing n only worsens performance (as it requires larger quorums)
	- by increasing n we can increase fault tolerance, as f can increase

### Byzantine Quorums Based on Size: Read operation
![[Pasted image 20220206175228.png]]

### Safety
Any read that is not concurrent with writes returns the last value written in some serialization of the preceding writes

![[Pasted image 20220206175410.png]]
![[Pasted image 20220206175420.png]]

### Naïve Implementation with Concurrent Writes
![[Pasted image 20220206175503.png]]

### Server Failures and Liveness
- By M−Availability, there is always a quorum, even if all f byzantine server fail
- By the write protocol: 
	- a client sends the update (v, t) to servers until it has received an acknowledgement from every server in some quorum Q'
	- **A server** only updates its local copy and timestamp to the received values (v, t), only if t is greater than the timestamp of its current copy

## Dissemination Quorum Systems
![[Pasted image 20220206175928.png]]

### Read Operation
![[Pasted image 20220206175944.png]]

### Size-based Byzantine Dissemination Quorums
![[Pasted image 20220206180012.png]]
![[Pasted image 20220206180021.png]]

## Critical Evaluation
- Compared with state machine replication, Byzantine Quorums appear to require fewer messages, by a large margin
- But the protocols we have seen assume that:
	- Either clients can be trusted
	- Or the information stored is self-verifiable
- To handle other cases, in Phalanx (the SRDS 1998 paper) Malkhi and Reiter use **consensus-objects**, which appear to require about the same number of messages as Byzantine SMR
- Furthermore, these protocols support only read/write operations.