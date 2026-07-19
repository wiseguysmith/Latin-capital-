# 03 — Regulatory Strategy: Costa Rica (Primary) + El Salvador, Colombia, Brazil

| Field | Value |
|---|---|
| Purpose | **Governing compliance document** for the capitalYA + CRP program. Where the PRD/specs conflict with this document on any near-term matter, this document wins. |
| Audience | Founders, legal/compliance, product, engineering, partner lenders, investors |
| Status | Regulatory research & operating-model analysis — **not a legal opinion.** Signed counsel opinions required before launch (see §17). |
| Version | 1.0.0 |
| Owner | Founder + external counsel |
| Dependencies | `01_CAPITALYA_PROGRAM_BRIEF.md`, `02_INTEGRATED_ARCHITECTURE.md`, `04_ENTITY_AND_PARTNER_STRUCTURE.md` |
| Provenance | Authored as the deep-research deliverable for capitalYA/CRP (research date July 19, 2026), preserved here verbatim as the program's compliance backbone. |
| Last updated | 2026-07-19 |

> **How to read this document.** This is the compliance backbone of the program. The Program Brief (`01`) and Integrated Architecture (`02`) are downstream of it. The original capitalYA PRD and Technical Specifications in `sources/` describe the north-star ambition; **this document constrains what is actually built first.** The 18 counsel questions in §17 are the gate before the closed pilot.

---

# capitalYA + CRP Regulatory Strategy

## Costa Rica Primary Market; El Salvador, Colombia, and Brazil Secondary Markets

**Research date:** July 19, 2026
**Target model:** Partner-originated lending, independent capital-readiness assessment, later tokenization and syndication
**Entities considered:** capitalYA CR S.A.; US parent; Cayman foundation/DAO; CRP assessment entity; local licensed lender; future SPV or trust structures

This report follows the ten-part research specification supplied for capitalYA and CRP.

## Important limitation

This is regulatory research and operating-model analysis, not a formal legal opinion. Before launch, capitalYA should obtain signed opinions from securities, banking, consumer-finance, data-protection, tax, and AML counsel in each jurisdiction.

---

# 1. Executive Verdict

## The recommended launch structure

capitalYA can likely launch a **non-custodial, partner-lender MVP in Costa Rica** without becoming a licensed financial intermediary, provided that:

1. A properly authorized or supervised lender makes every credit decision.
2. The lender signs the loan contract and is the legal creditor.
3. The lender disburses and receives funds through its own accounts or regulated payment providers.
4. capitalYA does not take deposits, pool investor money, guarantee repayment, set final credit terms, or lend from capital raised from the public.
5. CRP provides an assessment and readiness recommendation, not a public credit rating or final underwriting decision.
6. No transferable loan token is sold to the public during the MVP.

Costa Rican law defines financial intermediation around habitually collecting funds from the public and deploying those funds, at the intermediary’s own account and risk, into credit or investments. Only expressly authorized entities may conduct that activity. A software and assessment platform that does not receive public money, does not lend at its own risk, and does not become the creditor has a materially stronger “not a lender” position.

## What capitalYA should not launch in Q3 2026

As of **July 19, 2026**, the original deadline of obtaining approval by Q2 2026 has passed; Q2 ended on June 30, 2026. A Q3 2026 launch is still possible only for a constrained partner MVP.

Do not launch the following in Costa Rica during the first phase:

* Public retail sales of tokenized loans or invoices.
* A DEX for secondary trading.
* capitalYA-controlled wallets or pooled investor accounts.
* Automated approval without lender review.
* A Cayman DAO directly marketing investments to Costa Rican residents.
* Tokenized loan participations without a written SUGEVAL classification.
* A capitalYA-branded “credit rating” used publicly to sell investments.

Costa Rican securities law defines a security broadly as a patrimonial right capable of trading in the securities market and intended to obtain resources from the public. Public offerings and securities-intermediation services may be conducted only by authorized parties. The legal analysis is based on the economic rights and offering activity—not whether the instrument is represented by paper, a database entry, or an ERC-3643 token.

## Recommended expansion order

**Operational launch:** Costa Rica, using traditional loans and a licensed lender.

**Tokenization-first expansion:** El Salvador, initially through a private digital-asset issuance to no more than 50 qualified investors and a registered CNAD structurer.

**Regulated retail/investment-market expansion:** Colombia, through an existing SFC-authorized collaborative-financing platform.

**Institutional scale:** Brazil, only through CVM-registered crowdfunding, securitization, and—where applicable—BCB-authorized virtual-asset partners.

Brazil is the largest opportunity but also the easiest place to accidentally build three regulated businesses at once. That is generally frowned upon by both regulators and bank accounts.

---

# 2. Recommended Legal and Operational Architecture

## capitalYA CR S.A.

capitalYA CR should operate as:

