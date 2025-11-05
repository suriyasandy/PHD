# ML Solution for OMRC Exception Management: AI Governance Approval & POC Proposal

## Executive Summary for AI Governance Committee

This proposal seeks approval to develop and pilot a **machine learning solution for automated OMRC (Off-Market Rate Checking) exception management**. While our existing SmartWorkbench tool handles structured data extraction from emails, tables, and images, it **cannot** perform the contextual analysis, risk prediction, and intelligent triage that ML enables.

**Purpose**: Obtain AI Governance Committee approval to proceed with a 12-week Proof of Concept (POC) that demonstrates ML's ability to:
1. Understand contextual intent in email conversations between front/middle office
2. Predict exception risk based on historical closure patterns
3. Automate alert triage and prioritization

**Regulatory Alignment**: SR 11-7, EU AI Act (High-Risk classification), FCA AI Principles, Model Risk Management Framework

---

## 1. Use Case Justification: Why ML is Necessary Beyond SmartWorkbench

### Current State: SmartWorkbench Capabilities
SmartWorkbench successfully:
- Processes large volumes of emails from shared mailboxes
- Extracts structured data from tables and images (trade confirmations, fee schedules)
- Applies rule-based extraction patterns
- Handles document classification

### The Gap: What SmartWorkbench Cannot Do

**1. Contextual Communication Analysis**
- **SmartWorkbench**: Extracts text but cannot understand conversational intent, sentiment, or nuanced language patterns
- **ML Requirement**: NLP models (BERT, FinBERT) to detect:
  - Unusual urgency or pressure in trader communications
  - Pre-arrangement language indicating collusion
  - Justification quality assessment (legitimate vs. suspicious)
  - Relationship dynamics between front office and middle office

**Example**: Email thread contains "client is extremely urgent on this", "can we push this through today?", "rate is slightly off but relationship critical". SmartWorkbench extracts these phrases but cannot assess that this combination indicates elevated risk requiring senior review.

**2. Predictive Risk Scoring from Historical Patterns**
- **SmartWorkbench**: No learning capability; cannot identify patterns across historical exceptions
- **ML Requirement**: Supervised learning models (XGBoost, Random Forest) to:
  - Score exceptions based on 2-3 years of closure outcomes (approved vs. escalated)
  - Detect trader behavioral anomalies (deviation from historical patterns)
  - Identify temporal clustering (multiple exceptions from same trader/counterparty)
  - Flag relationship risks (repeated off-market trades between specific parties)

**Example**: Trader X has 5 OMRC exceptions in 2 weeks with Counterparty Y, all with similar rate deviations. SmartWorkbench processes each independently. ML detects the pattern and auto-escalates for potential collusion investigation.

**3. Intelligent Alert Triage and Prioritization**
- **SmartWorkbench**: Processes exceptions sequentially without prioritization
- **ML Requirement**: Multi-class classification to:
  - Categorize exceptions by risk level (High/Medium/Low)
  - Auto-approve low-risk cases (30-40% reduction in manual workload)
  - Route high-risk cases to senior analysts with context
  - Provide explainable justifications for triage decisions

**Example**: POC target is to auto-approve 35% of exceptions with 95%+ accuracy, reducing analyst workload by 25 FTE hours/week.

### Business Impact Summary

| Capability | SmartWorkbench | ML Solution | Business Value |
|------------|----------------|-------------|----------------|
| **Data Extraction** | ✓ (rule-based) | ✓ (enhanced with NER) | Baseline capability |
| **Contextual Understanding** | ✗ | ✓ (NLP models) | Detect manipulation language, assess justification quality |
| **Risk Prediction** | ✗ | ✓ (supervised ML) | Proactive identification of high-risk exceptions |
| **Pattern Detection** | ✗ | ✓ (anomaly detection) | Uncover collusion networks, behavioral anomalies |
| **Alert Triage** | ✗ | ✓ (classification) | 60-70% reduction in manual effort |
| **Continuous Learning** | ✗ | ✓ (online learning) | Adapt to evolving manipulation tactics |

**Conclusion**: ML is not replacing SmartWorkbench but **augmenting** it with capabilities that are impossible with rule-based systems.

---

## 2. AI Governance Requirements & Compliance Framework

### 2.1 Regulatory Classification

**AI System Classification**: High-Risk AI System (EU AI Act Article 6)
- **Rationale**: Used for creditworthiness assessment and operational risk management in regulated financial institution
- **Implications**: Requires comprehensive risk assessment, human oversight, transparency, and ongoing monitoring

