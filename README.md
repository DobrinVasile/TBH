# TBH
IDEAS for inspiration 
TBH 2024
13-14 April @ Odeon Theatre


BWARE LABS 
Blockchain transactions executor
Implement a service for supervised execution of blockchain transactions.
The core API would support:
creating a new Ethereum account within the platform
posting a transaction to be executed by an owned account
querying the status of transactions submitted by an owned address

The service should ensure that each transaction is monitored and retried if it is pending for too long without any client intervention. Chain reorganizations and unexpected events such as txs being dropped from the mempool should be addressed too.

This system should scale with the number of clients and transaction volume, and ensure each transaction is eventually mined and confirmed.

Reference product: Openzeppelin Defender

In addition to ensuring transaction inclusion, another important goal is to optimize gas fees by designing/using an accurate gas price oracle.

The end user should also be granted enough control over their pending txs:
To drop or edit
To enforce orderings (additional to the native one of address nonce)
To set expiration times
To request a tx to be mined within T seconds with probability P
etc

Finally, this service should support any EVM-compatible chain.

Data consistency layer
In the context of load balancing RPC requests over a pool of Ethereum nodes, there might be the case that the nodes are not synchronized to the same block. Some nodes may be far behind the chain’s head block.

Because certain RPC methods are resolved within the context of the "latest" block known by the full-node, a scenario might occur where a client obtains outdated data subsequent to already receiving a current version of it during polling and due to the load balancing.

This scenario can be minimized by removing unsynced nodes (of threshold blocks) from load balancing. However, this is not completely addressing this issue and would theoretically require a threshold of 1 block, which consequently reduces the overall effective throughput.
Design and implement a protocol that guarantees data consistency over all RPC requests served through the load balancer or over all requests of each individual client. The resulting impact on performance metrics such as throughput and latency should be minimal.
Secured ETH validator creation
There is a known vulnerability when creating validators on Ethereum PoS.

Assuming that BLS precompiles would be available in EVM, design a protocol for secure creation of validators by a staker who wants to use an operator’s supplied validator key without risking their ETH or undergoing a complex validation themselves.
TheGraph parallel indexing
A subgraph defines a set of entities (database tables) which will be populated by the indexing process and exposed via GraphQL.

Currently, the indexing is executed sequentially: event logs, blocks and function calls are fetched via RPC and transformations are applied one by one.

Adapt the current indexer implementation and possibly the subgraphs definition language to enable parallelization of entity updates that are known to be data independent.
These can be specified either by the subgraph developer or be automatically constructed by running a syntactic analysis on the subgraph definition.

This would substantially decrease the indexing time for large subgraphs indexing DEXes, ERC20s and NFT collections.

Delegations notifier

Develop a service that alerts delegators regarding outstanding events on their stake locked on multiple staking protocols. A user can subscribe to a particular delegation (theirs or others) and configure their preferred media for receiving notifications: telegram, discord, slack, email etc.

Events that may trigger a notification for a particular delegation might be:
Validator missed a block resulting in a loss of rewards for the delegation
Validator has been slashed
Validator changed their commission fee
Network’s reward rate (APY) changed
Validator is not in the active set anymore
Delegation is not in the active set anymore (in case of Moonbeam for example)
Delegator has been evicted by the pool owner and would stop producing rewards without noticing it
Delegation changed state (which could mean that delegator’s wallet has been compromised)
Validator changed any of its configuration parameters
Unlocking period ended for the delegation and stake can be withdrawn
Nice to have: compare staking rewards between different delegators, highlighting significant discrepancies and possibly triggering alerts when these discrepancies exceed a certain threshold.

The proposed app should treat each supported staking protocol independently and extract as much information as possible into descriptive notifications.
Users should be able to filter which protocol, delegations and event types they are subscribed to.
A suitable protocol to start developing a prototype for would be Aptos delegated staking.




GENEZIO

