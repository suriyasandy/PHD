# AI/ML Solution for OMRC Cross-Product Exception Management
## Proposal for AI Governance Approval & POC Initiative

---

## Executive Summary

The **Market Security & Services (MSS) OMRC team** faces unique challenges in managing exceptions across multiple products (equities, fixed income, FX, interest rate derivatives, commodities) that require advanced AI/ML capabilities. This proposal seeks AI Governance Committee approval for a **12-week Proof of Concept (POC)** to develop intelligent automation for:

1. **Automated Market Data Validation**: ML-powered reconciliation with Reuters, Bloomberg, and other external sources to detect data discrepancies
2. **Intelligent Email Conversation Analysis**: NLP to extract package/structured trade details and fee information from complex IRD (Interest Rate Derivatives) email threads
3. **Cross-Product Pattern Detection**: ML algorithms to identify suspicious patterns across multiple asset classes that indicate potential manipulation
4. **Predictive Risk Scoring & Triage**: Historical pattern-based risk assessment to prioritize high-risk exceptions across diverse product types

**Unique Value Proposition**: Unlike front office-focused tools, this solution addresses MSS-specific challenges of handling high-volume, cross-product surveillance with limited resources, manual market data validation, and complex structured trade parsing.

**Regulatory Alignment**: SR 11-7, EU AI Act, FCA AI Principles, IOSCO Trade Surveillance Standards

---

## 1. Business Problem: OMRC Team's Operational Challenges

### 1.1 The OMRC Context

The **Market Security & Services (MSS) OMRC function** is responsible for monitoring off-market rate exceptions across:
- **Multiple asset classes**: Equities, Fixed Income, FX, Interest Rate Derivatives (IRD), Commodities, Structured Products
- **Multiple trading venues**: Exchange-traded and OTC instruments
- **High exception volumes**: 150-300 exceptions daily across all products
- **Complex workflows**: Each product has unique characteristics requiring specialized knowledge

**Key Difference from Front Office Tools**: Front office tools are product-specific and trader-focused. OMRC requires **cross-product visibility** and **centralized exception management** that existing tools do not provide.

### 1.2 Current Operational Pain Points

#### **Challenge 1: Manual Market Data Validation Across Products**

**The Problem**:
For certain products and exceptions, the OMRC team encounters **market data quality issues** where:
- Reference rates from internal surveillance systems are outdated, missing, or incorrect
- Analysts must **manually verify rates** by checking:
  - **Reuters terminals** for FX spot rates, commodity prices, bond yields
  - **Bloomberg terminals** for derivatives pricing, credit spreads, volatility surfaces
  - **Exchange websites** for official settlement prices
- Validation requires **15-20 minutes per exception** when market data issues arise
- **Error-prone**: Manual lookups and transcription errors lead to incorrect validations (estimated 10-15% error rate)
- **Product expertise required**: Each asset class has different reference sources (e.g., LIBOR for USD swaps, EURIBOR for EUR swaps, ICE for oil futures)

**Example Scenario**:
- OMRC exception: Trader executed USD/EUR FX forward at 1.0850 when system reference rate shows 1.0820 (30 bps deviation)
- Analyst must:
  1. Check Reuters for FX forward curves at execution time
  2. Verify if rate was during volatile market period (e.g., ECB announcement)
  3. Cross-check with Bloomberg for interbank quotes
  4. Document justification if rate was within market bounds
- **Time consumed**: 20 minutes for a single exception when market data is questionable

**Current State Impact**:
- **30-40% of exceptions** require manual market data validation
- **50-60 analyst hours/week** spent on data lookups across Reuters/Bloomberg
- **Delays in exception closure**: Average 2-3 days vs. target of same-day closure
- **Inconsistent validation**: Different analysts use different sources and methodologies

**ML Solution Opportunity**:
- **Automated market data reconciliation**: ML agents query Reuters/Bloomberg APIs in real-time to fetch reference rates at exact execution timestamps
- **Anomaly detection**: ML models trained on historical market volatility to flag when rate deviations are within normal bounds vs. suspicious
- **Source selection intelligence**: NLP to understand product type from exception and automatically select correct reference source (Reuters vs. Bloomberg vs. exchange)
- **Impact**: Reduce manual validation time by 80%, improve consistency to 95%+, enable same-day exception closure

---

#### **Challenge 2: Complex Email Parsing for IRD Package/Structured Trades**

**The Problem**:
For **Interest Rate Derivatives (IRD)** products, traders often execute **package trades** (multiple linked swaps) or **structured trades** with complex fee arrangements:
- Trade details are communicated via **unstructured email threads** between front office and middle office
- Emails contain:
  - **Multiple sub-trades**: "Package consists of 5-year receiver swap ($100M notional, 3.75% fixed) + 10-year payer swap ($80M notional, 4.10% fixed)"
  - **Fee structures**: "Upfront fee: $250K; ongoing servicing fee: 2bps annually; early termination penalty: $500K"
  - **Justifications**: "Client requested off-market rate due to structured credit facility; relationship critical for cross-sell opportunities"
  - **Conversational context**: Email threads span 10+ messages with back-and-forth negotiations

**Current Manual Process**:
1. Analyst reads through entire email thread (5-15 emails)
2. **Manually extracts**:
   - Number of sub-trades in package
   - Notional amounts, fixed rates, floating rates, tenors for each sub-trade
   - Fee breakdown (upfront, ongoing, penalty clauses)
   - Business justification for off-market pricing
3. **Validates** fee calculations and package economics
4. **Documents** findings in case management system
5. **Approves or escalates** based on extracted information

**Time Required**: 20-30 minutes per IRD package trade exception

**Error Risks**:
- **Missed sub-trades**: Analyst overlooks one component in complex package (10% of cases)
- **Fee miscalculation**: Incorrect fee extraction leads to wrong approval decision (5-8% of cases)
- **Justification assessment inconsistency**: Different analysts have varying interpretations of "legitimate" justifications

**Example IRD Email Thread**:
```
Email 1 (Trader): "Need approval for package trade with XYZ Corp. 
- Trade A: 5yr receiver, $100M, 3.75% (market is 3.82%)
- Trade B: 10yr payer, $80M, 4.10% (market is 4.05%)
- Upfront fee: $250K
Client has structured credit facility that requires specific swap profile."

Email 2 (Middle Office): "Can you clarify the ongoing fees and early termination?"

Email 3 (Trader): "Ongoing is 2bps annually on combined notional. 
Early termination penalty is $500K if within first 2 years, then $250K years 3-5.
This is strategic relationship - client committed to $500M bond issuance next quarter."

Email 4 (Middle Office): "Approved subject to Credit sign-off on relationship value."
```

**Current State Impact**:
- **15-20% of OMRC exceptions** are IRD package/structured trades (30-50 daily)
- **12-15 analyst hours/week** spent parsing complex email threads
- **Inconsistent extraction quality**: 10-15% error rate in fee/sub-trade extraction
- **Knowledge dependency**: Only senior analysts can handle complex IRD structures

**ML Solution Opportunity**:
- **NLP-powered email parsing**: Extract structured data from unstructured email threads
  - **Named Entity Recognition (NER)**: Identify trade details (notional, tenor, rates, counterparties)
  - **Relation Extraction**: Link fees to specific sub-trades
  - **Conversation summarization**: Generate concise summary of 10+ email thread
  - **Justification quality assessment**: ML classifier to evaluate legitimacy of business rationale
- **Automated fee validation**: ML model trained on historical package trades to validate if fees are within market norms
- **Sub-trade completeness check**: Ensure all components of package are captured
- **Impact**: Reduce email parsing time by 75%, improve extraction accuracy to 95%+, enable junior analysts to handle complex IRD trades

---

#### **Challenge 3: Cross-Product Pattern Detection for Market Manipulation**

**The Problem**:
Sophisticated traders may engage in **cross-product manipulation**, where they:
- Execute off-market trades in **one product** to manipulate pricing in **a related product**
- Example: Large off-market equity order to influence correlated equity derivative pricing
- Example: Bond trade to impact interest rate swap curves

**Current Surveillance Limitation**:
- Existing surveillance systems analyze **each product independently**
- OMRC analysts review exceptions **within a single asset class**
- **No automated detection** of suspicious correlations across products
- **Manual investigation** only triggered if compliance officer notices pattern (rare)

**Regulatory Pressure**:
- **SEC/CFTC enforcement actions**: $920M fine to JP Morgan for cross-product spoofing in US Treasuries and futures (2020)
- **UK FCA focus**: Growing scrutiny on cross-product manipulation in FICC markets
- **IOSCO principles**: Emphasize need for holistic, cross-market surveillance

**Current State Impact**:
- **Zero proactive detection** of cross-product manipulation patterns
- **Reactive investigations only**: Triggered by external tips or regulator inquiries
- **Regulatory risk**: Potential for significant fines and reputational damage
- **Competitive disadvantage**: Peers deploying advanced cross-product surveillance (e.g., SteelEye, Eventus platforms)

**ML Solution Opportunity**:
- **Cross-product correlation analysis**: ML algorithms to detect suspicious patterns across asset classes
  - Example: Trader executes off-market equity trade at same time as large equity derivative position is opened
  - Example: Multiple off-market bond trades clustered around interest rate swap executions with same counterparty
- **Graph Neural Networks (GNNs)**: Map trader relationships and cross-product activity networks
- **Temporal pattern detection**: Identify time-sequenced activity across products indicating coordination
- **Anomaly scoring**: Flag cross-product exception clusters for investigation
- **Impact**: Proactive detection of manipulation; reduce regulatory risk; demonstrate best-in-class surveillance to regulators

---

#### **Challenge 4: Predictive Risk Scoring Across Diverse Product Types**

**The Problem**:
- OMRC handles **150-300 exceptions daily across 6+ asset classes**
- Current workflow: **Sequential processing** (first-in-first-out)
- **No prioritization**: High-risk violations may be reviewed late in the day
- **Resource constraints**: 8-10 analysts covering all products
- **Product specialization**: Each analyst has expertise in 1-2 asset classes, requiring careful queue assignment

**Consequences**:
- **Delayed escalations**: High-risk exceptions may take 2-3 days to reach senior analysts
- **Inefficient resource allocation**: Junior analysts spend time on complex cases they can't resolve; senior analysts waste time on low-risk cases
- **Regulatory SLA breaches**: Target is same-day exception closure; actual is 40-50% same-day closure rate
- **Analyst burnout**: Constant firefighting with unclear priorities

**ML Solution Opportunity**:
- **Historical pattern-based risk scoring**: Train ML model on 2-3 years of exceptions with closure outcomes (approved, escalated, violation)
  - Features: Product type, rate deviation magnitude, trader history, counterparty, trade size, timing, email sentiment
  - Output: Risk score (0-100) for each exception
- **Intelligent triage system**:
  - **Auto-approve** (score 0-20): Low-risk exceptions with clear justifications (30-35% of volume)
  - **Junior analyst queue** (score 20-50): Standard cases
  - **Senior analyst queue** (score 50-80): Complex or elevated risk
  - **Immediate escalation** (score 80-100): High probability violations requiring urgent investigation
- **Product-specific routing**: Automatically assign exceptions to analysts with relevant product expertise
- **Impact**: 35% workload reduction via auto-approval; same-day closure rate improvement to 85%+; better resource utilization

---

### 1.3 Why These Gaps Exist

These operational challenges exist because:

1. **OMRC is a centralized surveillance function** serving multiple business lines, unlike front office tools that are product-specific
2. **Market data providers (Reuters, Bloomberg)** are optimized for trading desks, not surveillance teams (different APIs, different access patterns)
3. **IRD structured trades** are inherently complex and customized, requiring contextual understanding that rule-based systems cannot provide
4. **Cross-product surveillance** requires correlation analysis across disparate systems and data formats that existing tools don't integrate
5. **High exception volumes with limited resources** demand intelligent prioritization that manual processes cannot deliver at scale

**Conclusion**: These are **genuine capability gaps** that AI/ML can address. This is not about replacing existing tools but **filling critical operational voids** that impact OMRC efficiency, risk detection, and regulatory compliance.

---

## 2. Proposed AI/ML Solution Architecture

### 2.1 Solution Overview

Our proposed solution consists of **four integrated ML components**, each addressing a specific OMRC operational gap:

#### **Component 1: Automated Market Data Validation Engine**

**Purpose**: Eliminate manual Reuters/Bloomberg lookups by automating reference rate fetching and validation

**ML Techniques**:
1. **API Integration Layer**:
   - Real-time connectivity to Reuters Eikon API, Bloomberg Data License
   - Query reference rates at exact exception execution timestamp
   - Handle multiple rate types: FX forwards, bond yields, swap curves, commodity prices, equity index levels

2. **Product-Aware Source Selection**:
   - **NLP-based product classification**: Parse exception description to determine product type (FX forward, USD interest rate swap, WTI crude oil futures, etc.)
   - **Rule engine + ML hybrid**: Use product type to select appropriate reference source
     - Example: "5-year USD swap" → Bloomberg BVAL swap curves
     - Example: "EUR/USD forward" → Reuters FX forward curves
     - Example: "ICE Brent crude" → ICE exchange settlement prices

3. **Intelligent Rate Reconciliation**:
   - **Fuzzy matching**: Account for timestamp precision (exact execution second may not have market quote; use nearest quote within ±1 minute)
   - **Bid-ask spread analysis**: ML model to determine if execution rate is within reasonable bid-ask bounds
   - **Volatility-adjusted thresholds**: ML anomaly detection trained on historical market volatility
     - During high volatility (e.g., Fed announcements, geopolitical events), wider rate deviations are normal
     - ML model learns volatility regimes and adjusts acceptable deviation thresholds dynamically

4. **Automated Validation Decision**:
   - **Auto-validate**: If execution rate is within ML-predicted bounds → Mark as "Market Data Validated - No Issue"
   - **Flag for review**: If execution rate exceeds bounds → Escalate to analyst with reference data attached
   - **Confidence scoring**: Provide confidence level (e.g., "95% confidence rate is within market bounds")

**Output**: For each exception, system provides:
- Reference rate from appropriate source (Reuters/Bloomberg/exchange)
- Execution rate vs. reference rate deviation (in bps)
- Volatility context (normal, elevated, extreme)
- Validation decision (auto-validated / requires review)
- Audit trail (data source, query timestamp, reference quote details)

**Impact Metrics**:
- **Time savings**: Reduce manual validation from 15-20 min to <1 min per exception
- **Accuracy improvement**: Eliminate manual lookup errors (reduce error rate from 10-15% to <2%)
- **Workload reduction**: 50-60 analyst hours/week saved
- **SLA improvement**: Enable same-day closure for data validation issues

---

#### **Component 2: Intelligent IRD Email Conversation Parser**

**Purpose**: Automatically extract structured trade details, fees, and justifications from complex email threads

**ML Techniques**:
1. **Email Thread Reconstruction**:
   - **Conversation threading**: Use email headers (In-Reply-To, References) to reconstruct full email chain
   - **Chronological ordering**: Sort emails by timestamp to understand negotiation flow
   - **Participant identification**: Extract sender/recipient roles (trader, middle office, credit, compliance)

2. **Named Entity Recognition (NER) for Financial Entities**:
   - Fine-tuned **FinBERT** model to extract:
     - **Trade identifiers**: Trade ID, package ID, sub-trade labels (Trade A, Trade B, Leg 1, Leg 2)
     - **Notional amounts**: $100M, EUR 50M, £25M (with currency normalization)
     - **Rates**: 3.75% fixed, LIBOR + 50bps, 4.10% (distinguish fixed vs. floating)
     - **Tenors**: 5-year, 10-year, 3M, 6M (normalize to standard format)
     - **Counterparties**: Client names, entity IDs
     - **Dates**: Execution date, effective date, maturity date
     - **Fees**: Upfront fee, ongoing fee, termination penalty (extract amounts and fee types)

3. **Relation Extraction**:
   - **Link entities**: Associate fees with specific sub-trades
     - Example: "$250K upfront fee" applies to "entire package" vs. "Trade A only"
   - **Calculate package economics**: Sum notionals, calculate weighted average rates, total fee burden
   - **Validate fee consistency**: Check if stated total fee matches sum of individual fee components

4. **Justification Quality Assessment**:
   - **Text classification**: Categorize business justifications into:
     - **High quality**: Specific client relationship value, cross-sell commitments, structured facility requirements
     - **Medium quality**: General relationship statements without quantification
     - **Low quality**: Vague or missing justifications
   - **Sentiment analysis**: Detect urgency or pressure language that may indicate coercion
     - Red flags: "Client threatening to move business", "Must approve by EOD or lose deal"
     - Green signals: "Strategic partnership aligned with long-term revenue targets"

5. **Conversation Summarization**:
   - **Abstractive summarization**: Generate 3-5 sentence summary of 10+ email thread
     - Example: "Trader requests approval for 2-leg package trade with XYZ Corp (5yr receiver $100M + 10yr payer $80M). Off-market rates justified by structured credit facility. Total upfront fee $250K with 2bps ongoing fee. Client committed to $500M bond issuance next quarter."
   - **Key decision timeline**: Extract approval/rejection decisions and rationale from middle office responses

**Output**: For each IRD exception, system provides:
- **Structured trade details** (JSON format):
  ```json
  {
    "package_id": "PKG-2025-1234",
    "sub_trades": [
      {
        "trade_id": "Trade A",
        "product": "Interest Rate Swap",
        "notional": 100000000,
        "currency": "USD",
        "tenor": "5Y",
        "fixed_rate": 0.0375,
        "floating_rate": "3M LIBOR",
        "pay_receive": "receive"
      },
      {
        "trade_id": "Trade B",
        "product": "Interest Rate Swap",
        "notional": 80000000,
        "currency": "USD",
        "tenor": "10Y",
        "fixed_rate": 0.0410,
        "floating_rate": "3M LIBOR",
        "pay_receive": "pay"
      }
    ],
    "fees": {
      "upfront": 250000,
      "ongoing_bps": 2,
      "termination_penalty": 500000
    },
    "justification": {
      "category": "Structured Facility",
      "quality_score": 85,
      "cross_sell_commitment": "500M bond issuance Q2"
    }
  }
  ```
- **Email conversation summary** (natural language)
- **Justification quality score** (0-100)
- **Extraction confidence** (e.g., "95% confidence all sub-trades extracted")
- **Flagged concerns** (e.g., "Urgency language detected in Email 3")

**Impact Metrics**:
- **Time savings**: Reduce email parsing from 20-30 min to 2-3 min per exception
- **Accuracy improvement**: Reduce extraction errors from 10-15% to <5%
- **Knowledge democratization**: Enable junior analysts to handle complex IRD cases
- **Workload reduction**: 12-15 analyst hours/week saved
- **Consistency**: Standardized extraction methodology across all analysts

---

#### **Component 3: Cross-Product Pattern Detection System**

**Purpose**: Proactively identify suspicious trading patterns across multiple asset classes

**ML Techniques**:
1. **Data Integration Layer**:
   - Aggregate OMRC exceptions across all products: Equities, Fixed Income, FX, IRD, Commodities, Structured Products
   - Enrich with trade data: Execution timestamps, order book activity, counterparty details
   - Link to communication data: Email threads, chat logs (if available)

2. **Correlation Analysis**:
   - **Product correlation mapping**: Maintain correlation matrix between products
     - Example: US Treasury bonds ↔ US Treasury futures (high correlation)
     - Example: Equity indices ↔ Equity index futures (high correlation)
     - Example: Corporate bonds ↔ Credit Default Swaps (CDS) on same entity
   - **Temporal pattern detection**: Flag when off-market trades in correlated products occur within short time windows (e.g., ±15 minutes)

3. **Graph Neural Networks (GNNs) for Relationship Mapping**:
   - **Trader-Counterparty-Product graph**: Model relationships between:
     - Traders → Counterparties (who trades with whom)
     - Traders → Products (who trades which products)
     - Products → Products (which products are correlated)
   - **Community detection**: Identify clusters of traders and counterparties with suspicious cross-product activity
   - **Anomaly scoring**: GNN produces anomaly score for each trader-product-counterparty combination

4. **Sequence Pattern Mining**:
   - **Time-series clustering**: Detect repeated patterns in trader behavior across products
     - Example: Trader consistently executes off-market equity trades followed by equity derivative positions within 1 hour
   - **LSTM models**: Learn temporal sequences of cross-product activity indicating manipulation
     - Training data: Historical manipulation cases (internal + public enforcement actions)

5. **Scenario-Based Detection**:
   - **Predefined manipulation scenarios**:
     - **Cross-Product Spoofing**: Large off-market order in Product A to manipulate price in correlated Product B
     - **Layering across products**: Build positions in Product A while simultaneously executing off-market trades in Product B to benefit Product A positions
   - **ML scenario matching**: Score each exception against known manipulation scenarios

**Output**: Daily/weekly cross-product surveillance reports:
- **High-priority alerts**: Traders with cross-product anomaly scores >80/100
  - Example: "Trader X executed 3 off-market USD swap trades on same day as large USD bond purchases with Counterparty Y. Historical correlation between these activities is 95%. Potential cross-product manipulation."
- **Trader-Counterparty risk networks**: Visualizations of suspicious relationship clusters
- **Temporal heatmaps**: Show when cross-product activity spikes (e.g., around economic data releases)
- **Explainability**: SHAP values showing which cross-product patterns contribute to high risk scores

**Impact Metrics**:
- **Proactive detection**: Shift from reactive to proactive cross-product surveillance
- **Regulatory compliance**: Demonstrate best-in-class surveillance to FCA, SEC, CFTC
- **Risk reduction**: Early detection of manipulation reduces potential regulatory fines (estimated $5M-$20M risk mitigation annually)
- **Competitive positioning**: Match capabilities of advanced RegTech platforms (SteelEye, Eventus)

---

#### **Component 4: Predictive Risk Scoring & Intelligent Triage**

**Purpose**: Prioritize exceptions by predicted risk level and route to appropriate analyst queues

**ML Techniques**:
1. **Historical Pattern Learning**:
   - **Training data**: 2-3 years of OMRC exceptions (estimated 50,000-80,000 cases) with closure outcomes:
     - **Approved** (70-75%): Legitimate off-market trades with valid justifications
     - **Escalated** (20-25%): Required senior analyst review or compliance investigation
     - **Violations** (5-10%): Confirmed policy breaches or regulatory violations
   - **Feature engineering** (100+ features):
     - **Exception characteristics**: Rate deviation magnitude (bps), trade size (notional), product type, venue
     - **Trader features**: Trader tenure, historical exception rate, product specialization, performance reviews
     - **Counterparty features**: Counterparty credit rating, relationship length, trading volume
     - **Temporal features**: Time since last exception, clustering score (multiple exceptions in short window), time of day (off-hours trading)
     - **Email features** (from Component 2): Justification quality score, urgency language score, email chain length
     - **Market data features** (from Component 1): Market volatility at execution time, bid-ask spread width
     - **Cross-product features** (from Component 3): Cross-product anomaly score

2. **Ensemble Model Training**:
   - **XGBoost classifier**: Primary model for risk prediction
     - Multi-class classification: Predict probability of {Auto-Approve, Standard, Escalate, Violation}
   - **Random Forest**: Backup model for comparison and ensemble voting
   - **Hyperparameter tuning**: 5-fold cross-validation on training set
   - **Class imbalance handling**: SMOTE (Synthetic Minority Over-sampling) for rare Violation class

3. **Risk Score Calibration**:
   - **Probability → Score mapping**: Convert model probabilities to 0-100 risk score
     - 0-20: Very low risk (auto-approve candidates)
     - 20-50: Low-medium risk (junior analyst queue)
     - 50-80: Medium-high risk (senior analyst queue)
     - 80-100: High risk (immediate escalation to compliance)
   - **Threshold optimization**: Tune thresholds to meet business constraints
     - Target: 95%+ precision for auto-approve category (minimize false approvals)
     - Target: 90%+ recall for violation category (minimize missed violations)

4. **Explainability Module**:
   - **SHAP (SHapley Additive exPlanations)**: For each exception, provide top 5 risk factors
     - Example: "Risk Score: 72/100. Top factors: (1) Trader has 4 similar exceptions in past 10 days (+22 points), (2) Rate deviation is 2.5x trader's historical average (+18 points), (3) Email urgency language detected (+12 points), (4) Off-hours execution (+10 points), (5) Counterparty relationship <6 months (+10 points)"
   - **Counterfactual explanations**: "If rate deviation were 50bps instead of 80bps, risk score would drop to 55/100"

5. **Intelligent Triage & Routing**:
   - **Auto-approval workflow**:
     - Exceptions with score 0-20 AND high model confidence (>90%) → Auto-approve
     - Daily audit sampling: 5-10% of auto-approved cases reviewed by senior analyst
   - **Queue assignment**:
     - Junior analyst queue: Scores 20-50, standard products (Equities, FX)
     - Senior analyst queue: Scores 50-80, complex products (IRD, Structured Products)
     - Compliance escalation: Scores 80-100, immediate notification to Head of OMRC
   - **Product-based routing**:
     - Match exception product type to analyst specialization
     - Load balancing: Distribute workload evenly across analysts

**Output**: For each exception:
- **Risk score** (0-100)
- **Triage category** (Auto-Approve / Junior Queue / Senior Queue / Escalate)
- **Top 5 risk factors** (SHAP values with point contributions)
- **Recommended analyst** (based on product expertise and current workload)
- **Similar historical cases** (retrieval of 3-5 most similar exceptions with outcomes)

**Impact Metrics**:
- **Workload reduction**: 30-35% auto-approval rate → 50-100 exceptions/day handled without analyst intervention
- **Efficiency gain**: Analysts focus on high-value cases; reduce time on low-risk exceptions by 60%
- **SLA improvement**: Same-day closure rate improvement from 40-50% to 85%+
- **Resource optimization**: Better alignment of analyst expertise to exception complexity
- **Risk detection**: 90%+ recall on violation category → fewer missed regulatory breaches

---

## 3. AI Governance & Compliance Framework

### 3.1 Regulatory Classification

**AI System Risk Level**: **High-Risk AI System**

**Regulatory Frameworks**:
1. **EU AI Act (Article 6)**: High-risk classification due to use in risk assessment and compliance management
2. **SR 11-7 (Model Risk Management)**: Supervisory guidance on model validation, governance, and monitoring
3. **FCA AI Principles**: Transparency, fairness, accountability, security, redress, data governance
4. **IOSCO Trade Surveillance Standards**: Cross-market surveillance, market abuse detection

### 3.2 Model Inventory Registration

| Field | Details |
|-------|---------|
| **Model Name** | OMRC Cross-Product Exception Management AI System |
| **Model Components** | (1) Market Data Validation Engine, (2) IRD Email Parser, (3) Cross-Product Pattern Detector, (4) Risk Scoring & Triage System |
| **Model Owner** | Head of Market Security & Services - OMRC Function |
| **Business Sponsor** | Chief Compliance Officer / Head of Surveillance |
| **Model Developers** | AI/ML Engineering Team (Internal) + Data Science Team |
| **Risk Classification** | Tier 1 - High Risk (Material impact on compliance and regulatory obligations) |
| **Primary Use Case** | Automate OMRC exception management: market data validation, email parsing, cross-product surveillance, risk scoring |
| **Model Types** | Ensemble: NLP (FinBERT, NER), ML (XGBoost, Random Forest), GNN (Graph Neural Networks), Anomaly Detection (Isolation Forest) |
| **Training Data** | 2-3 years OMRC exceptions (~50K-80K cases), market data (Reuters/Bloomberg), email threads, historical closure outcomes |
| **Model Inputs** | Exception details, trade data, email text, market data, trader/counterparty profiles, historical patterns |
| **Model Outputs** | (1) Market data validation decision, (2) Structured IRD trade details, (3) Cross-product risk alerts, (4) Exception risk scores (0-100), (5) Triage recommendations |
| **Deployment Environment** | Cloud (AWS SageMaker / Azure ML) - Dedicated VPC, encrypted at rest and in transit |
| **Model Refresh Frequency** | Weekly retraining with new exceptions and closure outcomes |
| **Performance Monitoring** | Real-time dashboards: Precision, Recall, F1-score, AUC-ROC, Model Drift, Bias Metrics |
| **Independent Validation** | Quarterly validation by Model Risk Management team; Annual audit by Internal Audit |
| **Human Oversight** | Mandatory review for high-risk exceptions (score >80); Daily audit sampling of auto-approved cases (5-10%) |
| **Compliance Requirements** | SR 11-7, EU AI Act, FCA AI Principles, GDPR, Model Risk Policy, Surveillance Policy |

### 3.3 Explainability & Transparency

**Challenge**: Ensemble ML models (XGBoost, GNNs) can be complex and difficult to interpret

**Mitigation Strategies**:

1. **SHAP Values for Risk Scores**:
   - Every risk score accompanied by top 5 contributing factors with point values
   - Example: "Risk Score: 87/100. Factors: (1) Cross-product anomaly detected (+25), (2) Trader exception clustering (+22), (3) Email urgency language (+18), (4) Off-market rate 3x normal deviation (+12), (5) New counterparty relationship (+10)"
   - Analysts can drill down into factor details for investigation

2. **Model Cards** (Comprehensive Documentation):
   - Model architecture, training methodology, data sources
   - Performance metrics by product type, trader segment, exception category
   - Bias analysis: Ensure no discrimination by geography, trader seniority, product type
   - Limitations and edge cases clearly stated
   - Version control and change log

3. **Audit Trail**:
   - Every model decision logged with:
     - Input data snapshot (exception details, email text, market data)
     - Model version used
     - Risk score and triage decision
     - SHAP explanations
     - Analyst override (if applicable) with rationale
   - Retention: 7 years (regulatory record-keeping requirement)

4. **Transparency for Stakeholders**:
   - **OMRC Analysts**: Full visibility into model decisions; override capability with documented rationale
   - **Traders**: General awareness that exceptions are analyzed by AI (communicated via existing surveillance policy)
   - **Compliance & Audit**: Complete access to model documentation, validation reports, decision logs, performance metrics
   - **Regulators**: Upon request, provide explainability reports and model governance documentation

### 3.4 Bias Detection & Fairness

**Risk**: Model may unfairly penalize certain traders, products, or geographic regions

**Mitigation**:

1. **Pre-Deployment Bias Testing**:
   - Analyze model performance across segments:
     - **Geography**: EMEA vs. APAC vs. Americas traders
     - **Product types**: Equities, FX, Fixed Income, IRD, Commodities
     - **Trader tenure**: Junior (<2 years) vs. Mid (2-5 years) vs. Senior (>5 years)
   - **Fairness metrics**:
     - **False positive rate parity**: Ensure <10% variance across segments
     - **Precision parity**: Ensure precision (auto-approve category) is similar across segments
   - **Disparate impact analysis**: If model flags >20% more exceptions for any segment, investigate for bias

2. **Ongoing Monitoring**:
   - Monthly bias metrics in model monitoring dashboard
   - Alerts if fairness metrics deteriorate (e.g., false positive rate for one segment increases by >15%)
   - Quarterly review by Compliance and Model Risk teams

3. **Fairness-Aware Training**:
   - Apply **fairness constraints** during model training (e.g., equalized odds across segments)
   - Separate validation sets for minority segments (e.g., rare product types) to ensure model performs well on all segments

### 3.5 Data Governance & Privacy

**Data Sources**:
1. OMRC exception data from trade surveillance system (trade IDs, rates, counterparties, products)
2. Email threads from shared OMRC mailboxes (text content only)
3. Market data from Reuters/Bloomberg APIs (reference rates, volatility data)
4. Historical exception closure outcomes (approved, escalated, violation)
5. Analyst notes and rationale (structured fields + free text)

**GDPR Compliance**:
- **Legal Basis**: Legitimate interest (operational risk management, regulatory compliance, market abuse prevention)
- **Data Minimization**: Only necessary data collected; no extraneous personal information
- **Pseudonymization**: Trader names replaced with employee IDs in training data; counterparty names pseudonymized where feasible
- **Retention**:
  - Training data: 3 years (aligned with regulatory record-keeping for surveillance)
  - Model decision logs: 7 years (SR 11-7 requirement)
  - Email data: Per existing email retention policy (typically 7 years)
- **Access Controls**: Role-based access; only authorized OMRC, Risk, Compliance personnel
- **Data Subject Rights**: Traders can request access to their data; right to rectification if data is inaccurate

**Data Quality Assurance**:
- **Completeness**: Validate that 95%+ of exceptions have closure outcomes in historical data
- **Accuracy**: SME review of 10% sample of historical data to verify closure outcome labels
- **Consistency**: Standardize data formats across products and trading venues (ETL pipeline)
- **Timeliness**: Real-time data feeds for market data (Reuters/Bloomberg APIs); daily batch updates for exception data

### 3.6 Third-Party & Vendor Risk Management

**External Vendors/Services**:

1. **Reuters (LSEG) Eikon API**:
   - **Purpose**: Real-time market data (FX rates, bond yields, commodity prices)
   - **Data Shared**: Exception product identifiers, timestamps (no trader/counterparty PII)
   - **Security**: API calls encrypted (TLS 1.3); data encrypted at rest
   - **Vendor Due Diligence**: Annual SOC 2 Type II review; security questionnaire

2. **Bloomberg Data License**:
   - **Purpose**: Derivatives pricing, swap curves, volatility surfaces
   - **Data Shared**: Product identifiers, timestamps
   - **Security**: Encrypted connections; data encrypted at rest
   - **Vendor Due Diligence**: Bloomberg BAA (Business Associate Agreement); annual security assessment

3. **AWS SageMaker (or Azure ML)**:
   - **Purpose**: ML model training, deployment, serving
   - **Data Shared**: Pseudonymized training data (no PII in cloud environment)
   - **Security**: Data encryption at rest (AES-256) and in transit (TLS 1.3); dedicated VPC with network isolation
   - **Vendor Due Diligence**: AWS SOC 2 Type II, ISO 27001; GDPR-compliant data processing addendum

4. **Open-Source Libraries** (Hugging Face Transformers, XGBoost, PyTorch):
   - **Purpose**: NLP and ML algorithms
   - **Deployment**: Self-hosted within internal infrastructure (no data shared with external parties)
   - **Security**: Quarterly vulnerability scans; version updates for security patches

**No Third-Party Black-Box Models**: All models developed internally; full control over architecture, training, and explainability

### 3.7 Model Documentation (SR 11-7 Requirements)

**Development Documentation**:

1. **Conceptual Soundness**:
   - Literature review: NLP in financial surveillance, cross-product manipulation detection, ML for trade surveillance
   - Rationale for model architecture choices:
     - Why FinBERT for NLP (domain-specific pre-training on financial text)
     - Why XGBoost for risk scoring (handles non-linear relationships, robust to missing data)
     - Why GNNs for cross-product detection (captures network relationships)
   - Assumptions and limitations clearly stated:
     - Model performance degrades for rare exception types (<0.5% of training data)
     - Requires retraining if new product types introduced (e.g., cryptocurrencies)
     - Email parsing accuracy depends on email formatting (structured vs. unstructured text)

2. **Data and Methodology**:
   - Training data sources, preprocessing steps (cleaning, normalization, feature engineering)
   - Model training process:
     - Hyperparameter tuning (grid search, Bayesian optimization)
     - Cross-validation (5-fold stratified CV)
     - Performance metrics on test set (precision, recall, F1, AUC-ROC by exception category)
   - Feature importance analysis (SHAP values, permutation importance)

3. **Model Limitations**:
   - **Cannot detect** highly sophisticated manipulation if language is deliberately neutral or coded
   - **Performance degrades** for rare products (<1% of training data): e.g., exotic derivatives, new structured products
   - **Requires human oversight** for high-risk exceptions (score >80) due to potential false positives
   - **Market regime changes** (e.g., shift from low to high interest rate environment) may require model retraining

**Validation Documentation**:

1. **Outcomes Analysis**:
   - Compare model predictions to actual outcomes on hold-out test set (15% of data)
   - **Targets**:
     - Precision (auto-approve category): >95%
     - Recall (violation category): >90%
     - AUC-ROC (risk score): >0.85
   - Performance breakdown by product type, trader segment, exception size

2. **Sensitivity Analysis**:
   - Test model performance under stressed scenarios:
     - High market volatility (e.g., 2020 COVID crash, 2008 financial crisis data)
     - Introduction of new trader cohort (e.g., onboarding 50 new traders in one month)
     - Product mix shift (e.g., 50% increase in IRD exceptions)
   - Validate that model remains robust under these scenarios

3. **Benchmarking**:
   - Compare to baseline (current manual process):
     - Baseline accuracy: ~85-90% (human analyst decision accuracy)
     - ML model target: >90% accuracy, >95% precision for auto-approve
   - Compare to industry standards (RegTech vendor solutions, academic literature)

**Ongoing Monitoring Documentation**:

1. **Monthly Performance Reports**:
   - Model accuracy, precision, recall, F1-score by category
   - Model drift metrics (data drift, concept drift)
   - Incident log (model failures, false approvals, false escalations)
   - Auto-approval rate, analyst override rate

2. **Quarterly Validation Reviews**:
   - Independent Model Risk team review
   - Bias and fairness analysis (segment-level performance)
   - Comparison to benchmark (manual process, previous model versions)
   - Recommendation: Continue / Retrain / Enhance / Discontinue

3. **Annual Audit**:
   - Internal Audit assessment of model governance
   - Review of model documentation completeness
   - Compliance with SR 11-7, EU AI Act, FCA AI Principles
   - Attestation that model is fit for purpose and governance controls are effective

---

## 4. Proof of Concept (POC) Plan: 12-Week Roadmap

### 4.1 POC Objectives

**Primary Goal**: Demonstrate that AI/ML can address OMRC-specific operational challenges and deliver measurable efficiency and accuracy improvements

**Success Criteria (Quantitative)**:

1. **Market Data Validation** (Component 1):
   - **Automation rate**: 80%+ of market data validations completed without manual lookup
   - **Accuracy**: <2% error rate in reference rate fetching and validation
   - **Time savings**: Reduce validation time from 15-20 min to <2 min per exception

2. **IRD Email Parsing** (Component 2):
   - **Extraction accuracy**: >90% precision and recall for trade details, fees, justifications
   - **Time savings**: Reduce email parsing time from 20-30 min to <5 min per exception
   - **Completeness**: Capture 95%+ of sub-trades in complex package structures

3. **Cross-Product Detection** (Component 3):
   - **Detection capability**: Identify 5+ suspicious cross-product patterns in historical data (validation with SMEs)
   - **False positive rate**: <20% of flagged patterns are false alarms

4. **Risk Scoring & Triage** (Component 4):
   - **Risk score AUC-ROC**: >0.85 on hold-out test set
   - **Auto-approve precision**: >95% (i.e., <5% of auto-approved cases should have been escalated)
   - **Violation recall**: >90% (capture 90%+ of actual violations)
   - **Auto-approval rate**: 30-35% of exceptions eligible for auto-approval

**Success Criteria (Qualitative)**:

1. **Analyst Satisfaction**: >75% of OMRC analysts find ML system helpful (post-POC survey)
2. **Explainability**: Model explanations (SHAP values) are clear and actionable for analysts
3. **Bias Testing**: No significant bias detected across product types, trader segments, or geographic regions
4. **Governance Approval**: AI Governance Committee approves progression to pilot deployment based on POC results

### 4.2 POC Scope & Constraints

**In Scope**:
- **Component 1**: Automated market data validation for FX, Fixed Income, Commodities (focus on high-volume products)
- **Component 2**: IRD email parsing for package trades and fee extraction
- **Component 3**: Cross-product pattern detection for Equities ↔ Equity Derivatives, Bonds ↔ Interest Rate Swaps
- **Component 4**: Risk scoring and triage for all exception types

**Out of Scope (for POC)**:
- Real-time integration with production case management system (manual upload/batch processing acceptable for POC)
- Advanced cross-product scenarios (e.g., multi-leg structured products, cryptocurrencies)
- Multi-language support (English emails only for POC)
- Full audit trail implementation (simplified logging for POC)

**Data Constraints**:
- **Historical data**: 2 years of OMRC exceptions (estimated 30,000-50,000 cases across all products)
- **Anonymization**: All data pseudonymized (trader IDs, counterparty IDs) for POC privacy compliance
- **Test environment**: Separate from production; no live data access during POC

### 4.3 POC Timeline: 12 Weeks

#### **Phase 1: Data Collection & Preparation (Weeks 1-3)**

**Week 1: Governance Kick-off & Planning**
- **Activities**:
  - AI Governance Committee approval meeting (present this proposal)
  - Model Inventory registration
  - Team formation:
    - 2 ML Engineers (NLP, ML modeling)
    - 1 Data Scientist (Risk scoring, explainability)
    - 1 Data Engineer (ETL pipelines, API integration)
    - 3-4 OMRC SMEs (part-time, 8-10 hours/week for data annotation and UAT)
  - Data access requests:
    - Historical exception data from surveillance system
    - Email mailbox access for IRD threads
    - Reuters/Bloomberg API credentials (read-only, test environment)
- **Deliverables**:
  - AI Governance approval confirmation
  - Model Inventory entry created
  - Team mobilized and kick-off meeting completed
  - Data access approvals in progress

**Week 2-3: Data Extraction, Anonymization & Quality Assessment**
- **Activities**:
  - Extract 2 years of OMRC exception data (all products)
  - Extract email threads for IRD exceptions (500-1000 email chains)
  - Fetch sample market data from Reuters/Bloomberg APIs (100 test queries)
  - **Data anonymization**:
    - Replace trader names with pseudonymized IDs (TRADER_001, TRADER_002, ...)
    - Replace counterparty names with pseudonymized IDs (COUNTERPARTY_A, COUNTERPARTY_B, ...)
    - Redact any PII from email text (names, phone numbers, personal identifiers)
  - **Data quality assessment**:
    - Check completeness: % of exceptions with closure outcomes, email threads, market data
    - Check accuracy: SME review of 300 sample cases (closure outcome labels, exception details)
    - Check consistency: Validate data formats across products and venues
  - **Data annotation** (for ML training):
    - **IRD emails**: 200 email threads annotated by SMEs with ground truth:
      - Sub-trade details (notional, tenor, rate, pay/receive)
      - Fees (upfront, ongoing, termination)
      - Justification quality (High/Medium/Low)
    - **Risk labels**: 500 exceptions annotated by SMEs with risk levels (Low/Medium/High/Violation)
- **Deliverables**:
  - Anonymized dataset (30,000-50,000 exceptions)
  - Data quality report (completeness, accuracy, distribution across products)
  - Annotated training data (200 IRD emails, 500 risk-labeled exceptions)
  - Annotation guidelines document

#### **Phase 2: Model Development (Weeks 4-8)**

**Week 4: Component 1 - Market Data Validation Engine**
- **Activities**:
  - **API integration**:
    - Develop connectors to Reuters Eikon API and Bloomberg Data License
    - Test API queries for different product types (FX forwards, bond yields, commodity prices, swap curves)
    - Handle API response parsing (extract reference rates, timestamps, bid-ask spreads)
  - **Product classification**:
    - Train NLP classifier to identify product type from exception description
    - Training data: Exception descriptions labeled with product types (500 samples)
  - **Rate reconciliation logic**:
    - Implement fuzzy timestamp matching (±1 minute window)
    - Calculate rate deviation (execution rate vs. reference rate in bps)
    - Implement bid-ask spread check (is execution within bid-ask bounds?)
  - **Anomaly detection**:
    - Train Isolation Forest model on historical rate deviations
    - Learn normal volatility regimes for each product type
    - Generate anomaly scores for rate deviations
- **Testing**:
  - Test on 100 sample exceptions with known market data issues
  - Validate accuracy of reference rate fetching (compare to manual analyst lookups)
- **Deliverables**:
  - Market data validation engine prototype
  - API integration code
  - Product classification model (accuracy >85%)
  - Anomaly detection model (AUC-ROC >0.80)

**Week 5: Component 2 - IRD Email Parser (Part 1: NER & Entity Extraction)**
- **Activities**:
  - **Email preprocessing**:
    - Email thread reconstruction (sort by timestamp, link replies)
    - Text cleaning (remove signatures, disclaimers, HTML tags)
    - Segment emails by sender role (trader, middle office, credit)
  - **Named Entity Recognition (NER)**:
    - Fine-tune FinBERT on financial entity corpus (use public datasets + 200 annotated OMRC emails)
    - Train NER to extract:
      - Trade IDs, notional amounts, currencies, tenors, rates (fixed/floating), pay/receive indicators
      - Fee amounts, fee types (upfront, ongoing, termination)
      - Counterparty names, dates (execution, effective, maturity)
    - Test NER on 50 held-out email threads
  - **Relation extraction**:
    - Develop rules + ML hybrid to link fees to sub-trades
    - Validate fee consistency (does sum of individual fees equal stated total fee?)
- **Testing**:
  - Evaluate NER precision and recall on 50 test email threads
  - Target: >85% precision, >85% recall for key entities (notional, rate, fee)
- **Deliverables**:
  - Email preprocessing pipeline
  - Fine-tuned FinBERT NER model
  - Relation extraction module
  - NER performance report (precision, recall by entity type)

**Week 6: Component 2 - IRD Email Parser (Part 2: Justification Assessment & Summarization)**
- **Activities**:
  - **Justification quality classifier**:
    - Train text classifier on annotated justifications (200 samples: High/Medium/Low quality)
    - Use FinBERT embeddings + logistic regression / SVM
    - Test on 50 held-out samples
  - **Sentiment analysis**:
    - Apply sentiment analysis to detect urgency/pressure language
    - Flag red flag phrases: "client threatening", "must approve ASAP", "urgent deal"
  - **Conversation summarization**:
    - Fine-tune abstractive summarization model (BART, T5) on email threads
    - Generate 3-5 sentence summaries of 10+ email chains
    - SME review of 20 generated summaries for quality
  - **Integration**:
    - Combine NER, relation extraction, justification classifier, summarization into end-to-end pipeline
    - Test on 50 complete email threads (input: email thread → output: structured JSON + summary)
- **Testing**:
  - Evaluate end-to-end pipeline on 50 IRD email threads
  - Measure: Extraction accuracy (>90% target), justification classification accuracy (>80%), summarization quality (SME review)
- **Deliverables**:
  - Justification quality classifier (accuracy >80%)
  - Sentiment analysis module
  - Conversation summarization model
  - End-to-end IRD email parser pipeline
  - Pipeline performance report

**Week 7: Component 3 - Cross-Product Pattern Detection**
- **Activities**:
  - **Data integration**:
    - Aggregate OMRC exceptions across all products
    - Enrich with trade data (order book activity if available)
    - Link to email threads (for context)
  - **Correlation matrix**:
    - Calculate historical correlations between products (Equities ↔ Equity Derivatives, Bonds ↔ IRD Swaps)
    - Identify high-correlation product pairs (correlation >0.70)
  - **Temporal pattern detection**:
    - Flag exceptions in correlated products occurring within ±15 minute window
    - Calculate clustering scores (how many exceptions from same trader in correlated products)
  - **Graph Neural Network (GNN)**:
    - Build trader-counterparty-product graph from historical data
    - Train GNN for anomaly detection (nodes: traders, counterparties, products; edges: trading relationships)
    - Test GNN on historical manipulation cases (if available) or synthetic scenarios
  - **Scenario-based detection**:
    - Implement detection rules for known manipulation scenarios (cross-product spoofing, layering)
    - Score each exception against scenarios
- **Testing**:
  - Validate on 2 years of historical data
  - SME review of top 20 flagged cross-product patterns (true manipulation vs. false positive)
  - Target: Identify 5+ legitimate suspicious patterns with <20% false positive rate
- **Deliverables**:
  - Cross-product correlation matrix
  - Temporal pattern detection module
  - GNN model for relationship anomaly detection
  - Scenario-based detection rules
  - Cross-product detection performance report (flagged patterns + SME validation)

**Week 8: Component 4 - Predictive Risk Scoring & Triage**
- **Activities**:
  - **Feature engineering**:
    - Create 100+ features from historical exception data:
      - Exception characteristics, trader features, counterparty features, temporal features
      - Email features (justification quality score from Component 2)
      - Market data features (volatility, bid-ask spread from Component 1)
      - Cross-product features (anomaly score from Component 3)
  - **Model training**:
    - Train XGBoost multi-class classifier on 70% of historical data (train set)
    - Classes: {Auto-Approve, Standard, Escalate, Violation}
    - Handle class imbalance with SMOTE for rare Violation class
    - Hyperparameter tuning with 5-fold cross-validation
  - **Threshold calibration**:
    - Map class probabilities to risk scores (0-100)
    - Tune thresholds to meet precision targets:
      - Auto-approve precision >95%
      - Violation recall >90%
  - **Explainability**:
    - Implement SHAP values for every risk score
    - Generate top 5 risk factors for each exception
- **Testing**:
  - Evaluate on 15% validation set and 15% hold-out test set
  - Measure: AUC-ROC, precision, recall, F1-score by category
  - Target: AUC-ROC >0.85, Auto-approve precision >95%, Violation recall >90%
- **Deliverables**:
  - Trained XGBoost risk scoring model
  - Feature engineering pipeline
  - Threshold calibration report
  - SHAP explainability module
  - Model performance report (metrics by category, confusion matrix)

#### **Phase 3: Integration, Explainability & Governance (Weeks 9-10)**

**Week 9: End-to-End Integration & Dashboard Development**
- **Activities**:
  - **System integration**:
    - Integrate all 4 components into unified pipeline:
      1. Exception ingestion (batch upload for POC)
      2. Component 1: Market data validation
      3. Component 2: IRD email parsing (if applicable)
      4. Component 3: Cross-product pattern detection
      5. Component 4: Risk scoring and triage
    - Implement orchestration logic (when to apply each component based on exception type)
  - **Dashboard/UI development** (prototype):
    - **Exception review screen** showing:
      - Exception details (trade ID, product, rate deviation, trader, counterparty)
      - Market data validation result (reference rate, deviation, validation decision)
      - IRD parsed data (structured trade details, fees, justification summary) - if IRD exception
      - Cross-product alert (if flagged)
      - Risk score (0-100) with top 5 factors (SHAP values)
      - Triage recommendation (Auto-Approve / Junior Queue / Senior Queue / Escalate)
      - Override option with rationale capture
    - **Monitoring dashboard**:
      - Daily auto-approval rate, analyst override rate
      - Model performance metrics (precision, recall by category)
  - **Testing**:
    - Run end-to-end pipeline on 100 recent exceptions (not in training data)
    - Validate that all components integrate correctly
    - SME review of 20 sample outputs for quality
- **Deliverables**:
  - End-to-end integrated ML system
  - Analyst-facing dashboard prototype
  - Integration test report (100 exceptions processed successfully)

**Week 10: Governance Review, Validation & Bias Testing**
- **Activities**:
  - **Model documentation**:
    - Complete model cards for all 4 components:
      - Model architecture, training methodology, data sources
      - Performance metrics, feature importance
      - Limitations, edge cases, assumptions
    - Develop validation documentation:
      - Outcomes analysis on hold-out test set
      - Sensitivity analysis (stressed scenarios)
      - Benchmarking vs. manual process
  - **Independent validation**:
    - Model Risk team review of model documentation
    - Validation of model performance claims (reproduce metrics on test set)
    - Review of model governance controls (explainability, audit trail, monitoring)
  - **Bias and fairness testing**:
    - Analyze model performance across segments:
      - Product types (Equities, FX, Fixed Income, IRD, Commodities)
      - Trader tenure (Junior, Mid, Senior)
      - Geographic regions (EMEA, APAC, Americas)
    - Calculate fairness metrics:
      - False positive rate by segment (target: <10% variance)
      - Precision by segment (target: similar across segments)
    - Generate bias testing report
  - **Compliance review**:
    - Legal and Compliance review of data governance and privacy controls
    - Validate GDPR compliance (legal basis, data minimization, pseudonymization, retention)
    - Review audit trail and logging mechanisms
- **Deliverables**:
  - Model cards for all 4 components
  - Independent validation report (Model Risk team)
  - Bias and fairness testing report
  - Compliance review sign-off
  - Updated Model Inventory with validation results

#### **Phase 4: User Acceptance Testing (Weeks 11-12)**

**Week 11: UAT with OMRC Analysts**
- **Activities**:
  - **UAT participants**: 5-6 OMRC analysts (mix of junior, mid, senior; representing different product specializations)
  - **UAT dataset**: 100 recent exceptions (not in training data) across all products:
    - 30 with market data issues (test Component 1)
    - 20 IRD package trades (test Component 2)
    - 10 with potential cross-product patterns (test Component 3)
    - 40 standard exceptions (test Component 4 risk scoring and triage)
  - **UAT process**:
    1. Analysts review each exception using ML system dashboard
    2. Compare ML outputs to their own assessment:
       - **Component 1**: Is market data validation decision correct? (validate vs. flag for review)
       - **Component 2**: Are extracted IRD trade details, fees, justifications accurate?
       - **Component 3**: Are cross-product alerts legitimate or false positives?
       - **Component 4**: Is risk score reasonable? Is triage recommendation appropriate?
    3. Capture analyst feedback via structured survey:
       - Usability: Is dashboard intuitive and easy to use? (1-5 scale)
       - Accuracy: Are ML outputs accurate and helpful? (1-5 scale)
       - Explainability: Are SHAP explanations clear and actionable? (1-5 scale)
       - Trust: Would you trust ML recommendations in production? (Yes/No/Maybe)
       - Free-text comments: Suggestions for improvement, concerns, positive feedback
  - **Feedback analysis**:
    - Quantitative: Calculate average scores for usability, accuracy, explainability, trust
    - Qualitative: Thematic analysis of free-text comments
    - Identify common issues, improvement areas, high-value features
- **Deliverables**:
  - UAT report with analyst feedback (quantitative scores + qualitative themes)
  - List of identified issues and enhancement requests
  - Recommendations for refinement

**Week 12: POC Evaluation, Go/No-Go Decision & Pilot Planning**
- **Activities**:
  - **Consolidate POC results**:
    - Compare POC outcomes to success criteria (Section 4.1)
    - Quantitative metrics: Automation rates, accuracy, time savings, AUC-ROC, precision, recall
    - Qualitative feedback: Analyst satisfaction, explainability, usability
  - **POC evaluation report**:
    - Executive summary: Key achievements, success criteria met/missed
    - Detailed results by component (Component 1-4 performance)
    - UAT feedback summary
    - Governance compliance: Validation, bias testing, data privacy
    - Lessons learned and challenges encountered
  - **Go/No-Go decision framework**:
    - **Go to Pilot**: If 80%+ of success criteria met, analyst satisfaction >75%, no major governance concerns
    - **Refine POC**: If 60-80% of criteria met; identify specific areas for improvement and conduct 4-week refinement sprint
    - **Discontinue**: If <60% of criteria met or fundamental technical/governance issues identified
  - **Final presentation to AI Governance Committee**:
    - Present POC results, UAT feedback, governance compliance
    - Recommendation: Proceed to Pilot / Refine POC / Discontinue
    - If approved, present pilot deployment plan (see Section 5)
  - **Document lessons learned**:
    - What worked well (e.g., NLP email parsing exceeded expectations)
    - What was challenging (e.g., market data API integration took longer than expected)
    - Recommendations for pilot phase (e.g., expand to more products, enhance cross-product detection)
- **Deliverables**:
  - POC Final Evaluation Report (comprehensive document with all results)
  - Go/No-Go recommendation for AI Governance Committee
  - Pilot deployment plan (if approved to proceed)
  - Lessons learned document

### 4.4 POC Resource Requirements

**Team** (12 weeks):
- **2 ML Engineers** (full-time): NLP model development, API integration, model deployment - $120K
- **1 Data Scientist** (full-time): Feature engineering, risk scoring, explainability, validation - $60K
- **1 Data Engineer** (part-time, 50% allocation): Data pipelines, ETL, API connectors - $30K
- **3-4 OMRC SMEs** (part-time, 8-10 hours/week): Data annotation, UAT, feedback - $15K
- **1 Model Risk Analyst** (part-time, 2 weeks): Independent validation - $10K
- **1 Compliance Officer** (part-time, 1 week): Privacy and regulatory review - $5K