**Model Risk Management**: SR 11-7 Compliance Required
- **Model Definition**: Quantitative method, system, or approach that applies statistical, economic, financial, or mathematical theories, techniques, and assumptions to process input data into quantitative estimates
- **Applicability**: ML models for risk scoring and classification fall under SR 11-7

**FCA AI Principles Alignment**:
1. **Transparency**: Explainable AI (SHAP values) for risk score justifications
2. **Fairness**: Bias detection and mitigation across trader demographics
3. **Accountability**: Clear ownership (Middle Office Head)
4. **Security**: Data encryption, access controls, audit logging
5. **Redress**: Human-in-the-loop for high-risk escalations
6. **Data Governance**: Compliance with GDPR, data minimization

### 2.2 Model Inventory & Registration

Per AI Governance Policy, the following must be documented in the Model Inventory:

| Field | Details |
|-------|---------|
| **Model Name** | OMRC Exception Risk Scoring & Triage Model |
| **Model Owner** | Middle Office - OMRC Team Lead |
| **Business Sponsor** | Head of Middle Office |
| **Model Developer** | AI/ML Engineering Team (Internal) |
| **Risk Classification** | High-Risk (Tier 1) |
| **Use Case** | Automate OMRC exception risk assessment and alert prioritization |
| **Model Type** | Ensemble: NLP (FinBERT) + Supervised ML (XGBoost) + Multi-class Classifier |
| **Training Data** | 2-3 years of OMRC exceptions, email threads, closure outcomes (anonymized) |
| **Model Inputs** | Email text, trade details, trader/counterparty IDs, historical patterns |
| **Model Outputs** | Risk score (0-100), triage category (High/Med/Low/Auto-Approve), top risk factors |
| **Deployment Environment** | Cloud (AWS SageMaker) - Dedicated VPC with encryption |
| **Model Refresh Frequency** | Weekly retraining with new exception data |
| **Monitoring Metrics** | Precision, Recall, F1-score, AUC-ROC, False Positive Rate, Model Drift |
| **Validation Frequency** | Quarterly independent validation by Model Risk Team |
| **Human Oversight** | Mandatory review for High-risk exceptions (score >80); weekly audit sampling |
| **Compliance Requirements** | SR 11-7, EU AI Act, FCA AI Principles, GDPR, Model Risk Policy |

### 2.3 Three Lines of Defense

**First Line (Business Unit - Middle Office)**
- Model ownership and day-to-day operation
- User acceptance testing and feedback
- Incident reporting for model failures
- Monthly performance review

**Second Line (Risk & Compliance)**
- Model risk assessment and policy compliance
- Independent validation coordination
- Regulatory liaison and reporting
- Quarterly governance reviews

**Third Line (Internal Audit)**
- Annual audit of model governance
- Review of validation process
- Compliance with SR 11-7 and regulatory standards

### 2.4 Explainability & Transparency

**Challenge**: ML models (especially ensemble methods) can be "black boxes"

**Mitigation**:
1. **SHAP (SHapley Additive exPlanations)**: For each risk score, provide top 5 contributing factors
   - Example: "Risk Score 87/100. Top factors: (1) Trader has 4 similar exceptions in 10 days (+25 points), (2) Email contains urgency language (+18 points), (3) Rate deviation 2.5x trader average (+22 points)"

2. **Model Cards**: Comprehensive documentation including:
   - Model architecture and training methodology
   - Performance metrics by trader segment
   - Bias analysis (ensure no discrimination by trader location, seniority, etc.)
   - Limitations and edge cases

3. **Audit Trail**: Every model decision logged with:
   - Input data snapshot
   - Risk score and triage decision
   - SHAP values for explainability
   - Analyst override (if applicable) with rationale

**Transparency for Stakeholders**:
- **Traders/Sales**: General awareness that communications are analyzed for risk (via existing surveillance policy)
- **Analysts**: Full visibility into risk scores, explanations, and override capability
- **Auditors**: Complete access to model documentation, validation reports, and decision logs

### 2.5 Bias Detection & Fairness

**Risk**: Model may unfairly penalize certain traders, regions, or product types

**Mitigation**:
1. **Pre-deployment Bias Testing**:
   - Analyze model performance across trader demographics (region, tenure, product specialization)
   - Ensure false positive rates are similar across segments (max 10% variance)
   - Test for disparate impact (if model flags >20% more exceptions for any protected group, investigate)

2. **Ongoing Monitoring**:
   - Monthly fairness metrics in model monitoring dashboard
   - Quarterly review by Compliance and Model Risk teams
   - Immediate investigation if bias metrics deteriorate

3. **Fairness-Aware Training**:
   - Use fairness constraints during model training (e.g., equalized odds)
   - Separate model validation on minority segments

### 2.6 Data Governance & Privacy

**Data Sources**:
1. Email threads from shared OMRC mailboxes (text only, no attachments beyond what SmartWorkbench processes)
2. Trade surveillance system data (trade IDs, rates, counterparties)
3. Historical exception closure outcomes (approved, escalated, violation)
4. Analyst notes and rationale

**GDPR Compliance**:
- **Legal Basis**: Legitimate interest (operational risk management and regulatory compliance)
- **Data Minimization**: Only email text and metadata required; no personal trader information beyond employee IDs
- **Anonymization**: Trader names replaced with pseudonymized IDs in training data
- **Retention**: Training data retained for 3 years (aligned with regulatory record-keeping); model logs for 7 years
- **Access Controls**: Role-based access; only authorized Middle Office and Risk personnel

**Data Quality**:
- **Completeness**: Ensure 95%+ of exception emails have closure outcomes
- **Accuracy**: Validate closure outcome labels with SME review (sample 10% of historical data)
- **Consistency**: Standardize data formats across surveillance systems

### 2.7 Third-Party & Vendor Risk Management

**Vendor Tools** (if applicable):
- **Hugging Face Transformers** (NLP library): Open-source, internal deployment (no data shared with vendor)
- **AWS SageMaker** (ML platform): Cloud infrastructure; data encrypted at rest and in transit; AWS BAA in place
- **XGBoost** (ML algorithm): Open-source library, no vendor dependencies

**Vendor Due Diligence**:
- For AWS: Annual SOC 2 Type II audit review, security assessment
- For open-source libraries: Quarterly vulnerability scans, version updates

**No Third-Party Models**: All models developed internally; no reliance on external vendor black-box models

### 2.8 Model Documentation (SR 11-7 Requirements)

**Development Documentation**:
1. **Conceptual Soundness**:
   - Literature review of NLP and risk scoring in trade surveillance
   - Rationale for model architecture choices (why BERT for NLP, why XGBoost for risk scoring)
   - Assumptions and limitations clearly stated

2. **Data and Methodology**:
   - Description of training data sources, preprocessing steps
   - Feature engineering logic (e.g., how "urgency language" is quantified)
   - Model training process (hyperparameter tuning, cross-validation)
   - Performance metrics on test set

3. **Model Limitations**:
   - Cannot detect sophisticated manipulation if language is deliberately neutral
   - Performance degrades for rare exception types (<1% of training data)
   - Requires retraining if market conditions change significantly (e.g., new product types)

**Validation Documentation**:
1. **Outcomes Analysis**: Compare model predictions to actual outcomes on hold-out test set
   - Target: 90%+ precision for High-risk category, 95%+ precision for Auto-approve category
2. **Sensitivity Analysis**: Test model performance under stressed scenarios (e.g., market volatility, new trader cohort)
3. **Benchmarking**: Compare to baseline (current manual process accuracy ~85-90%)

**Ongoing Monitoring Documentation**:
1. **Monthly Reports**: Model performance metrics, drift detection, incident log
2. **Quarterly Reviews**: Validation team review, bias analysis, fairness metrics
3. **Annual Audit**: Internal Audit assessment of model governance

---

## 3. Proof of Concept (POC) Plan: 12-Week Roadmap

### 3.1 POC Objectives

**Primary Goal**: Demonstrate that ML can improve OMRC exception management efficiency and accuracy beyond SmartWorkbench capabilities

**Success Criteria** (Quantitative):
1. **Risk Scoring Accuracy**: AUC-ROC >0.85 on test set (predicting escalated vs. approved exceptions)
2. **Auto-Approval Precision**: >95% precision for Auto-approve category (false positive rate <5%)
3. **Processing Time**: <3 minutes per exception (vs. 15-20 minutes manual)
4. **Workload Reduction**: Achieve 30-35% auto-approval rate (baseline: 0%)
5. **Analyst Satisfaction**: >75% of Middle Office analysts find ML risk scores helpful (survey)