NOTE: If you choose to implement one of these problems, you should deliver a technical demo with a Genezio deployed app. Get extra points by implementing the following challenges using Genezio. 
Smart test generation
Given a GitHub repo that has backend code written as classes and methods, write a program that will automatically create unit tests for the server code. These tests will then run on a daily basis against that repo. It will use LLMs to understand the code and comments to ultimately generate the tests.
Notify me
Create a mobile app that, when installed, will generate (and display) a unique URL that, when called with a payload, will send the user that installed the app a notification with the payload data. When one opens the app they can see the last 10 notifications - content and date.
Alexa DIY
Create an Alexa-like raspberry-pi project with a microphone and speaker that will act just like Alexa, but will have OpenAI’s APIs behind it. I will be able to configure it from my phone to give it my own OpenAI credentials.
Smart/AI generated documentation
Based on a given source code for an API, generate documentation and/or a helper LLM chatbot for that product. Support easy deployment for the documentation using a static site generator (e.g. Docusaurus).
Git notification Slack bot
Build a system that integrates with Github, Gitlab etc. to automatically send notifications to Slack channels when a pull request is submitted. Support message customization to include relevant information (pull request author, reviewer suggestions, line of codes, CI/CD status etc.).
Security assistant for web applications
Develop a tool that analyzes a deployed web application and identifies potential vulnerabilities that could be exploited for phishing attacks. Analyze the website content (source code or take screenshots of the UI) or scripts for suspicious elements (e.g., fake login forms, misleading URLs, clones of popular web applications).



MULTIVERSX
DeFI/Payments
SplitPay
Description
A dApp that allows users to create groups, add expenses, split them equitably or based on custom allocations, and then settle these amounts directly using a cryptocurrency supported by MultiversX, such as EGLD or any other stablecoin.
Technical hints (those are just some recommendations, you should feel free to design your dApp how you wish)
Smart contracts
Group Contract
createGroup
addMember
removeMember
deleteGroup
Expense Contract
addExpense
splitAmount
splitType
getBalance
Settlement Contract
distributePayment
Frontend
CreateGroups, JoinGroup via invite, ManageGroupMembers
AddNewExpense to Group (select Group by ID), ViewBalances, ViewTransactionHistory
For the Settlement Page -> the user should be able to see how much each member owes or is owed and provides an option to settle balances.
Blockchain integration - for this you could use (but not limited to):
MultiversX SDKs: https://docs.multiversx.com/sdk-and-tools/overview
MultiversX Devnet APIs: https://devnet-api.multiversx.com/ 
MultiversX Devnet Wallet & Faucet: https://devnet-wallet.multiversx.com/ 
MultiversX Tutorials: https://www.youtube.com/watch?v=tv6OBimIX98&list=PLQVcheGWwBRWFjgEGLx1Fv2qF_6UVpSXX 
ChessChain 
Description
A dApp that enables users to play chess games directly on the blockchain. Each move is recorded as a transaction, ensuring game integrity and allowing for verifiable game histories. Use smart contracts on MultiversX to manage game logic, player rankings, and potentially wagering or rewards.
Technical hints (those are just some recommendations, you should feel free to design your dApp how you wish)
Smart contracts
Game Logic
MatchMaking (manage player queues and match them based on their skill level or ranking).
Ranking (for keeping track of player rankings based on game outcomes)
Wagering (place bet on the game). Important: you should also think about some time locks or dispute resolution mechanisms for cases where game is abandoned or disputed.
Frontend
For start, it could also represent only a move translator for the contract, developing the design at later stages.
Blockchain integration
MultiversX SDKs: https://docs.multiversx.com/sdk-and-tools/overview
MultiversX Devnet APIs: https://devnet-api.multiversx.com/ 
MultiversX Devnet Wallet & Faucet: https://devnet-wallet.multiversx.com/ 
MultiversX Tutorials: https://www.youtube.com/watch?v=tv6OBimIX98&list=PLQVcheGWwBRWFjgEGLx1Fv2qF_6UVpSXX 
Some other ideas:
Lending order book;
Auto-compounding for staking smart contracts:
A smart contract used to claim the rewards for a staking position and reinvest it; for economic incentives add fee cut from reinvested rewards
A bot implementation that triggers the auto-compounding for deposited staking tokens
Automatic crawler to resolve order book orders between single and multiple DEXs. Order book SC and external bot to resolve the trades, on the SC or with a combination on AMM.
Shorting and longing on MultiversX using Hatom contracts and the DEX.


