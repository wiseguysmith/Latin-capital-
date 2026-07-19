---
provenance: capitalYA (originally "TITANO") Detailed Technical Specifications v1.0, July 2026 — founder-supplied input
status: ARCHIVED SOURCE — Horizon-2 (tokenization) technical ambition, NOT the MVP architecture
superseded_by: The MVP architecture is ../02_INTEGRATED_ARCHITECTURE.md. On-chain components here are DEFERRED and gated by ../03_REGULATORY_STRATEGY_COSTA_RICA.md.
name_note: The brand is now "capitalYA". "TITANO" is retired; retained below only as the original text.
---

# SOURCE — capitalYA Technical Specifications v1.0 (archived)

> **Horizon-2 material.** None of the on-chain architecture below ships in the MVP. See how each component maps (and is gated) in `../02_INTEGRATED_ARCHITECTURE.md` §9. The regulatory corrections in `../03_...` (no public token, no DEX, no on-chain PII, no automated approval) override anything here.

TITANO PROTOCOL — DETAILED TECHNICAL SPECIFICATIONS
v1.0 | July 2026
SECTION 1: SMART CONTRACT ARCHITECTURE
1.1 Design Philosophy
TITANO's smart contract architecture follows three core principles:
Modularity via EIP-2535 (Diamond Pattern): Each financial product (invoice factoring, term loans, mortgages) is a separate "facet" that can be upgraded independently without migrating user funds or pausing the protocol.
Compliance by Default: All tokens use ERC-3643 (T-REX standard) which embeds identity verification, transfer restrictions, and regulatory compliance at the token level — not as an afterthought.
Oracle-Agnostic Resilience: No single oracle controls protocol state. Multi-source aggregation with manual override capabilities for edge cases.
1.2 Contract Hierarchy
Core Contracts
Table
Contract	Standard	Purpose	Key Functions
TITANODiamond	EIP-2535	Proxy + dispatcher	delegatecall routing to facets
InvoiceTokenFacet	ERC-3643	Security token for invoices	mint(), burn(), forcedTransfer()
LoanVaultFacet	ERC-4626	Yield-bearing vault	deposit(), withdraw(), accrueYield()
RiskEngineFacet	Custom	Credit scoring integration	validateInvoice(), scoreBorrower(), verifyZKProof()
LiquidationManagerFacet	Custom	Default handling	initiateLiquidation(), auctionCollateral(), distributeRecovery()
GovernanceFacet	Custom	DAO voting	propose(), vote(), execute(), emergencyPause()
InsurancePoolFacet	Custom	Default coverage	stake(), claim(), accrueRewards()
Diamond Facet Structure
plain
TITANODiamond (Proxy)
├── DiamondCutFacet      → Handles facet upgrades
├── DiamondLoupeFacet    → Introspection (EIP-2535 required)
├── OwnershipFacet       → Access control (DEFAULT_ADMIN_ROLE)
├── InvoiceTokenFacet    → ERC-3643 token operations
├── LoanVaultFacet       → ERC-4626 vault operations  
├── RiskEngineFacet      → Oracle + scoring logic
├── LiquidationManagerFacet → Default + auction logic
├── GovernanceFacet      → DAO voting + parameter changes
├── InsurancePoolFacet   → Staking + coverage payouts
└── PausableFacet        → Emergency pause (3-of-5 multi-sig)
1.3 Key Contract Specifications
InvoiceTokenFacet (ERC-3643)
Why ERC-3643 over ERC-20?
ERC-3643 (T-REX) is a security token standard with built-in compliance: whitelisting, transfer restrictions, identity verification, and recovery mechanisms. Required for tokenized securities in most jurisdictions including Costa Rica.
Core Functions:
solidity
function mint(address to, uint256 amount, bytes calldata complianceData) 
    external onlyRole(UNDERWRITER_ROLE);
function burn(uint256 tokenId) 
    external onlyRole(UNDERWRITER_ROLE);
function forcedTransfer(address from, address to, uint256 amount) 
    external onlyRole(DEFAULT_ADMIN_ROLE);  // For legal enforcement