* Technology provider.
* Borrower-intake and document-management provider.
* Loan-origination support service.
* Partner-management and workflow orchestration layer.
* Servicing technology provider, where servicing functions are contractually delegated.
* Non-binding risk-analysis and matching provider.

It should not describe itself as:

* A lender.
* A bank.
* A deposit-taker.
* An investment platform.
* A securities exchange.
* A fund manager.
* A guarantor of repayment.
* A provider of “approved investments.”

## Partner lender

The lender should remain responsible for:

* Final underwriting.
* Approval or rejection.
* Interest rate and loan terms.
* Borrower contract.
* Disbursement.
* Collections and enforcement.
* Consumer disclosures.
* AML reporting.
* Credit-file access and reporting.
* Regulatory reporting.
* Loan-loss accounting.
* Loan restructuring and default decisions.

## CRP assessment entity

CRP should provide:

* Document completeness analysis.
* Governance and financial-readiness evaluation.
* Data-quality flags.
* Risk-factor identification.
* Readiness score.
* Remediation recommendations.
* Evidence-backed assessment report.

The assessment should expressly state:

> The readiness score is an informational assessment and is not a promise of financing, investment recommendation, credit approval, public credit rating, guarantee of repayment, or substitute for a lender’s independent underwriting.

## Cayman foundation or DAO

The foundation may own protocol intellectual property or coordinate governance, but it should not initially:

* Contract directly with Costa Rican retail borrowers.
* Solicit Costa Rican investors.
* Receive loan repayments.
* Make credit decisions.
* Control local lender funds.
* Serve as the issuer of Costa Rican retail investment tokens.
* Attempt to use “decentralization” as a jurisdictional invisibility cloak.

A foreign issuer can still be captured by local offering law when an offer is made from Costa Rica or directed toward Costa Rican residents. A foreign entity does not become legally offshore merely because its smart contract is deployed offshore. Costa Rica’s securities rules expressly reach offerings directed into the Costa Rican market.

---

# 3. Regulatory Perimeter and Licensing

## 3.1 Costa Rica

### Direct answer

A matching and assessment platform does not automatically need a banking or lending license merely because it facilitates loans. The strongest dividing line is whether capitalYA:

* Collects funds from the public.
* Deploys those funds into loans at its own risk.
* Becomes creditor or co-creditor.
* Guarantees repayment.
* Exercises discretion over pooled capital.
* Markets transferable investment rights to the public.

Only authorized entities may perform financial intermediation, defined as habitually collecting financial resources from the public to apply them, for the intermediary’s own account and risk, to credit or securities.

A platform fee should not, by itself, make capitalYA a co-originator. The risk rises when compensation is combined with substantive creditor powers—such as approving loans, setting binding terms, funding credit, taking first loss, purchasing every loan, or guaranteeing performance.

### Regulatory risk

* **Platform and workflow services:** Green/Yellow.
* **Loan broker or originator-support role:** Yellow; obtain a banking-law opinion.
* **Taking investor money:** Red.
* **Funding loans from pooled public capital:** Red.
* **Retail token sale:** Red without SUGEVAL structure and authorization.
* **Private institutional token offering:** Yellow, subject to classification and private-offer compliance.

### Minimum viable footprint

The minimum viable footprint is:

* Costa Rican operating company.
* Licensed or supervised lender.
* Local commercial and consumer-finance agreements.
* Privacy registration analysis with PRODHAB.
* AML responsibility matrix.
* No client-money custody.
* No public offering.
* No local exchange or secondary market.
* Formal SUGEVAL consultation before any token launch.

### Correction to the original prompt

The referenced **Ley 8959 “Sistema de Banca para Todos”** is not the Costa Rican development-banking or fintech-sandbox law. The applicable development-banking statute is **Law 8634, Sistema de Banca para el Desarrollo**, whose current official record was updated through February 24, 2026. It provides development-finance mechanisms and participation by supervised or accredited entities; it does not create a general DeFi licensing exemption.

## 3.2 El Salvador

El Salvador has a distinct Digital Assets Issuance Law regime administered by the CNAD. Digital assets under that regime are expressly separated from traditional negotiable-securities laws, although issuers, offerings, structurers, platforms, custody providers, exchanges, and other service providers remain regulated under the digital-assets framework. Operating without CNAD approval is illegal.

A private digital-asset issuance may be directed to no more than 50 qualified investors, must use a CNAD-registered structurer, and receives a CNAD no-objection review with a stated 20-business-day evaluation period after a complete filing.

**Risk:** Yellow through registered partners; Red if capitalYA self-launches without CNAD registration.

## 3.3 Colombia

Colombia regulates collaborative financing through securities under Decree 1357 of 2018. The activity must be performed through an electronic infrastructure by an entity authorized by the Superintendencia Financiera de Colombia.

capitalYA should therefore partner with an existing authorized collaborative-financing company rather than operate its own retail investment platform initially.

**Risk:** Green/Yellow through a licensed partner; Red for an unlicensed capitalYA-operated platform.

## 3.4 Brazil

Brazil applies a substance-based analysis. Tokenization itself is not automatically regulated, but tokens representing receivables, debt, securitization instruments, or collective investment contracts may be securities. Public offerings, intermediation, custody, settlement, recordkeeping, and trading infrastructure then fall under CVM rules.

Separate BCB authorization may apply where the activity involves non-security virtual-asset brokerage, intermediation, or custody. BCB Resolution 520, issued November 10, 2025, establishes authorized categories for virtual-asset intermediaries, custodians, and brokers.

**Risk:** Yellow through regulated partners; Red for a self-operated, vertically integrated launch.

---

# 4. Partner-Lending Model

## 4.1 Can a partner originate while capitalYA tokenizes?

Legally, the structure is possible in principle, but tokenization cannot be treated as a cosmetic technology layer.

The enforceable legal chain must establish:

1. Who owns the receivable.
2. Whether the receivable can be assigned.
3. Whether borrower consent or notice is required.
4. Whether the token represents ownership, participation, beneficial interest, payment entitlement, or merely a record.
5. Who enforces the loan.
6. Who handles collections.
7. What occurs during default, insolvency, or servicing interruption.
8. Whether token holders have rights directly against the borrower, lender, SPV, trustee, or issuer.
9. Whether the resulting token is a security.
10. Whether transfers are legally effective outside the blockchain ledger.

ERC-3643 can enforce identity-based transfer restrictions, but it does not answer those legal questions. Smart-contract compliance is only one control layer; it is not a license, prospectus, assignment agreement, perfected security interest, or bankruptcy opinion.

## 4.2 Recommended contractual liability split

The partner agreement should include:

* Express statement that the lender is the sole originator and creditor.
* capitalYA’s inability to bind the lender.
* Final-decision authority reserved to lender personnel.
* Credit-policy ownership.
* Clear fee schedule separated from interest income.
* No capitalYA guarantee of principal or yield.
* Representations concerning lender authorization and good standing.
* AML, sanctions, KYC, and reporting responsibility matrix.
* Borrower-consent and data-sharing provisions.
* Loan-document ownership.
* Audit and regulator-access rights.
* Cybersecurity and incident-notification obligations.
* Servicing standards.
* Backup servicer.
* No commingling of funds.
* Records export and transition assistance.
* Change-of-control and change-in-law clauses.
* Indemnification for regulatory breaches caused by each party.
* Tokenization restrictions requiring prior legal approval.
* Wind-down and loan-transfer provisions.
* Business-continuity and disaster-recovery obligations.

## 4.3 What happens if the lender loses its license?

The outstanding loans do not disappear. The agreement should automatically:

* Stop new originations.
* Stop new token issuance.
* Move collections to an approved account.
* Trigger appointment of a backup servicer.
* Require delivery of complete borrower and loan files.
* Transfer or assign loans where legally permitted.
* Preserve borrower payment instructions.
* Notify investors and relevant regulators.
* Prevent capitalYA from independently “taking over” regulated lending activities.

capitalYA should never rely on the assumption that it may continue the lender’s regulated functions because the lender failed. A partner failure is a wind-down event, not an automatic promotion.

## 4.4 Platform fees

The safest fee structure is:

* Fixed technology subscription.
* Borrower assessment fee paid to CRP.
* Lender-paid workflow or origination-support fee.
* Documented success fee for completed introductions.
* Servicing-technology fee.
* Fixed investor-platform fee only after securities counsel approves the activity.

Higher-risk compensation includes:

* Percentage of loan interest.
* First-loss return.
* Spread between borrower and investor rates.
* Guaranteed minimum yield.
* Discretionary participation in principal.
* Payment conditioned on investor performance.

**Costa Rica risk:** Green/Yellow for fixed and disclosed service fees; Yellow/Red for economics resembling a creditor, fund manager, or securities intermediary.

---

# 5. CRP Assessment-Service Classification

## 5.1 Is CRP a credit-rating agency?

Not automatically.

A private readiness assessment used internally by a lender is materially different from a regulated rating of securities offered to the public. The regulatory risk increases when CRP:

* Calls itself a rating agency.
* Publishes ratings for investment solicitation.
* Rates a tokenized public debt issuance.
* Receives issuer compensation tied to the rating outcome.
* Represents that its rating satisfies a regulatory requirement.
* Allows investors to rely on the score as the principal investment recommendation.

Costa Rica has separate rules for risk-rating agencies and ratings used in public securities markets. A CRP readiness score should therefore remain an internal, decision-support assessment unless and until securities counsel confirms otherwise.

## 5.2 Can CRP charge $2,500–$5,000?

Yes, a flat fee for document analysis, readiness review, and remediation planning is substantially lower risk than compensation based on whether financing closes.

CRP should disclose:

* The precise scope of the assessment.
* Data sources.
* Score methodology at an understandable level.
* Assumptions and limitations.
* Date of assessment.
* Expiration or refresh date.
* Human-review process.
* Conflict-of-interest policy.
* Dispute and correction process.
* That the lender remains responsible for underwriting.

## 5.3 Liability for negligent scoring

CRP can still face contractual or tort claims if it:

* Uses knowingly inaccurate data.
* Ignores obvious document inconsistencies.
* Claims predictive accuracy it cannot substantiate.
* Conceals material limitations.
* Fails to follow its stated methodology.
* Allows investors to treat the score as a guarantee.
* Does not correct proven errors.

Recommended protections:

* Professional-services agreement.
* Limitation of liability.
* Exclusion of consequential loss, where enforceable.
* No third-party reliance without written authorization.
* Methodology governance committee.
* Version-controlled scoring models.
* Evidence log for each score.
* Human approval.
* Adverse-decision explanation.
* Borrower correction and appeal process.
* Annual independent model validation.
* Technology E&O, cyber, and professional-liability coverage.

## 5.4 Credit-information access

Costa Rica’s SUGEF operates the CIC for supervised financial institutions and the CICOC for qualifying non-supervised credit facilitators. Access is permissioned and requires debtor authorization rather than being an open credit-data API.

**Risk:** Green for private readiness assessment; Yellow for lender-facing credit-scoring services; Red without additional analysis if promoted as an investment-grade public rating.

---

# 6. Tokenization and Securities Law

## 6.1 Costa Rica

### Does tokenization make a loan a security?

Not automatically, but it very often pushes the arrangement toward securities regulation when the token:

* Represents a tradable patrimonial right.
* Is offered to obtain money from investors.
* Provides yield from loan repayments.
* Can be transferred between investors.
* Is marketed as an investment.
* Represents participation in a pool of receivables.

Costa Rican law defines a security as a patrimonial right capable of being traded in the securities market and intended to obtain resources from the public. That definition is technologically neutral.

### Public offering

A retail or broadly marketed tokenized-loan offering would likely require prior SUGEVAL authorization, registration, regulated offering documentation, and authorized intermediation.

### Private offering

Private offerings exceeding **US$1 million** by the same issuer or economic group must be accredited with SUGEVAL. Accreditation is not an approval, solvency review, or regulatory endorsement; investors do not receive the periodic-information protections of a registered public offering and cannot trade the securities in the registered secondary market.

### ERC-3643

ERC-3643 can support:

* Wallet allowlisting.
* Jurisdiction restrictions.
* Lockups.
* Investor qualification.
* Transfer approvals.
* Forced transfers.
* Identity-linked compliance rules.

It does not independently satisfy:

* Issuer authorization.
* Prospectus requirements.
* Private-offer rules.
* Broker-dealer requirements.
* Legal assignment.
* Custody requirements.
* Disclosure.
* Tax reporting.
* AML reporting.
* Bankruptcy remoteness.

### Secondary market

A capitalYA-operated DEX for loan tokens would create serious securities-intermediation and market-operation risk. The permissionless nature of a DEX is not compatible with an instrument requiring controlled transfers, investor eligibility, disclosures, recordkeeping, and jurisdiction-specific restrictions.

**Costa Rica risk:**

* Internal blockchain record without investor transferability: Yellow.
* One-to-one institutional participation documented off-chain: Yellow.
* Private token offering with counsel and SUGEVAL accreditation where required: Yellow.
* Public retail offer: Red before authorization.
* DEX trading: Red.

## 6.2 El Salvador

El Salvador provides the clearest dedicated token-issuance framework of the four markets. Digital assets can be structured and publicly or privately issued through the CNAD regime. The CNAD maintains active issuer, issuance, and PSAD registries, including registered real-estate and other tokenized offerings as of 2026.

The strongest pilot route is:

* Local or approved issuer.
* Registered structurer.
* Private offering.
* No more than 50 qualified investors.
* CNAD no-objection.
* Registered PSAD for relevant placement, custody, or trading services.

**Risk:** Yellow with partners; Red without CNAD registration.

## 6.3 Colombia

Colombia’s collaborative-financing regime is well suited to debt or equity financing of productive projects, but the electronic platform must be authorized by the SFC. A blockchain record can be integrated behind the regulated platform, but a separate unrestricted DEX would be inconsistent with the controlled offering structure.

**Risk:** Green/Yellow through an existing platform; Red for standalone retail token distribution.

## 6.4 Brazil

Brazilian regulators have specifically warned that receivables tokens and fixed-income tokens can be securities, including when they provide fixed or variable remuneration derived from credit rights. Certain offerings may use the CVM Resolution 88 crowdfunding route through a registered Brazilian platform.

A CVM crowdfunding platform must be organized in Brazil and registered and authorized by the CVM. The government’s published service target is approximately 90 calendar days for a complete registration request, though formation, documentation, technology, capital, controls, and remediation can make the practical project much longer.

**Risk:** Yellow through an incumbent; Red for a vertically integrated capitalYA exchange, custodian, issuer, and lender.

---

# 7. Sandbox and Regulatory Pathways

## 7.1 Costa Rica

The official Costa Rican resource located is the **Centro de Innovación Financiera**, a joint consultation and dialogue initiative of the financial supervisors and BCCR. Its published description presents it as an engagement channel, not a general authorization to suspend banking, securities, AML, or consumer laws.

I did not locate an official waiver-based Costa Rican sandbox equivalent to Colombia’s Espacio Controlado de Prueba.

Therefore, capitalYA should not base the launch plan on obtaining a “sandbox exemption.” It should use the CIF to:

* Present the full operating diagram.
* Ask which regulator owns each component.
* Obtain written or documented feedback.
* Request a SUGEVAL classification discussion.
* Confirm whether SUGEF views any activity as intermediation.
* Confirm whether AML-only registration applies.
* Document that capitalYA proactively approached regulators.

**Risk:** Yellow. Consultation is available; exemption should not be assumed.

## 7.2 El Salvador

El Salvador’s practical innovation path is the CNAD registration and issuance process rather than an informal sandbox. A complete public issuance application has a stated five-business-day CNAD evaluation period, while a private issuance has a stated 20-business-day period. Those clocks begin only after the structure and documents are complete.

## 7.3 Colombia

Colombia has both supervisory and regulatory sandbox mechanisms. Non-supervised companies may apply to the supervisory sandbox in alliance with an SFC-supervised institution. Regulatory exceptions are temporary and do not modify the permanent law; data-protection, tax, foreign-exchange, and monetary rules cannot be waived.

## 7.4 Brazil

Brazil has innovation and sandbox mechanisms, but they should not be treated as shortcuts around securities or virtual-asset authorization. The more practical commercial route is partnering with registered CVM and BCB participants.

---

# 8. AML/CFT and Sanctions

## 8.1 Costa Rica

The partner lender should be the primary regulated AML reporting entity for loan origination and financial transactions.

capitalYA should still conduct its own risk-based controls because it:

* Collects identity and ownership information.
* Introduces borrowers.
* Screens documents.
* May identify suspicious conduct before the lender sees it.
* May serve foreign investors.
* May facilitate digital-asset transactions later.

Costa Rican AML rules apply to supervised financial entities and to specified nonfinancial activities that may require AML registration with SUGEF. Such AML registration does not authorize the entity to conduct an otherwise regulated financial business.

### Required operating controls

* Individual and entity identity verification.
* Ultimate-beneficial-owner identification.
* Politically exposed person screening.
* Sanctions screening.
* Adverse-media screening.
* Source-of-funds review.
* Source-of-wealth review for higher-risk investors.
* Geographic-risk classification.
* Transaction monitoring.
* Wallet analytics before tokenization.
* Escalation to the partner lender’s MLRO.
* Record retention.
* Suspicious-activity referral process.
* Prohibition on tipping off.
* Periodic customer refresh.

### Sanctions lists

The compliance program should screen at minimum:

* United Nations lists.
* Costa Rican and applicable local lists.
* OFAC where US persons, US infrastructure, US dollar clearing, or US counterparties are involved.
* Any lists required by the partner bank or lender.

The prompt’s reference to transactions above **“$10,000 CRC”** should not be used. Ten thousand Costa Rican colones is only a small consumer transaction. Reporting thresholds and suspicious-transaction duties must be mapped to the exact entity type, transaction type, currency, and applicable regulation.

### Who reports?

The partner agreement must establish:

* Who files regulatory reports.
* Who investigates alerts.
* Who retains the case file.
* Who responds to information requests.
* How capitalYA transmits suspicious observations without tipping off the user.
* When capitalYA must file independently due to its own registration status.

**Risk:** Yellow. The lender cannot simply “own all AML” while capitalYA operates blindly.

## 8.2 Secondary markets

* **El Salvador:** CNAD-regulated issuers and PSADs have dedicated AML and reporting obligations.
* **Colombia:** An SFC-authorized partner and UIAF reporting structure should be used.
* **Brazil:** CVM or BCB-supervised partners should perform regulated AML functions; capitalYA should maintain a complementary group-level program.

No investor should receive simplified KYC merely because the investor is wealthy or accredited. Accreditation addresses investment eligibility, not identity or sanctions risk.

---

# 9. Data Protection and Cross-Border Processing

## 9.1 Costa Rica

Costa Rica’s Law 8968 applies to personal data in public or private manual and automated databases. The law requires prior, express, precise information about the database, collection purpose, recipients, processing, refusal consequences, rights, and database controller.

Personal data may be transferred only when the data subject has expressly and validly authorized the transfer and the transfer respects the law’s principles and rights. Transfers to foreign-country databases without consent are identified as a serious violation.