Blockchain Infrastructure, Tooling

On-Chain Messaging App
Description
Develop a secure, reliable and private chat system on MultiversX.
Technical hints (those are just some recommendations, you should feel free to design your dApp how you wish)
Smart Contracts
Message Storage: a contract that could handle the storage of message references. It could store message hashes or pointers to off-chain storage locations (for cost-reduction purposes).
Access Control: Managing permissions, ensuring that only intended recipients can decrypt and view messages.
Frontend
It should not be very complicated, but stilluser-friendly. A good starting point can be the following: https://github.com/multiversx/mx-template-dapp .
You could also think about making all messages public and develop a Twitter-like app, where users can publish immutable messages for the public.
Blockchain integration:
MultiversX SDKs: https://docs.multiversx.com/sdk-and-tools/overview
MultiversX Devnet APIs: https://devnet-api.multiversx.com/ 
MultiversX Devnet Wallet & Faucet: https://devnet-wallet.multiversx.com/ 
MultiversX Tutorials: https://www.youtube.com/watch?v=tv6OBimIX98&list=PLQVcheGWwBRWFjgEGLx1Fv2qF_6UVpSXX 
ChainCost - Transaction Simulator & Gas Estimator
Description
A developer-focused tool designed to help developers and users accurately estimate transaction costs for various operations. It should provide a detailed simulation of transactions (including SC interactions, token transfers and more) helping with budgeting and optimizing applications.
Technical hints (those are just some recommendations, you should feel free to design your dApp how you wish)
You could use already existing endpoints:
https://docs.multiversx.com/sdk-and-tools/rest-api/transactions#simulate-transaction 
https://docs.multiversx.com/sdk-and-tools/rest-api/transactions#estimate-cost-of-transaction 
A more accurate solution would be to run an instance of a blockchain somewhere ( 😏 ) and simulate the execution of the transaction. Be aware of the fact that in order to have a 1:1 result, the states from your blockchain instance and the one existing on MultiversX, should match.
On the other hand, smart contract developers would very much appreciate a method to estimate the gas costs for a certain code snippet:
Open VSCode
Write your code
Select it -> and ask for an approximation in costs.
Useful materials:
https://github.com/multiversx/mx-ide-vscode 
https://github.com/multiversx/mx-chain-mainnet-config/blob/master/gasSchedules/gasScheduleV7.toml 
On-Chain Inheritance
Description
A dapp on MultiversX that enables individuals to securely and automatically transfer their digital assets to designated beneficiaries upon their passing or after a specified period of inactivity (or time - but for this we could think about a timeLock period). The solution should respect privacy, ensure security, and comply with legal requirements for inheritance (this could be solved at a later stage)
Technical hints (those are just some recommendations, you should feel free to design your dApp how you wish)
Smart Contracts:
Asset Planning - smart contract where users can list their digital assets, designate beneficiaries, specify conditions under which assets should be transferred.
Inactivity Monitor - it is self explanatory - detecting that an account is inactive for a predetermined period, triggering the inheritance process.
Frontend:
Should guide users through setting their digital estates, allowing them to select between assets, designate beneficiaries and condition setting.
Blockchain:
A variation of this implementation could be the guardian solution implemented already at protocol level: 
https://docs.multiversx.com/developers/guard-accounts 
TimeLock Treasuries
Description
A dapp on MultiversX that allows users to create digital safes. These safes will safeguard digital content - assets, messages, other digital artifacts, allowing them to be revealed/transferred at a specified date in future. 
Technical hints (those are just some recommendations, you should feel free to design your dApp how you wish)
Smart Contracts:
Treasury - a smart contract that enables users to establish their Timelock Treasury, define the digital content, set the unlock time, and nominate the beneficiaries if desired. Pay great attention to the mechanism within the smart contract to ensure treasury’s content remains inaccessible until the arrival of the specified date.
Some other ideas:
Implement a JSON-based contract interactions composer;
Passwords Manager that holds encrypted secrets on the blockchain (as a browser extension);
Autogenerated Interaction ABI;
A tool that is able to read a JSON file that describes the ABI of a contract (the well-known ABI JSON files) and, based on the endpoints and the arguments of these endpoints (as found in the JSON file), this tool is able to dynamically generate a simple HTML-based user interface for interacting with the contract.
Example: having the endpoint Foo with two arguments, first a string, second an integer, the tool should generate a simple form with 2 fields: a text input for the first field, a numeric input for the second one.
Implement a small tool that continuously performs balance reconciliations (balance checks) on the MultiversX API - similar to how Rosetta CLI does it:
https://www.rosetta-api.org/docs/rosetta_cli.html#balance-reconciliation
Endpoints:
https://api.multiversx.com/#/accounts/AccountController_getAccountHistory 
https://api.multiversx.com/#/accounts/AccountController_getAccountTransfers 
https://api.multiversx.com/#/collections/CollectionController_getCollectionTransactions 

While the projects outlined above provide a structured approach toward using the capabilities of the MultiversX blockchain, we welcome and highly value any additional innovative ideas. We value innovation and creativity and we strongly believe that groundbreaking ideas and solutions often arise from the most unexpected concepts and creative explorations. That is why we encourage everyone to think beyond our proposed challenges and think about ideas that push the limits of what the Web3 industry met until now.

PI SQUARED
Sudoku in Zero Knowledge
Background:
Sudoku is a game that's played by filling a 9 x 9 grid with digits
such that each row, each column, and each of the nine 3 x 3 subgrids contains all of the digits from 1 to 9.

Sudoku puzzles started to appear in newspapers in the late 19th century and gathered great interest from the readers. However, the editors of the newspapers didn't know much about producing or solving Sudoku puzzles, so they wanted enthusiastic readers to produce them. On the other hand, the puzzle creator didn't want to leak a solution too early, either. How could the editors of the newspapers verify that a Sudoku puzzle has a solution without knowing what the solution is?

In this challenge, you'll help the newspaper editors and Sudoku puzzle creators by developing a zero-knowledge proof system. Using your proof system, a puzzle creator can produce, for a given Sudoku puzzle `P`, a zero-knowledge proof (ZKP) `pi` that verifies to any third party (i.e., newspaper editors) that `P` has a solution. This way, the editors feel safe to publish `P` in their most liked "Sudoku of the Day" column knowing that `P` is a valid puzzle, while the puzzle creator keeps the solution(s) to themselves.

You will be using `circom` link to build the zero-knowledge proof system and generate ZKPs. 
The official circom website has clear installation instructions and a few simple examples to help you get started. 
You will use circom to build an arithmetic circuit that takes a Sudoku puzzle `P` as input, checks `P` has a solution, and uses `circom`'s toolchain to produce the ZKP. 

Deliverable:
Use circom to write a circuit `Sudoku(P[9][9], ARGs)` where the first argument `P` is an array that encodes a Sudoku problem, with `P[i][j]` being the digit in the (i,j)-th cell (i.e., the cell in the i-th row and the j-th column). If an (i,j)-cell is empty, let `P[i][j]` be 0.
The circuit `Sudoku` is allowed to take other input arguments, denoted `ARGs`, but the only public input argument is `P`. For more about public/private arguments, refer to this section.

The circuit `Sudoku` should satisfy the following soundness and completeness properties.
Let `P` be any Sudoku problem as the public input.
Let `ZKverify` be the ZK verifier as described here, where `public.json` is `P` and `proof.json` is `pi`. 

1. (Completeness)
   If `P` has a solution, then there exists some `ARGS` such that  `Sudoku(P, ARGS)` completes successfully. Therefore, for the proof `pi` produced by running the `circom`+`snarkjs` pipeline over the `Sudoku(P, ARGS)` circuit, we can obtain that `ZKverify(P, pi) = OK`.

2. (Soundness)
   If `P` does not have a solution, then for any ZK proof `pi*` produced by a cheating prover,  `ZKverify(P, pi*) = OK` with a negligible probability.

Hints:
To satisfy (Completeness), you essentially need to ensure that for any `ARGs` (whose design is up to you), if `Sudoku(P, ARGs)` checks then `P` has a solution.
To satisfy (Soundness), we encourage all participants to share their implementations of the circuit `Sudoku` and try to find security issues in their competitors' solutions.



