# Governace Smart Contract By WEB23

Decentralized protocols are in constant evolution from the moment they are publicly released. Often, the initial team retains control of this evolution in the first stages, but eventually delegates it to a community of stakeholders. The process by which this community makes decisions is called on-chain governance, and it has become a central component of decentralized protocols, fueling varied decisions such as parameter tweaking, smart contract upgrades, integrations with other protocols, treasury management, grants, etc.


Ultimately, governance contracts tend behave like multisignature wallets with votes weighted by token balances of the voters.


## A proposal is an Blockchain transaction: an address or list of addresses, and a calldata or list of calldatas. The community (holders of tokens that give them the right to vote), propose Ethereum transactions and based on the outcome of the vote, the transaction is executed on chain, or defeated if it doesn’t pass the election.


For actions that are not done on chain (such as changing the legal license of a piece of software), a message granting the rights is simply signed.


## Useful terms
Before we start explaining the contracts, it’s helpful to know the technical terms of the governance contracts.


## Proposal
Every vote begins with a proposal, which was described earlier. It is always an Hedera transaction that can be signed, I.e. it has a target address(es) and calldata(s).


To prevent proposal spam, contracts usually have some kind of a filter for who can create the proposal, usually an adddress that must hold a certain percentage of the total supply of the governance token.


Under the hood, a proposal is usually a solidity struct with some flags about its current state, the votes applied to it, and what transactions will be executed if the proposal passes.


## Vote
Unsurprisingly, a vote is an ethereum transaction where the voter votes for or against a proposal. The vote is usually weighed by the amount of tokens the address held at the relevant snapshot.


## Quorum
If no action could be taken unless 100% of the token holders voted, then it is very likely nothing would ever be accomplished, as the system would grind to a halt if only one token holder decided not to participate. On the other hand, if only 1% of the votes were required for an election to be valid, it would be too easy pass undesirable proposals.


For the fate of a proposal to be decided, it must reach a quorum threshold (a percentage of the total possible votes) within the voting period.


## Voting period
Proposals don’t wait around indefinitely waiting for the quorum to be reached. Otherwise, the governance proposal might be executed at a time when the circumstances that prompted the proposal change. This countdown starts as soon as the proposal is created and if quorum isn’t reached within the time limit, the proposal is defeated.


## Queued and Execution
If enough votes passed the quorum threshold in favor of the proposal, before the voting period expired, then the proposal is considered the have passed. For safety reasons, there is usually a time delay between when a proposal succeeds and when it actually gets executed.


## Timelock
Not to be confused with the voting period, this is a delay between when a proposal has been approved and when the action is actually executed.


Consider a situation where a controversial proposal is in the governance contract, and a subset of dissenting users will withdraw liquidity if the proposal is enacted. The timelock gives them a window to leave after they see the lost the vote.


Giving users a chance to take action against unfavorable proposals incentives proposers to only include proposals that won’t cause a revolt.


## The phases of governance
There is no universal specification for how a governance proposal is created, voted on, and executed. However, you can roughly expect it to follow some approximation of these transations

Pending ⭢ Active ⭢ Defeated ⭢ Canceled
							  ⮑ Succeeded ⭢ Queued ⭢ Executed
								⮑----------⮑-------⮑  Expired



Who has the authority to create a proposal (and move it to pending) is protocol dependent. A common pattern is that wallets which hold enough of the governance token can create a proposal. Similarly, who can transition the state to “canceled” is protocol dependant also. There is no universal standard for who has the authority to execute these actions.


When unsure about when a state transition can occur, it’s best to simply read the governance solidity smart contract directly.