### Is US cloud storage prohibited?

I found no general Costa Rican data-localization requirement that forces all customer data to remain in Costa Rica.

US cloud processing should be structured with:

* Express international-transfer consent.
* Identification of recipients and processing countries.
* Controller-processor agreement.
* Subprocessor list.
* Encryption in transit and at rest.
* Access-control policy.
* Incident-response obligations.
* Retention and deletion schedule.
* Data-subject access, correction, and deletion procedures.
* Purpose limitation.
* PRODHAB registration analysis.
* Prohibition on placing personal data directly on a public blockchain.

Databases administered for distribution, diffusion, or commercialization must be registered with PRODHAB under Article 21. Whether CRP’s particular database requires registration should be confirmed from its exact use and sharing model.

### ZK proofs

Zero-knowledge proofs are compatible with privacy-by-design goals, but they do not replace:

* Consent.
* Lawful purpose.
* Accuracy.
* Correction rights.
* Controller accountability.
* Security.
* Deletion and retention rules.
* Governance of the source data used to produce the proof.

A ZK proof can reduce disclosure. It cannot make illegally collected source data legal.

## 9.2 Brazil

Brazil has a more prescriptive international-transfer regime. ANPD Resolution 19/2024 regulates international transfers and provides mechanisms including standard contractual clauses, equivalent clauses, specific clauses, binding corporate rules, and adequacy decisions.

## 9.3 Colombia and El Salvador

For Colombia, financial and credit data should be designed around the country’s general data-protection law and the special financial habeas-data regime. Colombia’s sandbox does not waive compliance with Laws 1581 or 1266.

El Salvador requires a separate local review of data rights, banking secrecy, CNAD information duties, and cross-border-processing contracts before launch.

**Overall data risk:** Yellow in Costa Rica; Yellow/Red if immutable personal information is written on-chain.

---

# 10. Consumer Protection and Disclosures

## 10.1 Costa Rica

Costa Rica’s consumer-credit regulation applies to supervised and non-supervised providers of financing, credit, or microcredit to consumers. It requires clear, truthful, free, Spanish-language information about factors affecting the consumer’s decision.

Consumer-facing credit materials should disclose:

* Legal lender.
* Total financed amount.
* Currency.
* Nominal and effective interest rates.
* Total cost.
* Fees and commissions.
* Payment schedule.
* Late charges.
* Collateral.
* Default consequences.
* Collection practices.
* Prepayment rights.
* Complaint channels.
* Data uses.
* Whether CRP’s score affects eligibility.
* How to dispute data or score results.

Contracts may not contain abusive clauses, and consumers have rights concerning early repayment. Advertising must be clear, timely, and non-misleading.

### Interest caps

Costa Rican law imposes maximum annual rates on financial, commercial, and microcredit operations. BCCR calculates and publishes applicable caps in January and July, and fees or commissions cannot be used to evade the caps. The actual rate must be checked for the currency and semester in which each contract is entered.

### Readiness-score rights

Even if no single statute uses the phrase “capital-readiness-score appeal,” CRP should implement:

* Notice that a score was used.
* Key adverse factors.
* Access to underlying submitted data.
* Correction process.
* Human review.
* Reassessment after remediation.
* Model-version record.
* Protection against prohibited discrimination.

That approach materially reduces privacy, consumer, discrimination, and negligence risk.

### Retail versus business borrowers

The consumer-credit rules are clearest where the borrower is a consumer. Commercial SME borrowers may have fewer consumer-law protections, but misleading advertising, data-protection, contract, fraud, and unfair-practice risks remain.

**Risk:** Yellow.

---

# 11. Tax Implications

## 11.1 Costa Rica

Costa Rican-source platform, assessment, and servicing fees earned by a Costa Rican operating company should be expected to fall within ordinary Costa Rican business taxation.

Payments of interest, commissions, and other financial expenses to nonresidents are generally subject to a 15% withholding framework, with specific reduced rates or exceptions for qualifying supervised financial institutions and certain international entities.

The final structure must separately analyze:

* Borrower interest.
* Lender interest.
* Investor yield.
* capitalYA platform fee.
* CRP assessment fee.
* Servicing fee.
* Token issuance fee.
* Secondary-trading gain.
* Assignment discount.
* Foundation or protocol fee.
* VAT treatment.
* Cross-border withholding.
* Transfer pricing.
* Permanent-establishment risk.
* Foreign-account and information-reporting obligations.

### Can capitalYA avoid income tax because it earns fees rather than interest?

No. Being “not a lender” is a regulatory position, not a tax exemption. Fee income earned through a Costa Rican business remains taxable under the applicable income and indirect-tax rules.

### Cayman foundation

Sending protocol fees to a Cayman foundation without documented substance, services, transfer pricing, and beneficial-owner transparency will create tax and banking problems rather than solve them.

