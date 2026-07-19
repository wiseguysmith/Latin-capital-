---
provenance: capitalYA (originally "TITANO") PRD v1.0, July 2026 — founder-supplied input
status: ARCHIVED SOURCE — north-star ambition, NOT the build spec
superseded_by: On all near-term regulatory, entity, token, and licensing questions, this document is superseded by ../03_REGULATORY_STRATEGY_COSTA_RICA.md and reconciled in ../01_CAPITALYA_PROGRAM_BRIEF.md (§7, §8).
name_note: The brand is now "capitalYA". "TITANO" is retired; retained below only as the original text.
---

# SOURCE — capitalYA PRD v1.0 (archived)

> **Read `../01_CAPITALYA_PROGRAM_BRIEF.md` first.** This PRD is preserved for traceability and vision context. Do not build from it directly; the near-term plan is governed by the Regulatory Strategy (`../03_...`).

TITANO PROTOCOL
Hybrid RWA + Consumer Debt Protocol for Latin America
High-Level Product Requirements Document (PRD) — v1.0 | July 2026
1. EXECUTIVE SUMMARY
TITANO is a hybrid DeFi protocol designed to replace fragmented, expensive corporate financing and consumer lending in Latin America — starting with Costa Rica.
PHASE 1 (Months 1-12): Invoice Factoring & SME Trade Finance
Tokenize unpaid invoices & purchase orders into liquid, yield-bearing assets
Bootstrap protocol liquidity, build regulatory trust, prove underwriting model
PHASE 2 (Months 13-24): Consumer Debt (Personal Loans → Mortgages)
Leverage Phase 1 infrastructure to underwrite consumer credit
Target the $50B+ mortgage gap across Central America
MISSION: Democratize access to capital for Latin American SMEs and families while delivering superior, transparent yields to global investors.
2. PROBLEM STATEMENT
Table
Pain Point	Impact
BANKING OLIGOPOLY	4 banks control ~80% of Costa Rica's credit market. No competition = no innovation.
USURIOUS RATES	SMEs pay 18-25% for working capital. Consumers pay 30-45% on credit cards.
BROKEN TIMELINES	SME loans take 60-90 days to approve. Invoice payments delayed 60-180 days by large buyers.
EXCLUSION	Banks won't lend below $50K tickets profitably. SMEs and middle-class families are locked out.
CAPITAL TRAPPED	Local savings earn 2-4% in banks while global investors seek 8-12% yield in emerging markets.
3. SOLUTION ARCHITECTURE
plain
┌─────────────────────────────────────────────────────────────────────┐
│                    🔒  PROTOCOL CORE LAYER                           │
│  Identity/KYC | Credit Scoring Engine | Stablecoin Settlement      │
│  Governance/Risk Management | Insurance Pool                         │
└─────────────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌───────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  PHASE 1:     │    │   PHASE 2:      │    │   CROSS-OVER    │
│  CORPORATE    │    │   CONSUMER      │    │   PRODUCTS      │
│    RWA        │    │    DEBT         │    │                 │
│               │    │                 │    │ • SME Owner     │
│ • Invoice     │    │ • Personal      │    │   Personal      │
│   Factoring   │    │   Loans         │    │   Loans         │
│ • PO Finance  │    │ • Student       │    │ • Employee      │
│ • SME Term    │    │   Loans         │    │   Salary-Backed │
│   Loans       │    │ • Credit Card   │    │   Loans         │
│ • Supply      │    │   Refinancing   │    │ • Revenue-Based │
│   Chain       │    │ • Mortgage      │    │   Financing     │
│   Finance     │    │   Refi + Orig   │    │                 │
└───────────────┘    └─────────────────┘    └─────────────────┘
4. PRODUCT SPECIFICATIONS
PHASE 1: CORPORATE RWA
Table
Product	Ticket Size	Investor Yield	Term	Collateral
Invoice Factoring	$5K - $50K	10-14%	30-90 days	Invoice receivable
PO Financing	$20K - $200K	11-14%	60-180 days	Purchase order
SME Term Loans	$10K - $100K	12-16%	6-24 months	Business assets
Supply Chain Finance	$10K - $500K	9-12%	30-120 days	Confirmed receivable
PHASE 2: CONSUMER DEBT
Table
Product	Ticket Size	Borrower Rate	Term	Target
Personal Loans	$500 - $5K	15-18%	6-24 months	Salaried employees
Student Loans	$2K - $10K	12-15%	2-4 years	Private university students
Credit Card Refinancing	$1K - $8K	18-22%	12-36 months	High-APR card holders
Mortgage Refinancing	$50K - $200K	8-10%	15-30 years	Existing homeowners
Mortgage Origination	$50K - $150K	9-11%	15-30 years	First-time buyers
5. TECHNICAL ARCHITECTURE
SMART CONTRACT LAYER
Invoice Tokenization: ERC-3643 / ERC-1400 security token standard for compliance
Loan Origination Engine: Automated underwriting, term generation, disbursement
Amortization & Distribution: Automated coupon/principal payments to investors
Liquidation & Default Handling: Grace periods, penalty accrual, collateral seizure logic
ORACLE & VERIFICATION LAYER
Invoice Verification API: Integration with Costa Rican tax authority (ATV) for invoice validation
Bank Account Confirmation: APIs with BAC, BCR, BN for payment verification
Credit Bureau Integration: TransUnion CR, Equifax for credit scoring
ZK-Proof Identity: Cédula verification + biometric matching without exposing PII on-chain
STABLECOIN & SETTLEMENT
Primary Settlement: USDC / USDT for investor deposits and yield distribution
Local Stablecoin: CRC-backed stablecoin (partner with Banco Nacional or issue via over-collateralization)
Fiat On/Off Ramp: Sinpe Móvil integration for instant CRC deposits/withdrawals
Repayment Automation: ACH / SINPE recurring deductions for consumer loans
GOVERNANCE & RISK
DAO Governance: TITANO token holders vote on protocol parameters, fee structures, risk limits
Risk Committee: Elected underwriters, legal counsel, compliance officers with veto power
Insurance Pool: Protocol-native coverage fund (10% of token supply) for default protection
Emergency Pause: Multi-sig pause mechanism for smart contract upgrades and crisis response
6. GO-TO-MARKET & MILESTONES
Table
Phase	Timeline	Key Deliverables	Target Metric
BOOTSTRAP	M1-3	Legal entity (CR S.A. + Cayman Foundation), MVP contracts, 3 pilot SME clients, regulatory intros	$0 AUM
VALIDATE	M4-6	$500K pilot pool live, first institutional investor, invoice oracle integration	$500K AUM
SCALE CORP	M7-12	$5M Corporate RWA AUM, SME term loans + PO finance live, $1M institutional commitment, sandbox application	$5M AUM
CONSUMER LAUNCH	M13-18	Personal + student loans live, $2M consumer book, credit card refi, Sinpe full integration	$7M AUM
MORTGAGE DOMINANCE	M19-30	Mortgage refi live ($10M book), mortgage origination pilot, Panama & Guatemala expansion	$25M AUM, $2M revenue
REGIONAL EXPANSION	M31-48	Full mortgage origination, $100M AUM across Central America, DAO decentralized	$100M AUM, $10M+ revenue
7. TOKENOMICS & REVENUE MODEL
TITANO Token Distribution
Table
Allocation	%	Purpose
Community / Liquidity Mining	25%	Incentivize lenders, borrowers, liquidity providers
Team & Advisors	20%	4-year vesting, 1-year cliff
Investors (Seed + Series A)	20%	Institutional and angel backing
Treasury / Ecosystem	20%	Grants, partnerships, protocol development
Insurance Pool Reserve	10%	Default coverage, protocol stability
Public Sale	5%	Community allocation, fair launch
Protocol Revenue Streams
Table
Stream	Rate	Description
Origination Fees	2-3% of loan/invoice amount	Primary revenue driver — charged at disbursement
Servicing Fees	0.5-1% annually on outstanding balance	Recurring, predictable revenue
Interest Spread	1-2% of interest rate differential	Protocol keeps spread between borrower rate and investor yield
Secondary Market Fees	0.25-0.5% per trade	Grows as tokenized debt becomes liquid
Data/Analytics	SaaS subscription	Sell anonymized credit risk data to insurers, banks
Insurance Premiums	0.5-1% for default coverage	Protocol-native revenue from optional default insurance
Key Protocol Metrics (Year 3 Target)
AUM: $100M+
Active Loans: 5,000+
Default Rate: <3%
Investor APY: 10-14%
Protocol Revenue: $10M+
Users: 50,000+
Countries: 4 (Costa Rica, Panama, Guatemala, El Salvador)
8. RISK FRAMEWORK
Table
Risk Category	Mitigation
Smart Contract Risk	Multiple audits (Trail of Bits, OpenZeppelin), bug bounty program, insurance via Nexus Mutual
Default Risk	Diversified pools, over-collateralization where possible, insurance pool, credit scoring
Regulatory Risk	Proactive engagement with SUGEVAL/BCCR, legal sandbox participation, compliant token standards
Oracle Risk	Multi-source verification, manual override for edge cases, oracle staking/slashing
Currency Risk	USD-denominated pools for export-facing SMEs, CRC pools with hedging reserves
Liquidity Risk	Staggered maturities, secondary market incentives, protocol liquidity backstop
9. COMPETITIVE LANDSCAPE
Table
Competitor	Type	TITANO Advantage
BAC / BCR / BN	Traditional Banks	50% lower rates, instant approval, 24/7 access, global capital
Konfío / Kueski	LatAm Fintechs	On-chain transparency, global investor base, composability
Centrifuge / Maple	DeFi RWA	LatAm-native, local regulatory expertise, Sinpe integration
Aave / Compound	Pure DeFi	Real-world yield, undercollateralized lending, local compliance
10. APPENDIX: REGULATORY ROADMAP (Costa Rica)
Table
Quarter	Action	Target Outcome
Q1 2026	Incorporate Costa Rica S.A. + Cayman Foundation	Legal entity ready
Q2 2026	Meet with SUGEVAL, BCCR, SUPEN	Regulatory awareness, informal guidance
Q3 2026	Submit sandbox application	Formal regulatory testing environment
Q4 2026	Launch pilot under sandbox	$500K pilot with regulatory oversight
Q1 2027	Apply for full license	Permission to operate at scale
Q2 2027	Obtain payment institution / lending license	Full commercial launch
