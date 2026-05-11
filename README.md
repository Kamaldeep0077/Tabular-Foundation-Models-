# Tabular-Foundation-Models-
This repository provides information on foundation models built specifically for tabular data. 
SLIDE 1: Tabular Foundation Models — What They Are, Why They Matter, and Where They Apply
What's Changing?

Banks have always built custom models for each problem — one for fraud, another for credit risk, another for churn. Each takes weeks to build, tune, and validate
A new class of pre-trained models is emerging that are purpose-built for the kind of structured data banks already have (transactions, credit files, customer records)
These models come "ready to learn" — they can be pointed at a new problem and deliver strong results with significantly less setup time and data
Two Notable Industry Moves

Who	What They Built	How It Works (Simply)
Mastercard	Large Tabular Model (LTM)	Trained on billions of anonymized card transactions to recognize patterns across payments, fraud, chargebacks. Learns what "normal" and "abnormal" look like across the entire payment network
PriorLabs	TabPFN model family	Trained on millions of simulated datasets to learn how to learn from any structured data. Give it a new dataset and it makes predictions immediately — no custom model build required
Why Should We Care?

Benefit	Plain-Language Impact
Faster deployment	What currently takes weeks of model development could be reduced to hours
Stronger on sparse problems	Performs better when positive cases are rare (fraud, money laundering, thin-file credit) — exactly where our traditional models struggle most
Better risk estimates	Produces more reliable probability scores — directly impacts how accurately we provision, price, and allocate capital
Fewer false alarms	Mastercard reports the biggest gains on high-value, infrequent transactions where current models tend to over-flag legitimate customers
Where Is the Industry Using These?

Use Case	Who's Doing It	Status
Fraud detection	Mastercard (LTM); Feedzai with Lloyds Banking Group	In production
Money laundering / mule accounts	Feedzai across $9T in annual payments	Early production
Credit risk (thin-file / niche segments)	Mission Lane, Taktile + PriorLabs	Pilot stage
Customer risk profiling	Nubank — serving 100M+ customers	In production
SLIDE 2: What This Means for Validation & Model Risk Management
Where Do These Fit Regulatorily?

The revised interagency MRM guidance (April 2026, replacing SR 11-7) keeps these models fully in scope — they are not generative AI, so the carve-out for GenAI does not apply
If used for credit or fraud decisions, they would likely be classified as high-materiality models requiring full validation
What's Different When We Validate These?

What We'd Assess	What Changes vs. Today's Models
How the model learns	These models don't train on our data in the traditional sense — they arrive pre-trained and adapt on the fly. We'd need to understand and document this new learning mechanism
Performance benchmarking	Must prove they match or beat our existing models on our data, across customer segments, and over time
Explainability	Traditional models (decision trees, gradient boosting) are relatively transparent. These models are less so — explanation methods exist but require more effort, especially for adverse action notices
Data privacy	Some models (Mastercard, Feedzai) are trained on real transaction data from multiple institutions. We'd need to assess data governance, consent, and privacy implications
Vendor dependency	Unlike open-source tools we control today, these often come from third-party vendors — creating concentration risk if multiple banks rely on the same provider
Ongoing monitoring	These models don't get "retrained" in the traditional cycle. We'd need new approaches to detect when performance degrades as market conditions shift
What's Not Ready Yet

Most models top out at moderate data volumes — not yet viable for scoring our largest portfolios without sampling
Significantly slower at scoring time than current models — not suitable for real-time, high-volume decisioning today
Limited production track record in regulated banking — tooling and best practices are still maturing
Bottom Line

These are not a replacement for our current model stack — Mastercard themselves plan to run these alongside traditional models, not instead of them
They are a strategic capability for problems where data is scarce and current models underperform — fraud, AML, thin-file credit
Our validation frameworks can accommodate these with targeted enhancements — the core MRM principles apply, but we need to evolve our approach on explainability, vendor risk, and monitoring.














Slide 1: Tabular Foundation Models — What, Why, and Where
What Are They? (2-3 bullet header)

A new class of pre-trained AI models purpose-built for structured/tabular data (the kind banks run on — transactions, credit files, customer records)
Unlike traditional ML (XGBoost, LightGBM) which requires building a new model from scratch per use case, these models come pre-trained and adapt to new tasks with minimal setup
Two emerging approaches in the industry:
Mastercard's Large Tabular Model (LTM) — trained on billions of anonymized card transactions; learns patterns across payment events, fraud incidents, chargebacks, and merchant data
PriorLabs' TabPFN — trained on millions of synthetic datasets to learn a "universal prediction algorithm"; adapts to new problems via in-context learning (no retraining needed)
Key Benefits for Banking

