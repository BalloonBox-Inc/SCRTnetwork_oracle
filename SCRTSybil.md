# Team Name: BalloonBox

## Project Description 
Project name: SCRTSybil

SCRTSybil is an oracle for credit score checks. The objective of SCRTSybil is to return a quantifiable, yet private and encrypted credit score for the SCRT network community ranking users by credibility and trustworthiness within the ecosystem. Credit checks will incentivize the activity of new, yet trusted and verified SCRT network users, while also affirming the reputation of long-standing users. Our Sybil is a dapp offering a credit scoring service that bridges web3 and web2 applications. Users will need to authenticate themselves via a Keplr wallet to access the SCRTSybil app to start the scoring process. After choosing their web2 (Apps like Plaid, Coinbase)/web3 (dApps like MetaMask) verifiers, their score is computed and sent to a smart contract generator which packages their score alongside some transactional information used to dervice the score to be encrypted and published on the blockchain. Thus, users' profiles are stored on-chain via Secret contracts. It is up to the user to choose whether they want their score to be fully or partially encrypted. After obtaining their score, the user can issue keys to third parties allowing them to 'interact' with the smart contract to view their score. After obtaining their credit score, the user chooses whether or not to endorse third parties to view their score or 'interact' with their smart contract. As such, SCRTSybil gives users control over their own data. 
 
## Problem / Solution  
How can one validate the trustworthiness of blockchain users, while maintaining user privacy? Our project focuses on building a SCRT oracle, called SCRTSybil, for credit scoring, using web2/web3 apps for acquiring the data to calculate a score and Web3 (SCRT network) for storing, encrypting, and making the score available to the world.  We will build TLS connections from on-chain validator enclaves to APIs, creating private credit check scores for secret contracts. These APIs will aggregate a user’s credit data from web3 (wallet balance and transaction history on a cryptocurrency exchange like Coinbase) with web2 data (bank account balance, credit history, recent activity, liquidity, etc. via Plaid). The credit check outcome will be quantified via a credit scoring algorithm which we will build based on an initially simple criteria. This algorithm, which will run off-chain, integrates web2 and web3 validation of a user’s financial standing and history. Validated private data feed, i.e., the actual credit score will be stored on-chain and can be accessed via a privately issued SCRT key, generated and distributed by the credit score holder. 

## Detailed Product Description 