function isWhitelisted(address account) 
    external view returns (bool);  // KYC/AML check before any transfer
Storage Layout:
mapping(uint256 => InvoiceData) public invoices;
struct InvoiceData { uint256 faceValue; uint256 purchasePrice; uint256 maturityDate; address debtor; bytes32 invoiceHash; InvoiceStatus status; }
mapping(address => bool) public whitelistedInvestors; // KYC verified
LoanVaultFacet (ERC-4626)
Why ERC-4626?
Standardized yield-bearing vault interface. Any DeFi protocol can integrate TITANO vaults without custom adapters. Composability is critical for liquidity.
Core Functions:
solidity
function deposit(uint256 assets, address receiver) 
    external returns (uint256 shares);  // Investor deposits USDC, receives vault shares
function withdraw(uint256 assets, address receiver, address owner) 
    external returns (uint256 shares);  // Redeem shares for USDC + accrued yield
function accrueYield() 
    external;  // Called by keeper bot every block to update yield accrual
function totalAssets() 
    public view override returns (uint256);  // Total USDC under management + accrued yield
Yield Accrual Math:
Yield accrues block-by-block using block.timestamp
yieldPerShare = (faceValue - purchasePrice) / (maturityDate - purchaseDate) / totalShares
Investors can withdraw early at a discount (secondary market) or hold to maturity for full yield
RiskEngineFacet
Core Functions:
solidity
function validateInvoice(
    bytes32 invoiceHash,
    address debtor,
    uint256 amount,
    uint256 dueDate,
    bytes calldata oracleSignatures
) external returns (ValidationResult result);
function scoreBorrower(
    address borrower,
    bytes calldata zkProof,
    bytes calldata offChainData
) external returns (uint256 score, RiskTier tier);
function verifyZKProof(
    bytes calldata proof,
    bytes calldata publicInputs
) external view returns (bool valid);
Oracle Aggregation:
Primary: Chainlink Price Feeds (for USD/CRC rates)
Secondary: Custom API oracles (ATV invoice verification, bank confirmation)
Tertiary: Manual override by Risk Committee for edge cases
Consensus: 2-of-3 oracle agreement required for state changes
LiquidationManagerFacet
Default Triggers:
Invoice Default: Debtor fails to pay by maturity date + 7-day grace period
Loan Default: Borrower misses 2 consecutive payments
Oracle Failure: Invoice verification fails post-purchase (fraud detected)
Liquidation Flow:
plain
1. RiskEngine.triggerDefault(uint256 tokenId)
2. InsurancePool.checkCoverage(uint256 tokenId) → payout if covered
3. LiquidationManager.initiateAuction(uint256 tokenId, uint256 reservePrice)
4. Investors bid on distressed asset (Dutch auction, price decays over 48 hours)
5. Highest bid distributed to original investors pro-rata
6. Remaining loss absorbed by Insurance Pool (if any)
1.4 Transaction Flow: Invoice Factoring (Detailed)
Step 1: SME Submits Invoice
SME connects wallet (KYC verified via IdentityRegistry)
Uploads PDF invoice + metadata (debtor name, amount, due date, invoice number)
Signs transaction with private key
Invoice hash stored on-chain: keccak256(pdfBytes + metadata)
Step 2: RiskEngine Validates
Queries ATV (Costa Rican tax authority) API via oracle:
Is invoice number valid and registered?
Does debtor exist and have valid tax ID?
Is invoice amount consistent with debtor's declared revenue?
Queries bank oracle:
Does SME's bank account match registered business?
Has debtor paid past invoices on time?
Credit bureau check on debtor (if available)
Validation result: APPROVED, REJECTED, or MANUAL_REVIEW
Step 3: Smart Contract Mints Token
If approved, InvoiceTokenFacet.mint() creates ERC-3643 token
Token metadata:
faceValue: $10,000
purchasePrice: $9,700 (3% discount = ~12% annualized for 90 days)
maturityDate: 90 days from now
debtor: [debtor wallet or bank account]
yieldAccrualRate: per-second rate calculated from discount
Step 4: Investors Purchase Tokens
Investors deposit USDC into LoanVaultFacet
Vault allocates USDC to purchase invoice tokens
Investors receive vault shares proportional to deposit
Invoice token transferred to vault contract as collateral
Step 5: Debtor Pays Invoice
Debtor pays invoice via bank transfer to SME's account
Bank oracle detects payment via API webhook
Oracle submits proof to RiskEngineFacet
Smart contract verifies: payment amount ≥ faceValue, payer matches debtor
Step 6: Automated Settlement
On payment confirmation:
faceValue USDC transferred from SME to vault
purchasePrice returned to vault (investor principal)
yield distributed to investors pro-rata
protocolFee (2% of yield) sent to Treasury
Invoice token burned
If debtor pays early: investors receive pro-rata yield up to payment date
1.5 Security Patterns
Table
Pattern	Implementation	Rationale
ReentrancyGuard	OpenZeppelin nonReentrant modifier on all external payable functions	Prevents recursive call attacks (e.g., The DAO hack)
Access Control	OpenZeppelin AccessControl with roles: DEFAULT_ADMIN, UNDERWRITER, ORACLE, PAUSER, KEEPER	Principle of least privilege
Pause Mechanism	Pausable with 3-of-5 multi-sig (CEO, CTO, Legal, External Security, Community Rep)	Emergency stop without single point of failure
Upgrade Proxy	EIP-2535 Diamond pattern	Upgrade individual facets without migrating state or funds
Flash Loan Protection	block.number validation on price-sensitive operations	Prevents atomic manipulation attacks
Oracle Manipulation Defense	Multi-source aggregation + TWAP (Time-Weighted Average Price) + manual override	No single oracle can manipulate protocol state
Integer Overflow	Solidity 0.8.x built-in overflow checks + SafeMath for older patterns	Prevents arithmetic exploits
1.6 Audit Surface
Phase 1 Audit Scope:
Internal Audit: Foundry fuzz testing (100,000+ random inputs per function)
External Audit #1: Trail of Bits or OpenZeppelin (4-6 week engagement, $80-120K)
External Audit #2: Secondary firm for fresh eyes (e.g., CertiK, Hacken)
Bug Bounty: Immunefi program ($50K-$500K rewards based on severity)
Formal Verification: Certora for critical invariants (e.g., "totalAssets ≥ totalShares * sharePrice")
SECTION 2: REGULATORY STRATEGY — COSTA RICA
2.1 Legal Entity Structure
Dual-Entity Architecture
Table
Entity	Jurisdiction	Purpose	Regulatory Status
TITANO CR S.A.	Costa Rica	Operating company	SUGEVAL-regulated securities issuer; BCCR payment institution license applicant
TITANO Foundation	Cayman Islands	Protocol governance & token issuance	Non-profit foundation; DAO treasury management
Why two entities?
TITANO CR S.A. handles all Costa Rican operations, KYC/AML, local compliance, and holds the lending license. This is the regulated entity that regulators can inspect, fine, or shut down if necessary — protecting the protocol's global infrastructure.
TITANO Foundation holds the protocol treasury, issues TITANO tokens, and manages DAO governance. Being offshore provides regulatory arbitrage for token issuance (avoiding Costa Rican securities classification for the governance token itself).
Service Agreement Between Entities:
Foundation licenses protocol IP to CR S.A. for 5% of protocol revenue
CR S.A. operates the protocol in Costa Rica under local regulations
Foundation cannot be forced to shut down by Costa Rican regulators (sovereign immunity of offshore entity)
2.2 Regulatory Bodies & Compliance Requirements
SUGEVAL (Superintendencia General de Valores)
Jurisdiction: Securities regulation, investor protection, market oversight
Key Questions for TITANO:
Are tokenized invoices "securities"?
Likely YES under Costa Rican Ley del Mercado de Valores (Law 7732)
Tokenized invoices represent a contractual right to future payment — meets the "investment contract" test
Mitigation: Register as a "small issuer" or seek exemption for private placements to accredited investors
Registration Requirements:
Issuer registration with SUGEVAL
Quarterly financial reporting
Annual audited financial statements
Disclosure of material risks to investors
Investor Protection:
Maximum 499 non-accredited investors per pool (private placement exemption)
Accredited investor verification (net worth > $1M or income > $200K/year)
Cooling-off period (5 days) for retail investors
BCCR (Banco Central de Costa Rica)
Jurisdiction: Payment systems, monetary policy, financial stability
Key Requirements:
Payment Institution License (Licencia de Institución de Dinero Electrónico)
Required if TITANO holds client funds > 30 days
Minimum capital: ~$150K CRC equivalent
AML/CFT compliance program
Quarterly reporting to BCCR
Stablecoin Issuance:
If TITANO issues a CRC-backed stablecoin, BCCR must approve
Alternative: Partner with existing licensed entity (e.g., BN Servicios, BAC Credomatic)
FX Controls:
Costa Rica has mild capital controls
USD-denominated pools require BCCR notification
No restrictions on crypto-to-crypto transactions
SUPEN (Superintendencia de Pensiones)
Jurisdiction: Pension funds, institutional investors
Opportunity:
Costa Rican pension funds (OPCs) manage ~$15B in assets
They are required to diversify and seek yield
Currently limited to government bonds (6-8% yield) and corporate bonds (8-10%)
TITANO opportunity: Offer invoice factoring pools at 10-12% yield as "alternative fixed income"
Requirements:
SUPEN approval for pension fund investment in alternative assets
Minimum credit rating (equivalent to BB- or higher)
Quarterly reporting on pool performance
Maximum allocation: 5% of pension fund assets in alternatives
ATV / UAF (Autoridad Reguladora de Servicios Públicos / Unidad de Análisis Financiero)
Jurisdiction: Tax compliance, anti-money laundering
KYC/AML Framework:
Customer Identification: Costa Rican cédula (physical + digital) + biometric verification
Beneficial Ownership: For SME borrowers, identify ultimate beneficial owners (UBOs)
Transaction Monitoring: Automated screening against OFAC, UN, EU sanctions lists
Suspicious Activity Reports (SARs): File with UAF within 48 hours of detection
Record Keeping: 5-year retention of all KYC and transaction data
2.3 Compliance Roadmap (Detailed)
Q1 2026: Foundation
Incorporate TITANO CR S.A. (Sociedad Anónima) in Costa Rica
Incorporate TITANO Foundation in Cayman Islands (Foundation Companies Law, 2017)
Engage Costa Rican law firm (BLP, Facio & Cañas, or Pérez Virgilio) for regulatory opinion
Engage Cayman counsel (Maples and Calder) for Foundation structuring
Q2 2026: Regulatory Engagement
SUGEVAL: Informal meeting to present concept. Request guidance on:
Classification of tokenized invoices
Sandbox eligibility
Registration pathway for small issuer
BCCR: Discuss payment institution license requirements
SUPEN: Preliminary discussion on pension fund eligibility
Deliverable: Legal opinion on regulatory classification and compliance path
Q3 2026: Sandbox Application
Apply to SUGEVAL's regulatory sandbox (if available) or BCCR's fintech sandbox
Submit:
Business plan and financial projections
Technology architecture documentation
Risk management framework
AML/CFT compliance manual
Consumer protection plan
Deliverable: Sandbox approval for $500K pilot with regulatory oversight
Q4 2026: Pilot Operation
Operate $500K invoice factoring pool under sandbox
Monthly reporting to SUGEVAL/BCCR
Quarterly audit by external firm
Success Criteria: Zero regulatory violations, <2% default rate, 100% KYC compliance
Q1 2027: Full Licensing
Apply for full payment institution license (BCCR)
Apply for securities issuer registration (SUGEVAL)
Submit SUPEN application for pension fund eligibility
Deliverable: Full operating licenses
Q2 2027: Commercial Launch
Launch at scale with regulatory compliance audit
Annual audit by Big 4 firm (Deloitte, PwC, EY, KPMG)
Ongoing: Quarterly regulatory reporting, annual license renewal
2.4 KYC/AML Technical Implementation
Tiered KYC Framework
Table
Tier	Requirement	Limit	Products
Tier 0	Email + wallet connection	$0 (view only)	Browse pools, view rates
Tier 1	Cédula verification + selfie	$1,000/year	Invest in invoice pools
Tier 2	Tier 1 + proof of address + bank statement	$50,000/year	SME term loans, larger investments
Tier 3	Tier 2 + accredited investor verification	Unlimited	All products, governance voting
Tier SME	Business registration + ATV tax ID + UBO identification	$500,000/year	Borrow as SME
Technical Stack
Identity Verification: Sumsub, Onfido, or Jumio (support Costa Rican cédula)
Biometric Matching: Liveness detection + facial recognition
Sanctions Screening: Chainalysis KYT + ComplyAdvantage
Document Storage: Encrypted off-chain (IPFS with encryption) — NOT on-chain
ZK-Proof Identity: Polygon ID or World ID for privacy-preserving verification
SECTION 3: CREDIT SCORING ENGINE
3.1 Data Input Pipeline
On-Chain Data Sources
Table
Data Point	Source	Weight	Privacy
Wallet age	Ethereum/Polygon block explorer	5%	Public
Transaction history	On-chain analysis (Nansen-style)	10%	Pseudonymous
DeFi repayments	Aave, Compound, Maker subgraphs	15%	Pseudonymous
Token holdings	ERC-20 balance snapshots	5%	Public
Gas payment consistency	Transaction frequency analysis	5%	Public
TITANO repayment history	Internal protocol data	20%	Pseudonymous
Off-Chain Data Sources
Table
Data Point	Source	Weight	Integration
Credit bureau score	TransUnion CR, Equifax	25%	API integration
Bank account statements	BAC, BCR, BN (with consent)	15%	Open Banking APIs
Tax returns	ATV (Autoridad Reguladora)	10%	Government API
Utility payment history	ICE (electricity), AyA (water)	5%	Utility APIs
Employment verification	Employer API or manual	5%	HR platform integration
Alternative Data Sources
Table
Data Point	Source	Weight	Rationale
Sinpe Móvil history	SINPE transaction logs	10%	90%+ of Costa Ricans use it; payment behavior is predictive
E-commerce transactions	Mercado Libre, Amazon CR	5%	Spending patterns indicate financial health
Social graph	Optional, with consent	3%	Network effects on creditworthiness
Geolocation stability	Mobile device data	2%	Frequent address changes = higher risk
Device fingerprinting	Browser/device metadata	2%	Fraud detection
Business Data (For SME Borrowers)
Table
Data Point	Source	Weight
Invoice payment history	Internal protocol data	20%
Purchase order fulfillment rate	Oracle verification	15%
Revenue trends (6-24 months)	Bank statements + tax returns	20%
Debtor concentration	Invoice analysis	10%
Industry risk classification	SIC/NAICS codes	5%
Years in business	ATV registration	5%
3.2 Scoring Model Architecture
Ensemble Model Design
Final Score = Weighted Average of Three Sub-Models:
Table
Sub-Model	Algorithm	Weight	Strengths
Traditional Credit Model	Logistic Regression	40%	Interpretable, regulator-friendly, proven on bureau data
Behavioral Model	XGBoost	35%	Handles non-linear relationships, high predictive power on alternative data
On-Chain Heuristics	Rule-based + LightGBM	25%	Captures DeFi-native behavior, wallet maturity, transaction patterns
Feature Engineering
Traditional Features (Logistic Regression):
Debt-to-income ratio
Credit utilization
Payment history (delinquencies in last 24 months)
Length of credit history
Number of hard inquiries
Behavioral Features (XGBoost):
Income volatility (coefficient of variation in monthly deposits)
Spending-to-income ratio
Emergency fund ratio (savings / monthly expenses)
Seasonal revenue patterns (for SMEs)
Invoice payment velocity (how quickly debtors pay)
On-Chain Features (LightGBM):
Wallet age (days since first transaction)
Transaction count (normalized by age)
Average transaction value
DeFi protocol diversity (number of protocols used)
Liquidation history (has wallet been liquidated?)
Gas price tolerance (willingness to pay for fast transactions = liquidity indicator)
3.3 ZK-Proof Integration
Problem: Borrowers don't want to expose sensitive financial data (bank statements, tax returns) on a public blockchain.
Solution: Zero-knowledge proofs allow borrowers to prove creditworthiness without revealing underlying data.
Implementation: Polygon ID + Custom Circuits
Circuit Design:
plain
Public Inputs:
  - min_credit_score_threshold (e.g., 650)
  - max_debt_to_income_threshold (e.g., 0.4)
  - borrower_commitment (hash of private data)
