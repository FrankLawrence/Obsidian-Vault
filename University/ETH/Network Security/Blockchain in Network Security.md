---
Tags: lecture
Created: 2026-01-22 12:46:22
---
(Links:: [[Network Security]])
# Byzantine Fault Tolerance
- At least 2/3 of miners/validators always online
	- Alice makes transaction -> submits transaction to validator network -> verifies and stores transaction

> [!definition] Safety
> Undesirable things never happen

> [!definition] Liveness
> Desirable things will eventually happen

# Adversary
## The Network: Worst possible schedule
- Properties
	- **Synchronous**: A message sent will be delivered before a maximum (known) delay
	- **Asynchronous**: A message sent will eventually be delivered at an arbitrary time before a maximum (unknown) delay
	- **Partial Synchronous**: the network is asynchronous but after some time it enters a period of synchrony

> [!danger] Challenges
> - Theoretical models: Need careful implementation to ensure we approximate them, e.g. retransmission
> - Memory: Naive implementations use infinite buffers. Identify conditions after which retransmissions are not necessary and buffers can be freed
> - Asynchrony means the protocol should maintain properties for any re-ordering of message deliveries
> - Unknown delay means delay should be adaptive to ensure robustness

## Bad Nodes: Arbitrary behavior
- Properties:
	- Correct/honest/good: Will remain live and follow the protocol as specified by the designers of the system
	- **Byzantine**: Will deviate arbitrarily from the protocol. May respond incorrectly or not at all

> [!danger] Challenges
> - **Crash & recover**: still a correct validators with very high latency. Need persistence to ensure this
> - **Rational**: honest validators may have

# Network Security
- **Validators**
	- Validators are exposed (not in datacenter nor on beefy machines)
	- Highly dynamic set of validators
- **Clients**
	- Different types of target links: clients-validator and validator-validator
	- Highly dynamic clients
	- Clients have no fixed identity
	- Unclear validator selection algorithm
- **Admission control**
	- No established way to run pre-checks on input transactions
- **Ordering**
	- How to find the best path to send the block to another node?
	- Ordering leaders 
		- linearising the sub-DAG is simple

---
References: