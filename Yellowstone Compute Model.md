![Image1forYellowstone](https://github.com/MorpheusAIs/Morpheus/assets/1563345/80a6c1cf-fe52-42ba-bfd4-91d9faf67f07)

# Morpheus “Yellowstone” Compute Model
### Erik Voorhees
### January 3rd 2023

A suggested revision to the Morpheus tokenomics structure for compute incentivization on a decentralized AI network. 
View on Notion: https://defiant-wolfsbane-830.notion.site/Morpheus-Yellowstone-Compute-Model-2ca9b4e7e1374c898937fdbd1b964cc1


## Summary
In the Yellowstone Compute Model, the Morpheus network pays Providers only for Compute actually provided through a competitive bid process, and allocates the scarce production of Tokens pro-rata to MOR token holders based on balance, rather than on payment. This drastically improves UX while minimizing Sybil vulnerability. Yellowstone also imbues the important metrics of time and a Pass/Fail test to ensure Providers are adequately prompt and accurate. Yellowstone preserves privacy by never sending prompts or results through the Router, and minimizes blockchain transactions to permit a large scale of operation. Through this model, MOR achieves fundamental value as it enables perpetual (though not unlimited) access to permissionless compute, without requiring transactions per inference. 

If adopted, this paper replaces the “Compute Proof, Registration & Reward” section of the Morpheus whitepaper (https://github.com/MorpheusAIs/Morpheus/blob/main/WhitePaper.md)

## Background
Morpheus uses tokenomics to incentivize sufficient and scalable compute as a resource for the purpose of decentralized and permissionless generative AI. In its original conception, Morpheus issued 24% of MOR emissions to Compute Providers directly, pro-rata depending on the inference requests received, and it prioritized inference requests to those providers based on how much MOR they held. 

### From the original white paper:
“The pro-rata MOR transaction fees burned by each Compute Provider serves as proof of the Compute Providers status and earns a proportion of the MOR tokens each day.
 
For example, if there are 100 Compute Providers on day 1 when the network launches, then each one gets a pro-rata reward based on the amount of MOR they have burned via fees. In this case, presuming each of the 100 compute providers burned 100 MOR, then 1% of the 3,456 MOR tokens each day = 34.56 MOR.”

### There are three major issues with this approach:
It requires users to pay per-inference transaction fees. Even if low, this is substantial friction and will cause poor UX and an ever-present inferiority to OpenAI’s UX. It also requires at least one blockchain transaction per inference, which is probably not scalable even on L2s. Each inference event is extremely low cost, and if a blockchain transaction was required, the economics would be infeasible. 
This model is substantially exploitable because expected revenue for compute providers is far higher than actual compute costs. An adversary could thus flood spam inference requests to his own Compute Provider node, and earn a relatively large portion of MOR tokens each day, even though no economic value was provided to anyone. Likely, it would lead to large amounts of early (unused) compute, which disappears once the huge revenue opportunity dissipates, and the MOR spent on that early subsidy would be wasted/lost.  
If inference requests are prioritized based on the MOR amount held by Providers, then the performance of those providers (response time) and the cost of their inference processing are ignored by the network, and it is precisely these two factors which the network should attempt to optimize (response time and compute cost should be ideally driven as low as possible). If the top MOR-holding provider was running a $200 GPU from his college days, inference performance for many users would be extremely poor. Priority should be based on bid price and performance, not MOR holding.

Below is proposed the “Yellowstone” Model which modifies the Morpheus tokenomics for compute provision to address the above issues. This model works regardless of what portion of emissions are allocated to compute, and we’ll assume the status quo of 24% of total emissions. 

### The goals are:
* Enable users to not pay per inference (ideally, to not pay at all)
* Achieve efficient, scalable, and sustainable provision of permissionless compute resource without overpaying for it
* Incentivize low response time and cost competition among compute Providers
* Minimize number of blockchain transactions (whether L2 or otherwise)
* Demonstrate an economically sound fundamental demand for MOR 

## Yellowstone Model
Four Components Involved:

### Users
* Have Queries
* Want fast/accurate compute for free and without censorship/surveillance

### Providers
* Have compute
* Want money (MOR)

### Router 
* High throughput processing engine
* Can be relatively centralized at first, ultimately needs to decentralize

## Standard Weights and Measures

There is an atomic unit of inferences in AI, measured in inferences/second (IPS). This can be conceptually compared to wei on the blockchain. Inferences are used to define rates in the Yellowstone router.  The weight of a single Morpheus AI unit is therefore an inference.  Depending on the type of request, this can be applied to any compute task.  

As AI and blockchain merge commonalities, Morpheus looks to provide an open-source standard of measurements in order to clarify terminology used by both AI and Blockchain. 


There are two types of prompts, defined by the size of response returned by a model:  

***Determined Length Prompts***, where the response considers the length of the response to return. Examples of this are:
- Chat/Image creation
- Disease diagnosis
- Object recognition
- Fraud detection


**Undetermined Length Prompts** require resources to respond that are only knowable after the response is created. Example prompts of non deterministic responses are:
- Sing a sonata about spaghetti.
- Generate a Happy Birthday video
- Combine model X with model Y
- Slice a 3D model into an .stl file

Yellowstone focuses on Determined Length Prompts.  The router described will be constructed in a fashion to handle undetermined prompts in the future, but not to service them today.  To accomplish this we use a standardized measurement of Decentralized AI.

DeAI Rates

### Expressions of inference/second:

| Type | Response | Rate |
|------|----------|------|
| Determined | Language | Inferred Tokens per second (TPS)|
| Undetermined - media | Audio | Inferred Samples per second (ISPS) |
| Undetermined - media | Video | Inferred Frames per second (IFPS) |
| Undetermined - future tech | Unknown Future Format | NA |

The first measure of inference for the Yellowstone router will be tokens. Other inference formats to follow.

### Time

The block time for inference is 12 seconds, meaning a block of inference transactions is published and accounted for 5 times per minute.  

### Compute Contract
*Permissionless smart contract which receives emissions of MOR, tracks credits and debits to Providers, and pays Providers when called.

“Users”: defined as any entity that has a MOR address and sends Requests to the Router, using the compute. This can be a specific individual person sending Requests from a Morpheus desktop node, or it could be a bot, or it could be a company or 3rd party website which interacts with the Morpheus network on behalf of its end-users (end-users' use of inference aren't tracked or  considered in the compute contract, except when there is an inference failure). 

“Providers”: defined as any entity, running a node that provides compute resources, has a MOR address and offers Token bids through the Router. When a Provider wins the bid from the Router, Provider provides the compute resource (GPUs, etc) for various AI models to the User. 

“Router”:  defined as a software application that has a MOR address and negotiates the 2-sided market between Users and Providers. The Router registers and tracks Provider addresses and bids, processes Requests from Users, records [miliseconds] and Pass/Fail tests of processed Requests, and instructs the Compute Contract to credit eligible Providers for payment in MOR. The Router never sends or receives MOR transactions (nor transactions on any blockchain). The Router never sees the content of a Request, nor the response. 

“Compute Contract”: defined as a smart contract that has a MOR address, receives all emitted MOR allocated to the Compute bucket, tracks amounts owed to eligible Providers, and pays MOR to eligible Providers when Providers request payment.

“Token” (“T”): is the smallest amount of letters or pixels bid on vi the router. Often this is ~4 characters of text, or 5x5 pixels of image, etc. This is not to be confused with blockchain “tokens” such as ERC20 tokens or the MOR token itself. 

“TokenMax” below refers to a maximum number of Tokens accepted for payment by the Router. 

“RFC”: stands for “Request for Compute.” A user sends an RFC to the Router, and specifies the [LLM] User desires access to as well as the [TokenMax], which is a cap on the acceptable LT’s in response. User will want to cap this because higher numbers = longer wait times for answers, and count more toward UserMax, which is limited each day. 


### Contract Protections

In order to prevent an attack that shorts or runs the number of MOR by manipulating  using any unused compute, the pool of unused MOR allocated to Compute Providers can be reduced by no more than 1% per block day. This is equal to normal compute emisions + 1%.

### Compute Bootstrapping Incentive

For the first year following the Capital Contract's bootsrtapping period, the top 100 Compute providers will be entitled to a pro-rata amount of 2.4% of MOR emissions.  This is calculated by the routers and accounted for in the compute contract.

## Workflow
1) Users, Providers, and Router all create MOR pub keys (this is their identity, all messages signed as such). 
2) If User hodls any balance of MOR, User may submit a signed Request for Processing “RFP” message to the Router. User specifies [LLM] and [TokenMax].
3) Router prioritizes RFPs based on User’s MOR balance (solves sybil issue)
4) Router selects Provider that supports the [LLM], prioritized based on lowest Bid per Token in MOR. 
5) Router sends liveness check to Provider. If Pass, then
6) Router connects User to the Provider
7) User sends Query ([LLM],[prompt]) to Provider 
8) Provider computes Query, sends Result to User
9) User reports Time [milliseconds] between Step 4 & 5, [Tokens] delivered, and Pass/Fail to Router
10) Router instructs Compute Contract to credit Provider with MOR if [milliseconds] per [oken] is no worse than X% below mean of past Z queries for that [LLM] and if User reported [Pass]. 
(11) (Some time later) Provider requests payment of MOR from Compute Contract and Compute Contract sends MOR payment if valid (first blockchain TX so far, can be batched).

![ComputeContractImage2](https://github.com/MorpheusAIs/Morpheus/assets/1563345/e66ea20c-9851-4f9e-9caa-66c6d798c462)

## Outcome
* User received fast Result for her Query, and paid nothing (this will lead to amazing UX and thus adoption). Solves Goal 1.
* Compute Contract paid for Compute through a competitive bidding process, and a check for quality/satisfaction from the User who ordered it. Solves Goal 2.
* Provider received money (MOR) from Compute Contract so long as response was fast enough. Provider received exactly what she asked for to provide the compute. If her ask is too high, others will bid lower, thus the system is efficient and will drive down Provider prices toward the cost of base electricity.  Solves Goal 3
* Number of on-chain transactions was minimized (many thousands of Queries can flow without a single on-chain TX). Solves Goal 4
* The ability to get fast, free compute drives the demand for MOR tokens to be held by Users. Solves Goal 5
* Step 6 & 7 provide reasonable privacy (Query never touches the Router, nor does Result). Providers are selected somewhat randomly, and never know identity of User other than IP address. Better privacy can be later achieved with TOR + FHE
* MOR balance was reduced from Compute Contract. Contract will be solvent so long as MOR paid < MOR earned per period from emissions.
* If User sends an RFC which exceeds User’s UserMax, the Router will reject the request.

—-------------

## Compute Budget
The Morpheus network needs to determine how much MOR it is willing to spend on compute in a given period (**such as each day, hour or minute**), this is referred to as the Compute Budget. Each period, up to this amount of MOR may be spent by the Compute Contract. This number multiplied by the MOR price gives us a dollar budget for acquisition of Compute each period.

Open question 1: How should the Compute Budget be determined? The simplest idea is to set Compute Budget = emissions into the Compute Contract. This way, Compute Contract would never run out of tokens. But then what to do with the unused tokens, since the maximum would never be utilized each day? These could, perhaps, be granted pro-rata to current MOR token holders. Or, they could be burned. Or, they could remain unused in the Compute Contract, to be spent in the future on Compute (but then this opens more governance questions). 

Open Question 2: **Edit: The flip side of the token econonmic model here is that network throughput is limited by the price of MOR. If MOR goes way up, the network can purchase many more IPS tokens, and the price of IPS tokens long term should go toward the cost of electricity plus some premium if you're using a new proprietary model. If the MOR price goes down, and IPS tokens are near the cost of electricity, the network can't fulfill all the queries on the network since in dollar terms the compute budget for a period is too low. This may be addressed partially since if people really want to use the network, they're buying MOR, and it drives the price up, allowing for more usage. This issue is that if the compute budget period computation is not frequent enough, increased demand for MOR (in terms of token price) won't translate fast enough to network capacity (compute budget). To address this potential issue, the compute budget should be determined on an hourly (or more frequent) basis vs a daily format. On the other side, if the network sees a massive surge in demand in a day, it could literally pause all requests if it runs out of the available IST tokens it could purchase. By making the compute budget computation more frequent, as people ape MOR to use the network more, this can allow for more network usage in real time. 
**

## AccessRate
The Morpheus network allocates the scarce resource of LT production through the concept of the “AccessRate”. The AccessRate determines how many LTs each MOR token can access per day. Unused access does not accrue. AccessRate is always displayed as a quantity of LTs per 1 MOR token (such as 1 MOR = 15,000 LT). AccessRate is determined in part by MaxLT, which quantifies the maximum number of LTs the network can purchase per day.

AccessRate = (1/MOR Supply) * MaxLT
MaxLT* = ((MOR Compute Budget * MOR Price) / LT Price) * 1000
UserMax = MaxLT * User MOR balance

*When computing the MaxLT, the equation includes the MOR price. It may be worth smoothing this out in the equation (7, 14 or 30 day moving average) to prevent wild swings in how much compute the network can handle. Probably the same thinking with IST price (may need to be in more real time like an average of different bids from different providers that day). 


### Example Assumptions: 
MOR Supply = 10,000,000 MOR tokens
MOR Compute Budget = 3,000 MOR tokens per day
MOR Price = $20
LT Price = $0.002 per 1000 LTs
User Balance = 5 MOR tokens

### Example Result:
MaxLT = 30,000,000,000 LTs (this is the maximum LTs the network can buy/produce each day)
AccessRate = 3,000 (thus each MOR token grants access to 3,000 LTs per day)
UserMax = 15,000 (a User with 5 MOR tokens can access up to 15,000 LT’s per day)


Each period (each day), Morpheus as a network has enough funds to buy X number of LTs from compute Providers. X is a function of the amount of MOR the Compute Contract is willing to spend (the “Compute Budget”) multiplied by the current MOR price divided by the market rate for LTs. 
If the Compute Budget is 3,000 MOR, and each is worth $20, then the network can buy (produce) up to $60,000 of LTs that day. If the going rate for 1,000 LTs is $0.002, then the network can buy up to 30 billion LTs (30m x 1000 LTs). 
That potential production of 30 billion LT’s is allocated by MOR balance, pro rata. Assume there are 10,000,000 MOR in existence. A user with 500 MOR tokens (0.005% of total) could freely access up to 1.5m LTs that day. 
So long as Compute Budget is at or below the emissions level, the Compute Contract cannot run out of MOR.  
In reality, most tokens will sit in wallets and exchanges, and only a fraction will be used to demand the LT production.





## Notes
* Fundamental demand for MOR comes from Users who wish to have access to generative AI and other forms of compute on the Morpheus network. 
 
* Provider’s hardware type is irrelevant to the network, so long as they satisfy the User’s pass/fail test. Any Provider bidding on more Queries than they can efficiently process will be penalized by failing this test.

* The above model importantly pays Providers ONLY when there is demand for their compute. This prevents the situation where large portions of MOR are emitted prematurely when the network doesn’t need it. 

* Providers should need to prove they have a given LLM, by signing hash of LLM model with their key. This doesn’t prove they used it, but it proves they downloaded and installed it, which represents work, thus preventing some forms of sybil-sensitive fraud. If Providers provide garbage results to User, User can send [Fail] along with [miliseconds] back to Router, and Provider won’t be credited for that compute. Morpheus doesn’t need all answers to be perfect… it only needs enough answers to be good enough, relative to competing alternatives. 

* Sybil attacks of flooding the network with RFCs is prevented by the AccessRate. The “cost” of sending and RFC is the cost of acquiring a MOR token divided by the number of RFCs submitted on its behalf. Cost is thus never zero, and yet a user won’t feel a loss each time an RFC is made. 

* Pass/Fail is determined by User, and polices quality to some degree. User conveys Pass/Fail result alongside [miliseconds] back to Router. If Fail, either no reward or penalty point (TBD). There is no incentive to falsely Fail a Provider (no monetary incentive in doing so). This mechanism prevents Providers from sending fast but useless Results.
Consider: perhaps No Reward occurs on Fail only if User MOR > Provider MOR. Otherwise, just a negative point which Router can use in its privatization logic.

* All four parties (User, Provider, Router, and Compute Contract) have unique MOR address as their identity. All messages between the parties require signatures (but most don’t require blockchain txs)

* Providers must have non-zero balance to discourage Sybil attack from Provider side.

* If [milisecond] criteria is higher, network will be generally faster, but discourages smaller Providers

* There is a disincentive to provide slow Results (no revenue after computation)
  
* Centrally hosted Router to start is probably fine (decentralize Router eventually (IPFS? Or PoS node consortium?))