Private Inputs:
  - credit_score (from TransUnion API)
  - monthly_income
  - monthly_debt_payments
  - bank_statement_hash
Constraints:
  1. credit_score >= min_credit_score_threshold
  2. monthly_debt_payments / monthly_income <= max_debt_to_income_threshold
  3. bank_statement_hash matches verified document
Proof Output:
  - isEligible: boolean
  - riskTier: enum (S, A, B, C)
  - proofHash: bytes32 (verifiable on-chain)
Flow:
Borrower downloads TITANO app, connects wallet
App fetches credit data from TransUnion (with borrower consent)
App generates ZK-proof locally (client-side computation)
Proof submitted to RiskEngineFacet.verifyZKProof()
Smart contract verifies proof without seeing raw data
Borrower assigned risk tier and loan terms
3.4 Risk Tier Output & Dynamic Pricing
Tier Classification
Table
Tier	Score Range	Borrower Rate	Expected Default	Max Loan	Investor Pool	Collateral Requirement
Tier S (Prime)	750-850	8-10%	0.5%	$500K	Institutional only	None (unsecured)
Tier A (Near-Prime)	650-749	11-14%	2.0%	$200K	Mixed retail/institutional	0-25%
Tier B (Sub-Prime)	550-649	15-20%	5.0%	$50K	Retail + insurance required	25-50%
Tier C (Deep Sub-Prime)	300-549	21-30%	12.0%	$10K	High-yield retail only	50-100%
Tier D (Declined)	<300	N/A	>20%	$0	N/A	N/A
Dynamic Pricing Formula
plain
borrowerRate = baseRate + riskPremium + liquidityPremium + currencyPremium
Where:
  baseRate = 10-year Costa Rican government bond yield (currently ~6.5%)
  riskPremium = f(creditScore, debtToIncome, industryRisk)  // 0% to 20%
  liquidityPremium = f(poolUtilization)  // 0% to 3%, increases as pool fills
  currencyPremium = f(CRC/USD volatility)  // 0% to 2%, for CRC-denominated loans