Benefit	What It Means
Faster time-to-model	Weeks of feature engineering and tuning reduced to hours — the model handles missing values, mixed data types, and feature discovery automatically
Superior small-data performance	Outperforms traditional ML when labeled data is scarce (fraud, AML, niche credit segments) — TabPFN showed ~3 AUC points over XGBoost on credit risk benchmarks
Better calibrated probabilities	Critical for pricing, provisioning, and capital reserves — delivers reliable probability estimates out of the box
Reduced false positives	Mastercard reports strongest gains on high-value, low-frequency transactions where traditional models over-flag
Emerging Use Cases in Financial Services

Use Case	Industry Example	Maturity
Fraud detection	Mastercard LTM (production); Feedzai RiskFM with Lloyds Banking Group (26M customers)	Production
AML / Mule account detection	Feedzai RiskFM across $9T in payments	Early production
Credit risk (niche/thin-file segments)	Mission Lane benchmarked TabICL vs LightGBM; Taktile + PriorLabs partnership	Pilot
Customer risk segmentation	Nubank foundation models serving 100M+ customers	Production
Slide 2: Validation & MRM Considerations
Regulatory Context

The revised interagency MRM guidance (OCC Bulletin 2026-13, April 2026) — which replaced SR 11-7 — explicitly scopes non-generative AI/ML models (including tabular foundation models) under full MRM requirements
These models would likely fall under Tier-1 (material) classification if used for credit decisioning or fraud, requiring full validation with dual control
Key Validation Dimensions

Dimension	Consideration	How It Differs from Traditional ML
Conceptual soundness	Must assess in-context learning mechanism — training data becomes part of inference input, not a separate training step	Fundamentally different from tree-based model training; requires understanding of Bayesian inference approximation
Performance testing	Benchmark vs incumbent models on bank-specific data; test calibration, subgroup fairness, and temporal stability	No hyperparameters to validate, but must validate that zero-tuning performance holds on our data distributions
Explainability	SHAP values available for local explanations; global interpretability more challenging than tree-based models	Tree splits are inherently interpretable; TFMs require post-hoc explanation methods — important for adverse action notices (CFPB/ECOA)
Data privacy	Models trained on real transactions (Mastercard, Feedzai) raise data governance questions; in-context learning exposes training data at inference	Traditional ML doesn't re-expose training data during scoring
Vendor / third-party risk	Reliance on external vendors (PriorLabs, Feedzai) creates concentration risk; GARP highlights correlated risk across institutions using same vendor models	Open-source XGBoost/LightGBM carry no vendor dependency
Ongoing monitoring	Must track performance drift and data drift — especially since TFMs don't retrain but inference-time context data may shift	Traditional models have clear retrain cycles; TFMs require new monitoring paradigms
Known Limitations (Current State)

Scale ceiling: Most TFMs handle 50K–500K samples (vs millions for XGBoost) — insufficient for scoring large portfolios without sampling
Inference latency: 40–10,000x slower than tree-based models — not viable for real-time decisioning at scale today
Production maturity: Limited track record in regulated banking environments; tooling and deployment patterns still evolving
Pragmatic View for the Bank

TFMs are not a wholesale replacement for existing ML — they are a complementary capability best suited for sparse-data problems (fraud, AML, thin-file credit) where traditional models underperform
Mastercard themselves plan hybrid systems combining traditional ML with their LTM
Validation frameworks need to evolve to address in-context learning, vendor dependency, and new explainability paradigms — but the core MRM principles from the revised guidance apply directly
Reading References for You
Must-reads (start here):

Mastercard announcement — new GenAI model — Official overview of their LTM
TabPFN Nature paper (2025) — The foundational academic paper; read the abstract and results sections
PriorLabs Financial Services page — Their banking-specific positioning
OCC Bulletin 2026-13 — Revised MRM Guidance — The new regulatory framework replacing SR 11-7
Banking-specific deep dives:
5. Mission Lane: Benchmarking TabICL for Enterprise Credit Risk — Real bank benchmarking TFMs vs LightGBM
6. Feedzai RiskFM press release — Foundation model deployed at Lloyds for fraud/AML
7. Nubank: Foundation Models for Customer Understanding — 100M+ customer production deployment
8. Can a Foundation Model Replace XGBoost for Credit Risk? — Honest benchmark comparison

MRM / Regulatory angle:
9. Databricks: Model Risk Management 2026 Banker's Guide — Practical guide to the revised guidance
10. GARP: SR 11-7 in the Age of Agentic AI — Vendor concentration risk and AI model governance