**Success Criteria** (Qualitative):
1. Model explanations (SHAP values) are clear and actionable for analysts
2. No significant bias detected across trader segments
3. AI Governance Committee approves progression to pilot deployment
4. Compliance and Model Risk teams validate governance framework

### 3.2 POC Scope & Constraints

**In Scope**:
- Email text analysis for contextual understanding (NLP)
- Risk scoring model based on historical exception outcomes
- Alert triage classification (High/Medium/Low/Auto-approve)
- Explainability dashboard for analysts

**Out of Scope (for POC)**:
- Integration with case management system (manual upload for POC)
- Real-time processing (batch processing acceptable for POC)
- Advanced collusion network analysis (Phase 2 post-POC)
- Multi-language support (English emails only)

**Data Constraints**:
- 2 years of historical OMRC exceptions (estimated 2,000-3,000 cases)
- Anonymized dataset for POC (no production access)
- Test environment separate from production infrastructure

### 3.3 POC Timeline: 12 Weeks

#### **Phase 1: Data Collection & Preparation (Weeks 1-3)**

**Week 1: Governance Kick-off**
- AI Governance Committee approval meeting
- Model Inventory registration
- Team formation (2 ML Engineers, 1 Data Scientist, 3 Middle Office SMEs part-time)
- Data access requests and approvals

**Week 2-3: Data Gathering & Annotation**
- Extract 2 years of exception data from shared mailboxes and surveillance system
- SME review of 300 cases for data quality validation (closure outcome accuracy)
- Data anonymization (pseudonymize trader IDs, remove PII)
- Annotate 200 exceptions for training labels:
  - Closure outcome: Approved / Escalated / Violation
  - Risk level (SME-assessed): High / Medium / Low
  - Key risk factors (SME notes)

**Deliverables**:
- Anonymized dataset (2,000-3,000 exceptions)
- Data quality report (completeness, accuracy, distribution)
- Annotation guideline document

#### **Phase 2: Model Development (Weeks 4-8)**

**Week 4-5: NLP Model Training**
- Fine-tune FinBERT on financial email corpus (100K emails from public datasets)
- Train Named Entity Recognition (NER) for trade details extraction
- Develop sentiment analysis and intent classification models
- Test on 50 sample OMRC emails for contextual understanding accuracy

**Week 6-7: Risk Scoring Model Training**
- Feature engineering (50+ features):
  - Email features: urgency score, justification quality, email chain length
  - Trader features: historical exception rate, rate deviation from average, tenure
  - Temporal features: time since last exception, clustering score
  - Relationship features: counterparty recurrence, cross-product patterns
- Train XGBoost model on 70% of historical data (train set)
- Hyperparameter tuning with 5-fold cross-validation
- Test on 15% validation set for model selection
- Reserve 15% hold-out test set for final evaluation

**Week 8: Triage Classification Model**
- Multi-class classifier to map risk scores to categories:
  - High (score 80-100): Mandatory senior analyst review
  - Medium (score 50-79): Standard analyst queue
  - Low (score 20-49): Junior analyst or automated review
  - Auto-approve (score 0-19): No manual review required
- Threshold tuning to meet precision targets (>95% for Auto-approve)

**Deliverables**:
- Trained NLP models (FinBERT, NER, sentiment analysis)
- Trained risk scoring model (XGBoost)
- Triage classification model
- Model performance report (precision, recall, F1, AUC-ROC)

#### **Phase 3: Explainability & Governance (Weeks 9-10)**

**Week 9: Explainability Implementation**
- Implement SHAP values for risk score explanations
- Develop model cards documenting:
  - Model architecture and training process
  - Performance metrics by trader segment
  - Limitations and edge cases
  - Bias analysis results
- Create analyst-facing dashboard mock-up showing:
  - Exception details (extracted by SmartWorkbench + ML enhancements)
  - Risk score with top 5 contributing factors
  - Triage category and recommended action
  - Override option with rationale capture

**Week 10: Governance Review**
- Model Risk Team independent validation (outcomes analysis, sensitivity testing)
- Bias and fairness analysis across trader segments
- Compliance review of data governance and privacy controls
- Update Model Inventory with validation results

**Deliverables**:
- SHAP explainability module
- Model cards (comprehensive documentation)
- Analyst dashboard prototype
- Independent validation report

#### **Phase 4: User Acceptance Testing (Weeks 11-12)**