investorYield = borrowerRate - protocolSpread - insuranceFee
Where:
  protocolSpread = 1-2% (protocol revenue)
  insuranceFee = 0.5-1% (optional default coverage)
3.5 Model Governance & Updates
Problem: Credit models degrade over time (concept drift). A model trained on 2024 data may fail in 2026.
Solution:
Quarterly Retraining: Retrain XGBoost/LightGBM models on new data every quarter
A/B Testing: Roll out new model to 10% of borrowers, compare default rates vs. old model
Human-in-the-Loop: Risk Committee reviews all Tier C loans and any loan >$50K
Regulatory Audit: Annual third-party audit of model fairness (no bias against protected classes)
DAO Governance: Major model changes require TITANO token holder vote
3.6 Fraud Detection Layer
Table
Fraud Type	Detection Method	Prevention
Synthetic Identity	Cross-reference cédula with ATV database + biometric liveness	Reject if mismatch
Invoice Fraud	ATV API verification + debtor confirmation + PDF metadata analysis	Multi-oracle verification
Money Laundering	Transaction pattern analysis + UAF sanctions screening	SAR filing, account freeze
Collusion	Graph analysis of borrower-investor relationships	Flag circular financing
Oracle Manipulation	Multi-source aggregation + anomaly detection	Reject outlier data points
sandbox:///mnt/agents/output/titano_detailed_specs.png
This completes the full technical specification for all three critical systems. Each section is designed to be handed directly to:
Smart Contract Architecture → Blockchain engineering team
Regulatory Strategy → Legal counsel and compliance officers
Credit Scoring Engine → Data science and risk management teams