**Total Personnel Cost**: ~$240K (internal team costs)

**Infrastructure** (12 weeks):
- **Cloud compute** (AWS SageMaker or Azure ML): GPU instances for model training - $5K
- **Data storage** (S3 or Azure Blob): Historical data, model artifacts - $1K
- **Reuters Eikon API** (test environment access): Market data queries - $3K
- **Bloomberg Data License** (test environment access): Derivatives pricing data - $3K
- **Development environment**: Test VPC, security controls - $2K

**Total Infrastructure Cost**: ~$14K

**Total POC Budget**: ~$254K (internal team costs + infrastructure)

**Note**: If using external consultants or vendors, costs would increase by 50-100%. This estimate assumes internal team with existing infrastructure access.

### 4.5 POC Risks & Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| **Insufficient historical data quality** | Medium | High | Week 2-3 data quality assessment; if <90% closure outcomes available, extend SME review to manually label 500+ additional cases |
| **Model accuracy below targets** | Medium | High | Week 8 interim checkpoint; if metrics <10% below targets, adjust model architecture or features; if fundamental issues, recommend POC refinement sprint |
| **Market data API integration challenges** | Medium | Medium | Allocate Week 4 buffer for API troubleshooting; if Reuters/Bloomberg APIs unavailable, use manual test data for POC and defer full integration to pilot |
| **Analyst resistance or low satisfaction** | Low | Medium | Early and frequent SME involvement (Weeks 2-3 annotation, Week 11 UAT); emphasize ML as decision support, not replacement; demonstrate time savings and workload reduction |
| **Data privacy or compliance concerns** | Low | High | Week 1 Legal/Compliance review; use pseudonymized data throughout POC; no production data access; address concerns before proceeding to Phase 2 |
| **POC timeline overrun** | Medium | Low | Weekly progress tracking; identify delays early; de-scope non-critical features if needed (e.g., defer advanced cross-product scenarios to pilot) |
| **Insufficient bias/fairness performance** | Low | Medium | Week 10 bias testing; if significant bias detected (>15% false positive variance across segments), apply fairness-aware training techniques or adjust model |

---

## 5. Post-POC: Pilot & Production Roadmap

**(Contingent on POC Success and AI Governance Committee Approval)**

### Phase 5: Pilot Deployment (3-4 Months Post-POC)

**Scope**:
- Deploy ML system in **test environment** with **live data feed** (read-only access to production exception data)
- **Parallel run**: ML system processes exceptions alongside manual analysts
  - Analysts review exceptions **without** ML assistance (to preserve baseline comparison)
  - ML system generates risk scores and triage recommendations **silently** (logged but not shown to analysts initially)
  - After 2 weeks baseline period, enable ML assistance for 50% of analysts (A/B test)
- **Gradual rollout**: Start with 30% of daily exceptions, then ramp to 50%, then 100% over 8 weeks
- **Weekly performance reviews**: Compare ML accuracy to manual analyst decisions
- **Monthly model retraining**: Incorporate new exceptions and closure outcomes into training data

**Success Criteria**:
- Maintain POC accuracy levels on live data (AUC-ROC >0.85, Auto-approve precision >95%, Violation recall >90%)
- Zero data privacy or security incidents
- >80% analyst adoption (analysts actively use ML risk scores and triage recommendations)
- Model drift detection: <5% degradation in performance month-over-month
- Business impact: 30-35% auto-approval rate achieved; same-day closure rate improvement to 70%+

**Governance**:
- **Monthly Model Risk reviews**: Performance metrics, drift detection, incident log
- **Quarterly independent validation**: Validate model accuracy on live data, review governance controls
- **Incident response plan**: Documented procedures for model failures, false approvals, data breaches

### Phase 6: Production Rollout (6-8 Months Post-POC)

**Scope**:
- **Full integration** with case management system (automated exception ingestion, triage routing, auto-approval workflow)
- **100% exception coverage**: All OMRC exceptions processed by ML system
- **Auto-approval workflow for low-risk cases**:
  - Exceptions with risk score 0-20 AND high model confidence (>95%) → Auto-approve (no analyst review)
  - Daily audit sampling: 10% of auto-approved cases reviewed by senior analyst (quality assurance)
- **Real-time monitoring dashboard** for Middle Office and Compliance:
  - Daily auto-approval rate, analyst override rate, model performance metrics
  - Cross-product alerts, high-risk escalations
  - Model drift alerts, bias metrics
- **Feedback loop**: Analyst overrides fed back into model retraining to improve accuracy

**Change Management**:
- **Training program** for all OMRC analysts (2-day workshop):
  - How ML system works (high-level overview)
  - How to interpret risk scores and SHAP explanations
  - When and how to override ML recommendations
  - Best practices for providing feedback
- **User guides and documentation**: Step-by-step guides for using ML dashboard, interpreting outputs, escalating issues
- **Helpdesk support**: Dedicated support team for 3 months post-launch (ML Engineers + OMRC SMEs)

**Continuous Improvement**:
- **Weekly model retraining**: Incorporate new exceptions, closure outcomes, analyst overrides into training data
- **Quarterly feature engineering reviews**: Add new risk indicators based on emerging manipulation tactics
- **Annual model refresh**: Retrain models from scratch with latest techniques (new NLP models, improved ML algorithms)
- **Expansion to new products**: Extend ML system to new asset classes (e.g., cryptocurrencies, ESG derivatives) as OMRC coverage expands

---

## 6. Business Impact & ROI

### 6.1 Quantitative Benefits (Annual, Post-Production)

| Metric | Current State | Target State (Year 1) | Improvement |
|--------|---------------|----------------------|-------------|
| **Exception Processing Time** | 15-20 min/exception (with market data/IRD issues) | 3-5 min/exception | **75% reduction** |
| **Market Data Validation Time** | 15-20 min/exception (30-40% of cases) | <2 min/exception | **90% reduction** |
| **IRD Email Parsing Time** | 20-30 min/exception (15-20% of cases) | <5 min/exception | **80% reduction** |
| **Daily Exception Throughput** | 60-80 exceptions/analyst | 150-200 exceptions/analyst | **2.5x increase** |
| **Auto-Approval Rate** | 0% | 30-35% | **50-100 exceptions/day** automated |
| **Same-Day Closure Rate** | 40-50% | 85%+ | **40-45% improvement** |
| **Data Validation Accuracy** | 85-90% (manual errors) | 98%+ (ML-assisted) | **10% improvement** |
| **Cross-Product Detection** | Reactive only (0 proactive cases/year) | 20-30 proactive alerts/year | **New capability** |
| **Analyst Time Savings** | - | 70-80 FTE hours/week | **1.5-2 FTE equivalent** |

### 6.2 Qualitative Benefits

1. **Enhanced Risk Detection**:
   - **Proactive cross-product surveillance**: Detect manipulation patterns that current systems miss
   - **Reduced regulatory violation risk**: Higher accuracy in identifying true violations (90%+ recall)
   - **Improved audit trail**: Explainable AI provides clear justifications for all decisions

2. **Operational Excellence**:
   - **Consistent exception processing**: Eliminate analyst-to-analyst variability in decision-making
   - **Scalability**: Handle growing exception volumes without proportional headcount increase
   - **Knowledge democratization**: Junior analysts can handle complex IRD cases with ML assistance
   - **Reduced analyst burnout**: Free up time from repetitive tasks for high-value investigations

3. **Compliance and Regulatory**:
   - **Demonstrate best-in-class surveillance**: Show regulators (FCA, SEC, CFTC) that firm uses advanced AI for market abuse detection
   - **Complete audit trail**: Every ML decision logged with inputs, outputs, explanations, analyst overrides
   - **Faster regulatory inquiries**: Rapidly retrieve and explain historical exception decisions using ML audit logs
   - **SLA compliance**: Meet regulatory and internal SLA targets for timely exception closure

4. **Strategic Value**:
   - **Foundation for AI expansion**: Prove ML value in OMRC → expand to other surveillance areas (trade reporting, transaction monitoring)
   - **Competitive positioning**: Benchmark against peers using RegTech platforms (SteelEye, Eventus, NICE Actimize)
   - **Insights into market trends**: ML models reveal patterns in trader behavior, market volatility, product trends
   - **Talent attraction/retention**: Modern AI/ML tools attract top surveillance talent; reduce analyst turnover

### 6.3 ROI Estimate (3-Year Projection)

**Year 1 Costs** (POC + Pilot + Production):
- POC development (12 weeks): $254K
- Pilot deployment (3-4 months): $150K (infrastructure scaling, additional testing, analyst training)
- Production rollout (6-8 months): $200K (full integration, change management, documentation)
- **Total Year 1**: ~$604K

**Annual Operating Costs** (Year 2+):
- Cloud infrastructure (AWS/Azure): $50K/year
- Reuters/Bloomberg API fees (production access): $40K/year
- Model maintenance and retraining (1 ML Engineer, 1 Data Scientist, 50% allocation): $80K/year
- Monitoring and governance (Model Risk, Compliance reviews): $30K/year
- **Total Annual Operating**: ~$200K/year

**Annual Benefits** (Estimated Savings + Risk Reduction):

1. **Analyst Time Savings**:
   - 70-80 FTE hours/week saved (from automation and efficiency gains)
   - 52 weeks/year × 75 hours/week = 3,900 hours/year
   - Loaded cost per hour: $100/hour (mid-level analyst salary + benefits)
   - **Annual savings: $390K**

2. **Error Reduction and Rework Avoidance**:
   - Reduce manual validation errors from 10-15% to <2% (8-13% improvement)
   - Estimated cost of errors (rework, regulatory findings, reputational damage): $150K/year
   - 8-13% improvement → **Annual savings: $12K-20K**

3. **Regulatory Risk Reduction**:
   - **Conservative estimate**: Reduce probability of regulatory enforcement action by 20-30%
   - Estimated cost of enforcement action (fines, remediation, legal): $1M-$5M (based on peer enforcement actions)
   - Expected value of risk reduction: 25% × $3M (midpoint) = **$750K**
   - **Annual risk reduction value: $750K** (conservative estimate; actual value could be higher)

4. **Headcount Avoidance**:
   - Without ML, OMRC would need 1-2 additional analysts to handle growing exception volumes
   - Cost per analyst (fully loaded): $150K-$200K/year
   - **Annual headcount avoidance: $150K-$200K**

**Total Annual Benefits**: $390K + $15K + $750K + $175K = **$1.33M**

**Net ROI**:
- **Year 1**: Benefits ($1.33M) - Costs ($604K) = **+$726K** (120% ROI)
- **Year 2**: Benefits ($1.33M) - Operating Costs ($200K) = **+$1.13M** (565% ROI)
- **Year 3**: Benefits ($1.33M) - Operating Costs ($200K) = **+$1.13M** (565% ROI)

**3-Year Cumulative ROI**: (~$726K + $1.13M + $1.13M) / ($604K + $200K + $200K) = **$2.99M / $1.00M** = **~300% ROI**

**Note**: This is a conservative estimate. Actual benefits could be higher due to:
- Additional risk mitigation (reputational damage, client retention)
- Expansion of ML to other surveillance areas (amplifying ROI)
- Faster response to regulatory changes (agility value)

---

## 7. AI Governance Approval Checklist

### Pre-POC Approval Requirements

For AI Governance Committee review, confirm:

- [ ] **Use Case Justification**: Clear articulation of OMRC-specific operational challenges and ML necessity (Section 1)
- [ ] **Solution Architecture**: Comprehensive design of 4 ML components with detailed techniques (Section 2)
- [ ] **Risk Classification**: High-risk AI system classification documented (Section 3.1)
- [ ] **Model Inventory Registration**: Model registered with complete metadata (Section 3.2)
- [ ] **Regulatory Compliance**: SR 11-7, EU AI Act, FCA AI Principles, IOSCO alignment (Section 3.1)
- [ ] **Explainability Approach**: SHAP values, model cards, audit trails defined (Section 3.3)
- [ ] **Bias & Fairness Plan**: Pre-deployment testing and ongoing monitoring (Section 3.4)
- [ ] **Data Governance**: GDPR legal basis, anonymization, retention policies (Section 3.5)
- [ ] **Vendor Risk Management**: Third-party vendors identified and risk-assessed (Section 3.6)
- [ ] **Model Documentation Plan**: Development, validation, monitoring documentation (Section 3.7)
- [ ] **POC Scope & Timeline**: 12-week plan with clear success criteria (Section 4)
- [ ] **Resource Approval**: Team and budget allocation (~$254K) confirmed
- [ ] **Risk Mitigation**: POC risks identified with mitigation plans (Section 4.5)
- [ ] **Business Case**: ROI projection and 3-year financial impact (Section 6)

### Post-POC Go/No-Go Review (Week 12)

- [ ] **Success Criteria Met**: 80%+ of quantitative and qualitative targets achieved (Section 4.1)
- [ ] **Model Validation**: Independent validation report completed and approved by Model Risk
- [ ] **Bias Testing**: No significant bias detected across product types, trader segments, geographic regions
- [ ] **Analyst Feedback**: >75% satisfaction with ML system; feedback incorporated into pilot plan
- [ ] **Governance Compliance**: All documentation, controls, and audit trails operational
- [ ] **Data Privacy**: Zero privacy incidents during POC; GDPR compliance validated
- [ ] **Pilot Plan**: Detailed pilot deployment roadmap prepared (Section 5)
- [ ] **Production Plan**: Change management, training, continuous improvement plans ready
- [ ] **Business Case Validation**: ROI projections validated with POC results; stakeholder buy-in confirmed

---

## 8. Conclusion & Recommendation

### Summary

This proposal seeks AI Governance Committee approval for a **12-week Proof of Concept (POC)** to develop an AI/ML solution addressing critical operational challenges faced by the **Market Security & Services (MSS) OMRC team**:

1. **Manual Market Data Validation**: Automate Reuters/Bloomberg lookups, saving 50-60 analyst hours/week
2. **Complex IRD Email Parsing**: Extract structured trade details from unstructured email threads, reducing parsing time by 75%
3. **Cross-Product Pattern Detection**: Proactively identify manipulation patterns across asset classes that current systems miss
4. **Predictive Risk Scoring & Triage**: Automate 30-35% of low-risk exceptions, improving same-day closure rate to 85%+

**Key Differentiators**:
- **Not a replacement for existing tools**: This ML solution addresses **genuine capability gaps** that SmartWorkbench and front office tools do not cover
- **MSS-specific challenges**: Designed for cross-product surveillance, centralized exception management, and high-volume workflows
- **Governance-first approach**: Comprehensive controls aligned to SR 11-7, EU AI Act, FCA AI Principles embedded from Day 1
- **Proven ROI**: **300% 3-year ROI** with $726K Year 1 net benefit and $1.13M annually thereafter

### Recommendation

**Approve Proof of Concept (POC) to commence immediately.**

**Rationale**:
1. **Well-Justified Use Case**: OMRC faces genuine operational challenges (manual market data validation, IRD email parsing, no cross-product detection, inefficient triage) that ML can address
2. **Comprehensive Solution**: 4 integrated ML components designed to fill specific capability gaps with measurable impact
3. **Robust Governance**: Compliance framework aligned to regulatory requirements (SR 11-7, EU AI Act, FCA); comprehensive validation, explainability, bias testing
4. **Structured POC Plan**: 12-week timeline with clear success criteria, phased approach, and risk mitigation
5. **Strong Business Case**: **$726K Year 1 net benefit**, **300% 3-year ROI**, operational efficiency gains, regulatory risk reduction
6. **Low Risk, High Value**: POC investment of ~$254K with controlled scope, test environment, and Go/No-Go decision point at Week 12

### Next Steps

**Immediate Actions (Weeks 1-2)**:
1. **AI Governance Committee approval meeting** (present this proposal)
2. **Model Inventory registration** (complete metadata entry)
3. **Team mobilization** (assign 2 ML Engineers, 1 Data Scientist, 1 Data Engineer, 3-4 OMRC SMEs)
4. **Data access approvals** (historical exceptions, email mailboxes, Reuters/Bloomberg API credentials)

**POC Execution (Weeks 2-12)**:
- Execute POC per timeline (Section 4.3)
- Weekly progress updates to AI Governance Committee sponsor
- Week 12: Return to AI Governance Committee with POC results and Go/No-Go recommendation for pilot

**Post-POC (Contingent on Success)**:
- **Pilot deployment** (3-4 months): Parallel run with live data, A/B testing, gradual rollout
- **Production rollout** (6-8 months): Full integration, auto-approval workflow, continuous improvement
- **Expansion** (Year 2+): Extend ML to other surveillance areas, new products, additional use cases

---

## Appendix A: Glossary

- **OMRC**: Off-Market Rate Checking - surveillance process to detect trades executed at rates significantly different from market reference rates
- **MSS**: Market Security & Services - centralized surveillance function responsible for cross-product exception monitoring
- **IRD**: Interest Rate Derivatives - swaps, swaptions, caps, floors, forward rate agreements
- **Package Trade**: Multiple linked trades executed as a single economic transaction (common in IRD)
- **Structured Trade**: Trade with complex fee arrangements, embedded options, or customized terms
- **Cross-Product Manipulation**: Market abuse where trader manipulates price in one product to benefit positions in correlated product
- **NLP**: Natural Language Processing - AI technique to understand and extract information from human language
- **NER**: Named Entity Recognition - NLP task to identify entities (amounts, dates, rates, counterparties) in text
- **FinBERT**: Financial domain-specific BERT model for NLP tasks
- **GNN**: Graph Neural Network - ML model for analyzing network relationships (traders, counterparties, products)
- **SHAP**: SHapley Additive exPlanations - method to explain individual ML predictions
- **SR 11-7**: Federal Reserve Supervisory Guidance on Model Risk Management
- **EU AI Act**: European Union regulation on high-risk AI systems
- **FCA**: Financial Conduct Authority (UK financial regulator)
- **IOSCO**: International Organization of Securities Commissions
- **AUC-ROC**: Area Under Receiver Operating Characteristic Curve - metric for binary classification accuracy
- **XGBoost**: Extreme Gradient Boosting - ensemble ML algorithm for supervised learning

---

## Appendix B: Contact Information

**Model Owner**: [Head of Market Security & Services - OMRC Function]  
**Business Sponsor**: [Chief Compliance Officer / Head of Surveillance]  
**AI/ML Project Lead**: [ML Engineering Team Lead]  
**OMRC Function Lead**: [OMRC Team Lead Name]  
**Compliance Liaison**: [Compliance Officer Name]  
**Model Risk Liaison**: [Model Risk Team Lead]

**For questions or additional information, contact**: [Project Lead Email]

---

**Document Version**: 1.0  
**Date**: November 6, 2025  
**Classification**: Internal - Restricted  
**Review Cycle**: Annual or upon material change  
**Next Review Date**: November 6, 2026