**Week 11: UAT with Middle Office Analysts**
- 5 Middle Office analysts test ML system on 100 recent exceptions (not in training data)
- Analysts review:
  - Risk scores and explanations (are they intuitive and accurate?)
  - Triage recommendations (do they agree with ML categorization?)
  - Dashboard usability
- Collect feedback via structured survey and interviews

**Week 12: POC Evaluation & Go/No-Go Decision**
- Consolidate UAT feedback and performance metrics
- Compare POC results to success criteria
- Final presentation to AI Governance Committee:
  - Quantitative results (accuracy, efficiency gains)
  - Qualitative feedback (analyst satisfaction, explainability)
  - Governance compliance (validation, bias testing, documentation)
  - Recommendation: Proceed to Pilot / Refine POC / Discontinue
- Document lessons learned and next steps

**Deliverables**:
- UAT report with analyst feedback
- POC final evaluation report
- Go/No-Go recommendation for AI Governance Committee
- Pilot deployment plan (if approved)

### 3.4 POC Resource Requirements

**Team**:
- 2 ML Engineers (full-time, 12 weeks) - Model development, deployment
- 1 Data Scientist (full-time, 12 weeks) - Feature engineering, explainability, validation
- 1 Data Engineer (part-time, 4 weeks) - Data pipeline, anonymization
- 3 Middle Office SMEs (part-time, 8 hours/week) - Data annotation, UAT, feedback
- 1 Model Risk Analyst (part-time, 2 weeks) - Independent validation
- 1 Compliance Officer (part-time, 1 week) - Privacy and regulatory review

**Infrastructure**:
- AWS SageMaker (GPU instances for model training): $3,000
- Data storage (S3): $500
- Development environment (test VPC): $1,000
- **Total Infrastructure**: $4,500

**Total POC Budget**: $85,000 - $100,000 (internal team costs + infrastructure)

### 3.5 POC Risks & Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| **Insufficient training data quality** | Medium | High | Week 2 data quality validation; if <90% closure outcome accuracy, extend SME review to 500 cases |
| **Model accuracy below targets** | Medium | High | Fallback to simpler models (logistic regression); adjust success criteria if business value still demonstrated |
| **Analyst resistance to ML** | Low | Medium | Early and frequent SME involvement; emphasize ML as decision support, not replacement |
| **Data privacy concerns** | Low | High | Legal and Compliance review in Week 1; use anonymized data; no production data access for POC |
| **POC timeline overrun** | Medium | Low | Weekly progress tracking; buffer in Phase 4 for contingency |

---

## 4. Post-POC: Pilot & Production Roadmap (Contingent on POC Success)

### Phase 5: Pilot Deployment (3-4 Months Post-POC)

**Scope**:
- Deploy ML model in test environment with live data feed (read-only)
- Parallel run: ML processes exceptions alongside manual analysts
- 20% of daily exceptions routed to ML triage (remaining 80% manual)
- Weekly performance reviews and model retraining

**Success Criteria**:
- Maintain POC accuracy levels on live data (AUC-ROC >0.85, Auto-approve precision >95%)
- Zero data privacy incidents
- >80% analyst adoption (analysts use ML scores to inform decisions)
- Model drift detection <5% monthly (performance stable over time)

**Governance**:
- Monthly Model Risk reviews
- Quarterly independent validation
- Incident response plan for model failures

### Phase 6: Production Rollout (6-8 Months Post-POC)

**Scope**:
- Full integration with case management system
- 100% of OMRC exceptions processed by ML
- Auto-approval workflow for low-risk cases (with daily audit sampling)
- Real-time monitoring dashboard for Middle Office and Risk teams

**Change Management**:
- Training program for all Middle Office analysts (2-day workshop)
- User guides and documentation
- Helpdesk support for 3 months post-launch

**Continuous Improvement**:
- Weekly model retraining with new exception data
- Quarterly feature engineering reviews (add new risk indicators)
- Annual model refresh (retrain from scratch with latest techniques)

---

## 5. AI Governance Committee Approval Checklist

For AI Governance Committee review, the following must be confirmed:

### Pre-POC Approval Requirements