**Risk:** Yellow. Obtain a transaction-level tax opinion before setting prices.

---

# 12. Long-Term Option C: Hybrid Origination and Syndication

## What changes at scale?

Option C creates a materially different regulatory profile if capitalYA:

* Funds loans itself.
* Holds loans before syndication.
* Purchases loans under forward-flow commitments.
* Pools investor capital.
* Selects investments using discretion.
* Issues fractional interests.
* Provides liquidity.
* Guarantees repurchase.
* Maintains reserves.
* Controls servicing and enforcement.
* Operates a secondary market.
* Holds more than incidental client assets.

At that point, capitalYA may resemble one or more of:

* Lender.
* Financial intermediary.
* Securities issuer.
* Securities intermediary.
* Investment fund.
* Fund manager.
* Securitization vehicle.
* Crowdfunding platform.
* Broker or placement agent.
* Custodian.
* Payment provider.
* Virtual-asset service provider.
* Market or trading-system operator.

## Can it remain “not a lender” at $100 million AUM?

AUM itself is not the only test. However, claiming not to be a lender becomes difficult if capitalYA controls underwriting, capital allocation, loan economics, servicing, enforcement, or credit risk across a $100 million portfolio.

The durable Option C structure should separate:

1. **Technology company:** capitalYA.
2. **Assessment company:** CRP.
3. **Licensed originators:** country-specific lenders.
4. **Issuer/SPV or trust:** owns receivables.
5. **Regulated placement platform:** distributes investments.
6. **Custodian or trustee:** holds assets or controls investor protections.
7. **Servicer and backup servicer:** administer loans.
8. **Fund manager or investment adviser:** where discretionary allocation exists.
9. **Transfer agent or registry:** maintains ownership records.
10. **Regulated secondary venue:** only where legally available.

**Risk:** Red for capitalYA acting as the entire stack; Yellow when regulated functions are separated among qualified partners.

---

# 13. Country Comparison

| Issue                          | Costa Rica                             | El Salvador                       | Colombia                                  | Brazil                                      |
| ------------------------------ | -------------------------------------- | --------------------------------- | ----------------------------------------- | ------------------------------------------- |
| Partner-lender MVP             | **Green/Yellow**                       | **Yellow**                        | **Green/Yellow**                          | **Yellow**                                  |
| Own lending operation          | **Red** without analysis/license       | **Red**                           | **Red**                                   | **Red**                                     |
| Private token pilot            | **Yellow** with SUGEVAL opinion        | **Green/Yellow** through CNAD     | **Yellow** through authorized platform    | **Yellow** through CVM structure            |
| Public retail token offering   | **Red** initially                      | **Yellow/Red** with CNAD approval | **Yellow** through SFC-regulated platform | **Yellow/Red** through CVM                  |
| Own retail investment platform | **Red**                                | **Red** without PSAD/CNAD         | **Red** without SFC authorization         | **Red** without CVM/BCB authorization       |
| Regulatory sandbox             | Consultation center; no waiver assumed | Dedicated digital-assets pathway  | Formal sandbox available                  | Formal innovation pathways                  |
| DEX secondary trading          | **Red**                                | **Yellow/Red**, licensed activity | **Red** for unapproved venue              | **Red** without proper market authorization |
| CRP private assessment         | **Green/Yellow**                       | **Green/Yellow**                  | **Yellow**                                | **Yellow**                                  |
| Foreign-cloud processing       | **Yellow**, express consent            | **Yellow**                        | **Yellow**                                | **Yellow**, ANPD mechanisms                 |
| Best initial use               | Traditional partner loan MVP           | Private token issuance            | Regulated crowdfunding partner            | Institutional/securitization scale          |

---

# 14. Practical Timelines and Cost Ranges

These are conservative project-planning ranges based on the expected legal, compliance, technical, audit, insurance, and partner-integration work. They are not published regulator fees.

| Project                                      | Planning timeline |     Estimated external cost |
| -------------------------------------------- | ----------------: | --------------------------: |
| Costa Rica partner-only MVP                  |        6–12 weeks |          US$40,000–$100,000 |
| Costa Rica private institutional token pilot |        3–6 months |          US$75,000–$200,000 |
| Costa Rica public securities offering        |      9–18+ months |        US$250,000–$750,000+ |
| El Salvador private CNAD issuance            |        2–4 months |          US$60,000–$175,000 |
| El Salvador public issuance/PSAD stack       |        4–9 months |        US$150,000–$500,000+ |
| Colombia existing-platform partnership       |        2–4 months |          US$50,000–$150,000 |
| Colombia own SFC-authorized platform         |      9–18+ months |      US$300,000–$1 million+ |
| Brazil existing CVM-platform partnership     |        3–6 months |         US$100,000–$300,000 |
| Brazil own CVM/BCB operating stack           |     12–24+ months |      US$750,000–$2 million+ |
| Group E&O/cyber coverage                     |        4–10 weeks | US$15,000–$75,000+ annually |
| Smart-contract audit                         |         4–8 weeks |         US$25,000–$150,000+ |
| Independent scoring-model validation         |        4–10 weeks |         US$20,000–$100,000+ |

