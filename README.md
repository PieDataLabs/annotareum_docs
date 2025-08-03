# annotareum_docs
Annotareum project docs

# Annotareum Whitepaper

**Version 1.0**  
**Last Updated:** August 2025

---

## 🧠 Abstract

**Annotareum** is a decentralized protocol for data annotation built on Ethereum. It enables data providers, annotators, and validators to collaborate on labeling datasets in a fully transparent and trustless way.

By combining smart contracts, programmable annotation logic, customizable interfaces, and economic incentives, Annotareum is building the infrastructure layer for high-quality AI datasets.

---

## 📚 Table of Contents

1. [Introduction](#introduction)  
2. [Participants](#participants)  
3. [Workflow](#workflow)  
4. [Architecture](#architecture)  
5. [Smart Contracts](#smart-contracts)  
6. [Economic Incentives](#economic-incentives)  
7. [Dispute Resolution](#dispute-resolution)  
8. [Reputation System](#reputation-system)  
9. [Go-to-Market Strategy](#go-to-market-strategy)  
10. [Roadmap](#roadmap)  
11. [Tokenomics and DAO](#tokenomics-and-dao)  
12. [Tech Stack](#tech-stack)  
13. [Team](#team)  
14. [Conclusion](#conclusion)  

---

## 🧩 Introduction

Traditional annotation platforms are centralized, opaque, and often misaligned with the interests of annotators and data owners. Annotareum introduces a Web3-native protocol where annotation work is open, verifiable, and governed by smart contracts.

Annotareum allows:  
- Project creators to define, price, and control annotation pipelines.  
- Annotators to earn fair rewards with guaranteed payments.  
- Validators to ensure quality and earn bonuses for catching errors.  
- All participants to interact via Ethereum wallets — without intermediaries.

---

## 👥 Participants

### 📦 Customers (Data Providers)  
- Upload datasets  
- Define interface and logic for annotation  
- Set prices for annotation and validation  
- Fund the smart contract with ETH or tokens  
- Control access via `permissions.txt`  

### ✍️ Annotators  
- View and complete annotation tasks  
- Use web interface (generated from `view.py`)  
- Receive automatic payments upon successful validation  

### 🔍 Validators  
- Review completed annotations  
- Accept or reject based on guidelines  
- Earn fixed fees + bonuses for accurate rejections  

### ⚖️ Arbiters (in disputes)  
- Vote on disputed annotations  
- Can be community members or assigned validators  
- Participate via DAO governance (future)  

---

## 🔄 Workflow

1. Customer sets up project:

```plaintext
project/
├── data/
├── permissions.txt
├── dataset.py
└── view.py
````

2. Annotators pick available tasks
3. Annotators submit annotations via `save_item()`
4. Validators review and approve/reject
5. Payments automatically distributed
6. If disputed, dispute contract is invoked
7. DAO or arbiters resolve the case

---

## 🏗 Architecture

### CLI & Dataset Structure

```python
# dataset.py
def get_item(idx):
    return {"image": f"./data/{idx}.jpg"}

def save_item(idx, data):
    # Save annotation data to disk or database
    pass
```

```python
# view.py
<View>
  <Image source="item.image" />
  <Choice choices="cat,dog,other" />
</View>
```

* `permissions.txt` lists Ethereum addresses allowed to annotate or validate.
* Data is served via web interface using the CLI: `annotareum serve project/`

---

## 🔐 Smart Contracts

Contracts are deployed on Ethereum L2 (Optimism/Base).

### Key Contracts

* **Escrow Contract**: Holds funds for each annotation/validation
* **Validator Contract**: Registers decisions and distributes fees
* **Dispute Contract**: Manages arbitration
* **DAO Registry**: (planned) governs protocol updates

### Events

All interactions emit events:

* `AnnotationSubmitted`
* `AnnotationValidated`
* `DisputeOpened`
* `DisputeResolved`

---

## 💰 Economic Incentives

| Case               | Annotator   | Validator        | Outcome            |
| ------------------ | ----------- | ---------------- | ------------------ |
| Valid annotation   | 100% reward | Verification fee | ✅ All paid         |
| Invalid annotation | -50% reward | +50% bonus       | ⚠️ Penalty applied |
| Dispute won        | 100% reward | 0%               | 🏆 Annotator wins  |
| Dispute lost       | 0%          | 100%             | ❌ Validator wins   |

* Annotation reward: Set by customer per item
* Validation reward: Also set by customer
* Penalty: 50% of annotation fee
* Dispute fee: Paid from escrow

---

## 🧾 Dispute Resolution

Disputes can be resolved in three ways:

1. **Direct Validator Arbitration** (3-of-N validators vote)
2. **DAO Voting** (via token governance)
3. **Staking-Based Courts** (validators stake tokens to vote)

Only one method is used per project depending on configuration.

---

## ⭐ Reputation System (Planned)

Reputation scores will influence:

* Access to premium tasks
* Higher reward multipliers
* Role in governance (validators/arbiters)

### Metrics

* Acceptance ratio
* Dispute win rate
* Completion speed
* Task diversity

Reputation will be stored on-chain or off-chain (zk-based private option planned).

---

## 📈 Go-to-Market Strategy

### 🎯 Target Audiences

* AI startups and ML teams
* Academic researchers and open dataset creators
* Freelance annotators (web3-savvy)
* Web3 DAOs needing labeled data
* Data-driven DeFi protocols

### 📣 Distribution Channels

* Twitter/X and Web3 Discord servers
* HackerNews and Reddit (r/MachineLearning, r/Ethereum)
* Kaggle forums and ML blogs
* Web3 and AI hackathons
* VC/accelerator community demos

### 🧲 Growth Loops

* Annotator referral program
* Public leaderboard and rewards
* Integration with HuggingFace datasets
* SDK for annotation-as-code (CI/CD style)

---

## 🚀 Roadmap

| Quarter | Milestone                                          |
| ------- | -------------------------------------------------- |
| Q3 2025 | ✅ MVP on Optimism/Base, CLI tooling, simple web UI |
| Q4 2025 | 🌐 IPFS/Arweave for data hosting, hosted dashboard |
| Q1 2026 | ⚖️ Dispute resolution & reputation system          |
| Q2 2026 | 🧠 AI-assisted annotation (LLM help)               |
| Q3 2026 | 🛒 Job marketplace, template library               |
| Q4 2026 | 🪙 Launch Annotareum DAO + Governance token        |

---

## 🪙 Tokenomics and DAO

### Planned Token: `$ANN`

Used for:

* DAO governance
* Validator staking
* Dispute resolution votes
* Reputation boosts

### Token Distribution (Proposed)

| Purpose              | %   |
| -------------------- | --- |
| Community Airdrops   | 20% |
| Ecosystem Incentives | 20% |
| Team & Advisors      | 20% |
| DAO Treasury         | 30% |
| Investors            | 10% |

### Governance Process

* Proposals submitted via DAO frontend
* Vote weights proportional to staked `$ANN`
* DAO can upgrade protocol parameters

---

## ⚙️ Tech Stack

| Layer       | Tools                             |
| ----------- | --------------------------------- |
| Blockchain  | Solidity, Optimism/Base           |
| Frontend    | React + DSL rendering             |
| CLI Tool    | Python (`pip install annotareum`) |
| Hosting     | IPFS, Arweave (future)            |
| Wallet Auth | EIP-191 / MetaMask                |
| Payments    | ETH, USDC                         |
| Interop     | REST API + Python SDK (Q4 2025)   |

---

## 🧑‍💼 Team

### Kaspar George — CEO, Co-founder

Web3 strategist and AI researcher. Leads vision, fundraising, partnerships.

### Artem Stanko — CTO, Co-founder

Smart contract and infrastructure developer. Creator of core Annotareum engine and CLI.

### Future Hires

* Frontend/Web3 Engineer
* Developer Advocate (AI + Crypto)
* Community Manager (DAO Growth)

---

## ✅ Conclusion

Annotareum reimagines the annotation pipeline as an open, permissionless, and economically fair protocol. It puts power back in the hands of data providers and workers, while ensuring quality through incentive alignment and on-chain transparency.

Join us to build a better future for AI — one label at a time.

---

## 📡 Links

* **GitHub:** [https://github.com/PieDataLabs/annotareum](https://github.com/annotareum)
* **Website:** [https://annotareum.io](https://annotareum.io)
* **Twitter/X:** [https://twitter.com/annotareum](https://twitter.com/annotareum)
* **Email:** [help@annotareum.io](mailto:hello@annotareum.io)