- [ ] **Use Case Justification**: Clear articulation of ML necessity beyond SmartWorkbench (Section 1)
- [ ] **Risk Classification**: High-risk AI system classification documented (Section 2.1)
- [ ] **Model Inventory Registration**: Model registered with complete metadata (Section 2.2)
- [ ] **Regulatory Compliance**: SR 11-7, EU AI Act, FCA principles alignment confirmed (Section 2.1)
- [ ] **Data Governance**: GDPR legal basis, data minimization, anonymization plan (Section 2.6)
- [ ] **Explainability Approach**: SHAP values, model cards, transparency mechanisms (Section 2.4)
- [ ] **Bias & Fairness Plan**: Pre-deployment testing and ongoing monitoring (Section 2.5)
- [ ] **Three Lines of Defense**: Roles and responsibilities defined (Section 2.3)
- [ ] **Model Documentation**: Development, validation, monitoring documentation plan (Section 2.8)
- [ ] **POC Scope & Timeline**: 12-week plan with clear success criteria (Section 3)
- [ ] **Resource Approval**: Team and budget allocation ($85K-$100K) confirmed
- [ ] **Risk Mitigation**: POC risks identified with mitigation plans (Section 3.5)

### Post-POC Go/No-Go Review (Week 12)

- [ ] **Success Criteria Met**: Quantitative and qualitative targets achieved (Section 3.1)
- [ ] **Model Validation**: Independent validation report completed and approved
- [ ] **Bias Testing**: No significant bias detected across trader segments
- [ ] **Analyst Feedback**: >75% satisfaction with ML system
- [ ] **Governance Compliance**: All documentation and controls in place
- [ ] **Pilot Plan**: Detailed pilot deployment roadmap prepared (Section 4)
- [ ] **Business Case**: ROI projection and resource requirements for pilot/production

---

## 6. Conclusion & Recommendation

**Summary**:
This proposal seeks AI Governance Committee approval for a 12-week POC to demonstrate that machine learning can enhance OMRC exception management in ways that SmartWorkbench fundamentally cannot:
- **Contextual understanding** of email communications to detect risk signals
- **Predictive risk scoring** based on historical patterns and behavioral anomalies
- **Intelligent triage** to automate 30-35% of low-risk exceptions

**Governance Compliance**:
The POC is designed with comprehensive controls aligned to SR 11-7, EU AI Act, and FCA AI principles. All requirements for model inventory, explainability, bias testing, and documentation are embedded from Day 1.

**Business Value**:
If successful, this ML solution will:
- Reduce analyst workload by 60-70% (25 FTE hours/week)
- Improve risk detection accuracy by 25% (90-95% vs. 70% manual)
- Reduce false positives by 50% (25-35% vs. 60-70%)
- Provide scalable foundation for expanding ML to other surveillance areas

**Next Steps**:
1. **AI Governance Committee approval** to proceed with POC (this proposal)
2. **Week 1**: Model Inventory registration, team mobilization, data access approvals
3. **Weeks 2-12**: Execute POC per timeline (Section 3.3)
4. **Week 12**: Return to AI Governance Committee with results and Go/No-Go recommendation for pilot

**Recommendation**: **Approve POC to commence immediately.** The use case is well-justified, governance framework is robust, and POC is designed for rapid validation with controlled risk.

---

## Appendix A: Glossary

- **OMRC**: Off-Market Rate Checking - surveillance process to detect trades executed at rates significantly different from market reference rates
- **SmartWorkbench**: Existing rule-based tool for email processing and structured data extraction
- **NLP**: Natural Language Processing - AI technique to understand human language
- **FinBERT**: Financial domain-specific BERT model for NLP tasks
- **SHAP**: SHapley Additive exPlanations - method to explain individual ML predictions
- **SR 11-7**: Federal Reserve Supervisory Guidance on Model Risk Management
- **EU AI Act**: European Union regulation on high-risk AI systems
- **FCA**: Financial Conduct Authority (UK regulator)
- **AUC-ROC**: Area Under Receiver Operating Characteristic Curve - metric for binary classification accuracy
- **XGBoost**: Extreme Gradient Boosting - ensemble ML algorithm for supervised learning

---

## Appendix B: Contact Information

**Model Owner**: [Middle Office - OMRC Team Lead Name]  
**Business Sponsor**: [Head of Middle Office Name]  
**AI/ML Project Lead**: [ML Engineering Team Lead Name]  
**Compliance Liaison**: [Compliance Officer Name]  
**Model Risk Liaison**: [Model Risk Team Lead Name]

**For questions or additional information, contact**: [Project Lead Email]

---

**Document Version**: 1.0  
**Date**: November 6, 2025  
**Classification**: Internal - Restricted  
**Review Cycle**: Annual or upon material change