---

# 15. Revised Q3 2026 Launch Plan

## July 20–31, 2026

* Freeze tokenized retail development.
* Produce the complete legal activity map.
* Select Costa Rican banking and securities counsel.
* Identify three potential SUGEF-supervised lenders.
* Define CRP as assessment-only.
* Finalize data-flow diagram.
* Prepare regulator presentation.
* Begin PRODHAB registration analysis.
* Create AML responsibility matrix.
* Draft partner term sheet.

## August 1–31, 2026

* Negotiate lender agreement.
* Complete banking-law legal opinion.
* Submit or conduct CIF consultation.
* Request SUGEVAL pre-classification discussion for future tokenization.
* Finalize borrower consent and privacy documents.
* Finalize CRP methodology governance.
* Implement human underwriting handoff.
* Integrate bank or lender payment rails.
* Bind cyber and E&O insurance.
* Conduct security assessment.

## September 1–30, 2026

* Launch closed Costa Rica pilot.
* Limit borrower and loan count.
* Use the lender’s accounts for all funds.
* Do not offer tokens.
* Do not admit retail investors.
* Audit the first assessments manually.
* Review complaints and data corrections.
* Test lender-failure and servicing-continuity procedures.
* Prepare pilot report for regulators and counsel.

## October–December 2026

Choose one tokenization pathway:

### Path A — El Salvador

Private CNAD issuance to no more than 50 qualified investors through registered providers.

### Path B — Colombia

Partner with an existing collaborative-financing platform for a controlled debt or productive-project offering.

### Path C — Costa Rica

Proceed with a private institutional structure only after SUGEVAL classification and counsel confirmation.

Brazil should remain a 2027 institutional expansion project unless a regulated Brazilian partner is already available.

---

# 16. Non-Negotiable “Do Not Cross” Lines

capitalYA should not:

1. Receive borrower or investor principal into its operating account.
2. Promise investors a return.
3. Guarantee loan repayment.
4. Automatically approve borrowers without lender authority.
5. Set final loan pricing without lender approval.
6. Market transferable tokens to Costa Rican retail investors.
7. Operate a DEX for loan interests.
8. Store personal or financial data on a public blockchain.
9. Describe a CRP score as regulatory approval.
10. Allow the Cayman foundation to solicit local investors directly.
11. Pool capital and choose loans without a fund-management analysis.
12. Continue regulated activities after a lender loses authorization.
13. Claim sandbox protection without written regulator approval.
14. Treat ERC-3643 as legal compliance by itself.
15. Assume an “accredited” investor requires less AML due diligence.

---

# 17. Formal Legal Opinions Required Before Launch

Costa Rican counsel should answer these questions in writing:

1. Does capitalYA’s precise fee and workflow model constitute credit intermediation, loan brokerage, or financial intermediation?
2. Is the proposed partner authorized to originate the intended SME, factoring, and consumer products?
3. Which party is legally considered the credit provider in every customer-facing interaction?
4. Does CRP require registration or specific disclosures?
5. Does the intended private token constitute a security?
6. Does the intended offer qualify as private?
7. Is SUGEVAL accreditation required due to aggregate offering size?
8. What assignment, notice, consent, and perfection rules apply to each receivable type?
9. Can an SPV or trust hold the receivables and issue participation interests?
10. What restrictions apply to foreign investors?
11. Which entity must register under AML Articles 15 or 15 bis?
12. Which entity files suspicious-transaction reports?
13. Which PRODHAB registrations are required?
14. What cross-border consent language is necessary?
15. What current interest caps apply to each product and currency?
16. What withholding applies to the selected investor and entity structure?
17. Which activities may capitalYA perform if the lender fails?
18. What regulator approvals are required before secondary transfers?

Separate opinions should then be obtained from CNAD counsel in El Salvador, SFC-capital-markets counsel in Colombia, and CVM/BCB counsel in Brazil.

---

# Final Recommendation

Proceed with capitalYA, but narrow Phase 1 aggressively.

The viable near-term business is not yet “DeFi lending for Costa Rica.” It is:

> **A regulated-partner capital platform that standardizes borrower readiness, assists origination, and coordinates lending without receiving funds or making the final credit decision.**

That model has commercial value on its own. It also creates the loan quality, documentation consistency, servicing history, and data governance needed for later tokenization.

Trying to launch lending, retail investment, token issuance, custody, a DAO, and secondary trading simultaneously would not be bold innovation. It would be regulatory speed-dating, except every regulator gets your number.

Build the compliant loan and assessment engine first. Tokenize only after the enforceable legal rights, servicing infrastructure, investor protections, and regulatory perimeter are proven.