STRIPE

Fraud Risk Analysis and Management Platform
Building a software platform that utilizes advanced data analysis techniques for the identification and prevention of fraud in various domains, such as online payments, insurance, or financial services.
Implementing machine learning algorithms for detecting anomalies and suspicious behavior patterns.
Integration with existing systems to provide a comprehensive and scalable solution for fraud risk management.

Delivery and Commission Management Platform for On-Demand Services
Creating a software platform to facilitate the delivery and management of commissions for on-demand services such as food delivery, courier services, or transportation.
Developing an intelligent commission routing system that takes into account various variables such as distance, time, and order volume.
Integration with digital payment solutions to ensure efficient transaction and revenue management.

AI-powered Invoice Reconciliation 
Design an app that uses AI to automatically match invoices with payments received through Stripe, reducing manual work and streamlining accounting for businesses.
Employ machine learning algorithms to analyze invoice data (amounts, dates, vendor information) and match them with corresponding payments received through Stripe.
Automatically import invoice details from various sources (email, cloud storage).


XOXNO

NFT Image Duplicate Detection System 
- Develop an algorithm that can scan NFT images across various collections on the multiverse X platform, identifying duplicates or near-duplicates. The system should take into account slight variations in images and offer a confidence score for each potential match.
Auto-Labeling System for Age-Restricted Content: 
- Create an AI-based tool that automatically scans and labels NFT images that may contain adult content. This tool should be capable of understanding and interpreting various artistic styles and nuances in digital art to accurately categorize images.
Purchase NFTs with Stripe
- In this challenge, participants will develop a solution that enables users to purchase NFTs on the XOXNO marketplace using fiat currency via Stripe payments, with the backend executing the transaction on the blockchain. The goal is to create a seamless and user-friendly experience for buying NFTs, bridging the gap between traditional payment methods and blockchain technology.

Key Components and Requirements:
Stripe Integration: Participants must integrate the Stripe payment gateway into the platform to handle fiat transactions securely. This includes implementing payment forms, handling card details securely, and processing payments in accordance with Stripe's API.

Blockchain Transaction Execution: Upon receiving confirmation of a successful payment from Stripe, participants must execute a transaction on the blockchain to purchase the selected NFT from the XOXNO marketplace. This involves interacting with smart contracts, submitting the necessary transaction data, and handling any errors or exceptions that may occur during the process.

Transaction Monitoring and Error Handling: Participants should implement mechanisms to monitor the status of blockchain transactions and handle any failures or errors gracefully. If a transaction fails for any reason, the user should be promptly notified, and appropriate steps should be taken to refund the payment.

User Feedback and Transparency: Throughout the payment process, participants should provide clear and timely feedback to the user, informing them of the status of their payment and any actions required on their part. This includes displaying transaction confirmations, error messages, and refund notifications in the user interface.

Activity Monitoring Using AI: Implement AI techniques to monitor and analyze the trading activity of each collection. The system should identify patterns that indicate healthy trading activities versus those that might suggest market manipulation or fraudulent activities.
A Community-Engaged and Activity-Based Verification System for NFT Collections
- Build a simplified and efficient system on the XOXNO marketplace that uses community input and $XOXNO token activity to dynamically manage verified badges for NFT collections.

Key Components and Requirements:

Community Voting with $XOXNO Tokens: Implement a voting mechanism where marketplace participants use $XOXNO tokens to endorse collections they believe should receive a verified badge.
Activity Monitoring Algorithm: Develop an algorithm to track key metrics of NFT collections, such as transaction frequency and volume, to assess their activity level.
Automated Badge Management: Create a system that awards or revokes verified badges based on community votes and the collection’s activity, adhering to predefined criteria.
Dynamic Badge Updates: Ensure the badge status is updated regularly, reflecting any changes in community opinion or collection activity.
Transparent Criteria and Feedback Loop: Clearly define the criteria for earning or losing a verified badge and provide a feedback mechanism for collection owners to understand and respond to their badge status.
	This task focuses on harnessing community engagement and data-driven insights to maintain the integrity and relevance of verified badges in the NFT marketplace.





REGISTER HERE