SCRTSybil Oracle Model
![SCRTSybil Oracle Model](https://github.com/BalloonBox-Inc/SCRTnetwork_oracle/blob/main/images/SCRTSybil_oracle.png)

SCRTSybil Architecture
![SCRTSybil Architecture](https://github.com/BalloonBox-Inc/SCRTnetwork_analytics/blob/main/images/SCRTSybil_architecture.png)



#### Components

1. **GUI**: user logs into SCRTSybil web application by authenticating with an existing Keplr wallet. We selected Keplr because it's the first and leading wallet enabled for inter-Blockchain Communication in the Cosmos ecosystem, it is widely used, and it offers a convenient Google Chrome plugin. A user should therefore own a Kepler wallet + SCRT public address with some minimum balance (TBD) in order to proceed in calculating a credit score for themselves. The core purpose of the web app, is to provide users with a rapid (< 3 minutes) experience, guided by a wizard, whereby they can authenticate with the SCRTSybil-integrated 3rd parties e.g. Plaid, which then computes their credit score. The credit score algorithm, coded in Python, will be executed off-chain and then will be compiled into a smart contract (Rust) to be written on-chain. If a user wants to issue multiple credit scores at different times, then we will interact with the contract initialized at the start to append the new score to pre-existing ones.

2. **Restful API**: Python (Flask) + webhook framework with public APIs that can be used. The SCRTSybil API is the only offchain component of this project and used for score computation. This offers the community an easy-to-use library for tweaking the scoring logic or using different py libraries quite easily. It furthermore enables used of ML libraries within the scoring logic for adding a predictive component to scoring.

3. **Credit Score Module**: Credit score algorithm integrating at least these 5 weighted metrics across 2 web2 oracles. Metrics were adapted from the factors used by the three largest American consumer credit reporting agencies: Equifax, Experian, and TransUnion and self-developed weightings approporaite to the use case. The core metrics include:

* Transaction History (txn counts + txn volume)
* Number and types of bank accounts/wallets owned
* Ratio of used liquidity / available liquidity for each account
* Length of account history
* Recent (last 21 days) account activity

Both a score matrix and a machine learning module will be built with SciKit-Learn Python library + Flask for web framework. The output returns two fields: a numerical score on 300-900 non-linear numerical scale and its associated qualitative quantifier: Excellent/Good/Fair/Below average/Poor.

4. **Smart Contract Compiler**: with the support of CosmWasm or Secret's adapted compiler, this compiler will process statements written in Python and turn them into smart contracts in Rust. We see this as a potentially super reproducible open-source micro project to help bridge the gap for non-Rust developers in producing low-code smart contracts on SCRT. The encrypted data we'll write to the SCRT blockchain via smart contract includes numerical score, quantitative score, public Key of Kelpr wallet, timestamp, validators used to compute the score (e.g., Plaid and Coinbase), and the data used to compute the score. Since we want to leverage the privacy-preserving uniqueness of the SCRT network, we aim to store users' data on-chain encrypting it in a secret contract. No user data will be stored offchain. Local storage will only be used for score computation with the raw data + score entirely written to the blockchain via smart contract. This eliminates the need for an off-chain DB, which would require maintenance and would challenge data privacy and ownership of the user.

Below is a high level design explaining smart contract compilation and interaction:

![SCRTSybil smartcontract](https://github.com/BalloonBox-Inc/SCRTnetwork_oracle/blob/main/images/contractInteraction.png)

 
## Go-to-Market plan
We are building a credit scoring tool on SCRT as base-layer infrastructure for a series of use cases where individuals or businesses may verify potential customers for extending credit, P2P lending, or vetting a potential employee with financial background checks.
The go-to-market plan is two-fold:

1. Transaction fees & Request Fees. Users requesting to view a person’s credit score will need to be issued with a key to access that user’s smart contract stored on SCRT. This will require a fraction of SCRT as a transaction fee for exercising that key on the smart contract. For companies requesting scores for people, we can make the model comparable with the current $10 fee for an Equifax report in Canada.

2. Self-run credit scores. If an individual wants to know their own credit score and how they rank on the SCRT network in terms of ‘credibility’, we will charge a fee in SCRT (less than the fee to a score requester as above) for computing the score. This will be a fee that the user pays in-wallet to SCRTSybil, however, the score is not necessarily published as a smart contract. 

Our core goal is to initially provide tools for building financial ecosystems on SCRT as the network gains traffic and user adoption. We believe in the value of having a public network with the option and flexibility of data encryption and ultimately believe that mainstream tools for financial services like credit scores will be of the first major requirements for service providers on SCRT.  
 
## Value capture for Secret Network ecosystem
A credit check oracle is both a clear value proposition for existing SCRT holders and an incentive for new or prospective users. First, a SCRT credit check oracle will increase Secret Network usability by validating users’ reputation (measured in terms of a credit check), and thus will create trust toward users who have been verified by the oracle. Secondly, the oracle promotes interoperability and accessibility for the Secret Network by connecting the chain to crypto exchange platforms. Thirdly, this oracle constitutes a credit scoring service to web2 users, specifically those users who are still unaware of the potential of the SCRT blockchain, yet under their need to use the SCRTSybil oracle will now open a Keplr wallet. Fourthly, the Sybil has the potential to expand from the SCRT community to the vast Ethereum community as well. Although the Sybil is built on SCRT network infrastructure, realistically it can provide credit score checks to any web3 user, because our credit scoring module fetches data from an exchange like Coinbase (which supports trading of over 100 cryptocurrencies) and thus we are capable to generate credit scores to ETH users too. Lastly, SCRTSybil enriches the SCRT ecosystem by leveraging on the privacy-preserving smart contracts which are distinctive of the Secret network. We believe the community needs a credit check oracle as soon as possible. SCRTSybil aims to provide that. 
 
## Team Members
* Michael Brink
* Matteo Mortelliti
* Yuchen Lin
* Mayllon Miranda
* Irene Fabris
* Hongbin Lin
* Yoon Kim

## Team Website	
* https://www.balloonbox.io/
 
## Team’s experience
All team members of SCRTSybil are from Balloon Box Technology Inc., a FinTech company based in Vancouver, BC specialized in software engineering & data science solutions from design through launch. Besides a string of projects for clients all around the world, our team is experienced in developing web apps for crypto asset management, custom Ledger-integrated apps, integrating with PSPs, crpyto exchanges and KYC providers, visualizing big data on L1 & L2s and building wallet solutions. Our team members are proficient in Python, C++, Java, Javascript, and other object-oriented and development languages. 
 
Our team consists of the following roles:
- Product Architect (Full product architecture) 
- Full Stack Engineer (Core framework)
- Designer (UI/UX on the web application)
- Backend Engineer (APIs, database structure, DevOps)
- Data Scientist (x2) (Credit Score algorithm + build)
- Product Manager (Community, articles, GitHub house-keeping, managing public PRs)
 
## Team Code Repos
- https://github.com/BalloonBox-Inc BalloonBox official repository
- https://github.com/MichaelBrink Michael Brink
- https://github.com/mattdm3 Matteo Mortelliti
- https://github.com/yul761 Yuchen Lin
- https://github.com/m3mayllon Mayllon Miranda
- https://github.com/irene-bbox Irene Fabris
- https://github.com/BalloonBox-Hongbin Hongbin Lin 
- https://github.com/yoon-bbox Yoon Kim
 
## Team LinkedIn Profiles
* https://www.linkedin.com/in/michael-brink-680b3767/ Michael Brink
* https://www.linkedin.com/in/matteo-mortelliti/ Matteo Mortelliti
* https://www.linkedin.com/in/foggysalmon/ Yuchen Lin
* https://www.linkedin.com/in/mayllon-miranda-8aa9b0163/ Mayllon Miranda
* https://www.linkedin.com/in/irenefabris/ Irene Fabris
* https://www.linkedin.com/in/hongbinlin/ Hongbin Lin
* https://www.linkedin.com/in/imyoonkim/ Yoon Kim
 
## Development Roadmap :nut_and_bolt:
### Overview

- **Total Estimated Duration:** 16 weeks
- **Full-Time Equivalent (FTE):** 3.5
- **Total Costs:** 125,000 USD


Our team will require ~4 months to complete this project (subject to a mutually agreed upon solution design that both Bbox and SCRT feel excited about). We intend to have 2 developers full-time, 2 data scientists full-time, and 1 UX/UI designer part-time, a product architect, and a project manager. We can receive payments in 4 disbursements, one at the beginning of the grant, three after completion of each milestone.
We would be willing to consider part payment in SCRT, BTC or ETH, up to 50%. The grant can be paid to either of the following addresses:

- BTC address: bc1qz36smg7zg9d2de7qurj9a49fetemavta39a9zp
- ETH address: 0x3ffA0e66091742a867430D140E36826c06ba19Be
- SCRT address: secret18qeg7lx63wkqvy9e295slufvch5rfel2v23tsu

*Initial amount at grant issuance: 22,500 USD
 
### Milestone 1 - APIs, Integration, Credit Score Algorithm
- **Estimated duration:** Week 1-6
- **Costs:** 42,500 USD


| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| 0a. | License | Apache 2.0 |
| 0b. | Documentation | Provide inline documentation of the code. |
| 1. | API module | Build APIs to selected validators. For this initial grant, we will limit our validators to Coinbase & Plaid. | 
| 2. | Data Cleaning | Setup VM with sufficient resources for score computation. |
| 3. | Data Integration | Overlay web2 with web3 validation data for each API. |
| 4. | ML Module - Build | Build an light ML model to calculate user credit score. |
 
 
### Milestone 2 - Smart Contract Compiler
- **Estimated duration:** Week 7-12
- **Costs:** 42,500 USD

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| 0a. | License | Apache 2.0 |
| 0b. | Documentation | We will provide inline documentation of the code. |
| 0c. | Test Guide | Run unit tests to ensure functionality and robustness of core functions (~70%). |
| 1. | ML Module - Deploy | Train, test, deploy the ML model and compute credit scores. |
| 2. | Smart Contract Compiler | Use cosmwasm-std to build CosmWasm smart contract. |
| 3. | Write to chain | Execute the smart contract, written in Rust, to encrypt the calculated credit score to SCRT chain. |
| 4. | Integarte with secretnodes for fetching and displaying smart contracts belonging to a user public address
  
 
### Milestone 3 - WebApp: framework + UI 
- **Estimated duration:** Week 13-16
- **Costs:** 17,500 USD


| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| 0a. | License | Apache 2.0 |
| 0b. | Documentation | We will provide inline documentation of the code for the WebApp framework. |
| 1. | Framework | Implement SCRT wallet Login, personal info input, hover+select+OAuth2 in chosen validator |
| 2. | UI Flow1: issuer | Design the UI flow for issuing the credit score to a user. | 
| 3. | UI Flow2: lender | Design the UI flow for the third party requesting to view the score prior to lending service to the credit score issuer. | 
| 4. | UI Module | We will refine the WebApp aesthetics and functionality. |
| 5. | Tutorial | We will publish a video tutorial that walks a user through the SCRTSybil WebApp instructing them on how to get their credit score calculated. | 
 
 
## Additional Information :heavy_plus_sign:

#### Why SCRT

1. **How did you hear about the SCRT Network Grants?** We heard through the Secret Network Webpage.

2. **Have you ever applied for other grants?** Yes, we have applied for another grant (TBD) to fund an entirely different project proposal for a different ecosystem. We have since declined pursuing this grant (2021-12-18)

3. **Why did we apply for this grant?** We, at BalloonBox, are a team of engineers excited about the future of decentralized technologies, and we want to contribute to building it! We think that the SCRT network is rapidly and innovatively unlocking the full value of decentralized applications by leveraging the computational privacy of Trusted Execution Environments (TEEs) and secret contracts to design Secret Apps preserving users’ privacy. We value the novelty of privacy-preserving smart contracts and their scalability potential which make SCRT a network that is uniquely set apart from other ecosystems. Specifically, we are submitting a proposal under the ‘‘Ecosystem’’ category because we want to build tools to expand the Secret Network and improve general usability. We hope our Credit Score Check to be the first of many such tools.  
 
 
#### Scope and Limitations
We will develop a simple WebApp for users to retrieve user credit scores. Although we aim to scale up the WebApp for numerous and diverse use cases in the near future, for this first initial grant we will limit the App to three UIs: (1) user login into Secret Network wallet via Keplr; (2) enter user’s personal information; (3) choose web2/web3 validators and calculate your credit score. We mocked up three UI prototypes, one for each page respectively, to give you a clear idea of our vision. Please, find the UI prototypes [here](https://github.com/BalloonBox-Inc/SCRTnetwork_oracle/tree/main/UI%20prototypes).
Initially, the WebApp for the SCRTSybil Oracle will only allow user authentication to their Secret Network Wallet via Keplr. In the future, this can scale to support a range of wallets supporting SCRT (e.g., Ledger, Math Wallet, Citadel.One, Chain or Secrets, Secret Nodes).
 
 
#### Future Plans

1. **Image processing** 
   - Integrate image processing for KYC authentication, e.g., interpreting passports, driving license, medical service cards, etc.


2. **Auxiliary & Novel Use Cases**

   Developing an oracle for credit score checks automatically satisfies this auxiliary use case:

    - personal information integrity validation: do the user’s first and last name match across different platforms (Coinbase, Binance, Plaid, etc.)?
   
   Furthermore, we’ve designed our oracle to easily scale up to use cases that we consider relevant to the Secret Network:
   
    - credit score check for Anti-Money Laundering (AML)
   
    - credit score check to support loan application for university/post-secondary education (in such case validators should include a diverse set of eLearning Platforms, e.g., Coursera, LinkedIn Learning, Udacity, CloudAcademy, Udemy, DataCamp. An academic institution may want a credit score for a prospective student based on financial history but also academic history. For this use case, we could skew the algorithm weightings towards OAuth, where eLearning Platforms prove/disprove self-study courses.

    - third party (e.g., banks, financial institutions) request issuing of Secret private keys to validate user’s credibility 


3. **More Credit Check Providers**
   - Expand our validators pool (Coinbase, Binance, Stripe, Plaid) including more consolidated and emerging cryptocurrency exchanges (Kraken, crypto.com, Gemini, Bitstamp, MetaMask, etc.). For the moment, we will exclude decentralized exchanges like Bisq that do not require any KYC. We want to base our credit scoring algorithm to be anchored on platforms that validate the user identity.  


4. **Implement Credit Score Templates**
   - Imagine a third party requesting to issue a Secret private key to access a user’s credit score. Third parties may be Real and Personal Property Insurance Companies, Loan Agencies, Banks, Health Care Providers, and more. A bank will likely require a granular check on a user’s financial standing, while a Health Care Provider will require in-depth medical records for a patient. Similarly, an individual wanting to loan a friend money will value USD velocity in a US bank account far more than identity and this may require a credit model more skewed on historical throughput for a checking account. We want to design credit score templates with pre-set data granularities (i.e., more or fewer fields to be hashed out) depending on who is the third party requesting to access the credit score. In the long run, we envisage monetizing the smart contract generator to build credit score templates based on the needs of the third party. That is, the smart contract generator will serve as a template builder which we build privately for corporations or as community projects for user-level use cases.
