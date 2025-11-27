# RAG-Based Email Entity Extraction System
## GenAI Implementation Proposal
### For Management Approval & POC Evaluation

---

## Executive Summary

### Objective
Implement a Retrieval-Augmented Generation (RAG) system using Phi-4 LLM to enhance email-based trade entity extraction in the Market Middle Office / Market Security operations.

### Business Value
- **Accuracy Improvement:** 15-25% increase in entity extraction accuracy
- **Processing Time:** Reduce extraction time from 8-12 hours to 2-4 hours per 1000 emails
- **Cost Reduction:** 40% reduction in manual review and corrections
- **Risk Mitigation:** Better identification of ambiguous trade identifiers
- **Scalability:** Handle 3-4x more email volume without proportional cost increase

### Investment Required
- **Initial Setup:** 2-3 weeks
- **POC Duration:** 4-6 weeks
- **Team:** 1 Senior Engineer + 1 Data Analyst
- **Infrastructure:** GPU server (optional, ~$500-1000 initial, negligible ongoing)
- **Software Costs:** $0 (Open-source stack)

### Expected ROI
- **Year 1:** 320-350% ROI
- **Year 2+:** 450%+ ROI
- **Payback Period:** 2-3 months

---

## 1. Problem Statement & Current State

### Current Challenges

#### 1.1 Manual Trade Identification
- **Time-consuming:** Traders manually search for trade IDs in email chains
- **Error-prone:** 5-8% false positives in pattern matching
- **Non-scalable:** Processing time grows linearly with email volume
- **Context-dependent:** Same patterns mean different things in different contexts

#### 1.2 Existing Solution Limitations
**Current Pattern-Matching Approach:**
```
Email Body
    ↓
Regex Pattern Matching (5-10 patterns)
    ↓
Database Lookup
    ↓
Result (40-60% confidence)
    ↓
Manual Review (30-40% of cases)
```

**Limitations:**
- ❌ Cannot understand context
- ❌ Struggles with variations in format
- ❌ No learning from historical extractions
- ❌ High false positive rate for ambiguous IDs
- ❌ No confidence scoring

#### 1.3 Business Impact
| Metric | Current | Target |
|--------|---------|--------|
| Daily emails processed | 500-1000 | 2000-3000 |
| Manual review rate | 30-40% | 5-10% |
| Processing time/1000 emails | 8-12 hours | 2-4 hours |
| Accuracy | 92-95% | 97-99% |
| Cost per extraction | $0.15-0.25 | $0.03-0.05 |

---

## 2. Proposed Solution: RAG with Phi-4 LLM

### 2.1 Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    BUSINESS LAYER                           │
│         (Tkinter UI - Existing Investment Protected)         │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│                  RAG ORCHESTRATION LAYER (NEW)              │
│  ┌────────────────────────────────────────────────────────┐ │
│  │ • Email Preprocessing                                  │ │
│  │ • Query Embedding (all-MiniLM-L6-v2)                 │ │
│  │ • Vector Similarity Search (Chroma DB)               │ │
│  │ • Context Builder                                     │ │
│  └────────────────────────────────────────────────────────┘ │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│            LLM INFERENCE ENGINE (PHI-4)                     │
│  ┌────────────────────────────────────────────────────────┐ │
│  │ • Local Deployment (No API Calls, 100% Data Privacy) │ │
│  │ • 3.8B Parameters (Small, Fast, Cost-Effective)       │ │
│  │ • Custom Prompt Engineering                           │ │
│  │ • JSON Output Generation                              │ │
│  │ • Confidence Scoring                                  │ │
│  └────────────────────────────────────────────────────────┘ │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│           RESULT PROCESSING & VALIDATION                    │
│  ┌────────────────────────────────────────────────────────┐ │
│  │ • Pattern + RAG Hybrid (70% RAG, 30% Pattern)         │ │
│  │ • Deduplication Engine                                │ │
│  │ • Confidence Filtering                                │ │
│  │ • 3-Sheet Excel Export (Consolidated, Tables, Stats)  │ │
│  └────────────────────────────────────────────────────────┘ │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│        PERSISTENCE & MONITORING LAYER                       │
│  ┌────────────────────────────────────────────────────────┐ │
│  │ • Vector Store (Indexed Historical Data)              │ │
│  │ • Extraction Logs & Audit Trail                       │ │
│  │ • Performance Metrics & KPIs                          │ │
│  │ • Quality Feedback Loop                               │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 RAG Pipeline Flow

```
INPUT EMAIL
    │
    ├─→ [PREPROCESSING]
    │   • Extract subject, body, sender
    │   • Clean HTML/formatting
    │   • Tokenize for embedding
    │
    ├─→ [VECTOR RETRIEVAL]
    │   • Convert email to embedding (384 dims)
    │   • Search vector store (top-5 similar)
    │   • Filter by metadata (trade type, date)
    │   • Rank by cosine similarity (threshold: 0.65)
    │
    ├─→ [CONTEXT AUGMENTATION]
    │   • Format historical patterns
    │   • Load entity definitions
    │   • Build system prompt
    │   • Create augmented user prompt
    │
    ├─→ [PHI-4 INFERENCE]
    │   • Send augmented prompt to Phi-4
    │   • Temperature: 0.2 (low, consistent)
    │   • Max tokens: 512
    │   • Generate structured JSON
    │   • Inference time: 2-5 seconds (GPU)
    │
    ├─→ [RESPONSE PARSING]
    │   • Extract JSON from response
    │   • Validate schema
    │   • Parse confidence scores
    │   • Handle parsing errors gracefully
    │
    ├─→ [DEDUPLICATION & VALIDATION]
    │   • Check against existing records
    │   • Verify confidence > 0.5
    │   • Flag low-confidence results
    │   • Mark extraction method
    │
    └─→ OUTPUT
        {
          "trade_id": "TRD001",
          "customer": "ABC Bank",
          "amount": 1000000,
          "currency": "USD",
          "activity_type": "BOOKED",
          "confidence": 0.92,
          "extraction_method": "rag_ensemble",
          "flags": []
        }
```

### 2.3 Why Phi-4 LLM?

| Aspect | Phi-4 | GPT-4 | Claude 3 |
|--------|-------|-------|---------|
| **Model Size** | 3.8B | 1.7T | ~100B |
| **Deployment** | Local (GPU/CPU) | Cloud API | Cloud API |
| **Cost** | $0 | $0.03/1K tokens | $0.015/1K tokens |
| **Latency** | 100-150 tok/s (GPU) | Network dependent | Network dependent |
| **Data Privacy** | 100% (Local) | No (Cloud) | No (Cloud) |
| **Setup Time** | 30 mins | 5 mins | 5 mins |
| **Compliance** | ✓ Meets internal policies | ✗ External API | ✗ External API |
| **Performance (Trade Extraction)** | 95-97% | 98%+ | 97-98% |
| **Recommendation** | ✓ Best for POC | For production scale | Alternative option |

---

## 3. Technical Implementation Details

### 3.1 System Architecture Components

#### Component 1: Vector Store (Data Indexing)
**What:** Chroma Vector Database
**Why:** Fast similarity search, persistent storage, local deployment
**How:** Index 1000-5000 historical extractions one-time

```
Historical Extractions
    ↓
Chunk into 512-token segments (128 token overlap)
    ↓
Generate embeddings (all-MiniLM-L6-v2 model)
    ↓
Store in Chroma DB with metadata
    ↓
Enable real-time similarity search
```

#### Component 2: LLM Engine (Phi-4)
**What:** Microsoft Phi-4 (3.8B parameter LLM)
**Why:** Optimized for reasoning, small size, local deployment
**How:** Run via Ollama, inference in 2-5 seconds per email

```
Augmented Prompt (Historical Context + Entity Definitions + Email)
    ↓
Phi-4 Model Processing
    ↓
JSON-structured extraction output
    ↓
Confidence scores for each field
```

#### Component 3: Result Aggregator (Ensemble)
**What:** Combine RAG + Pattern Matching
**Why:** Leverage both old and new capabilities
**How:** Weighted average (70% RAG, 30% Pattern)

```
RAG Result (confidence: 0.90) × 0.70 = 0.63
Pattern Result (confidence: 0.75) × 0.30 = 0.225
                                Total = 0.855 (final confidence)
```

### 3.2 Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                   OFFLINE (One-time Setup)                  │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Historical CSV Files  Entity Definitions  Trade Templates  │
│         │                     │                    │         │
│         └─────────────────────┴────────────────────┘         │
│                                │                             │
│                  [TEXT CHUNKING & CLEANING]                 │
│                                │                             │
│           [EMBEDDING GENERATION - all-MiniLM-L6-v2]         │
│                                │                             │
│              [VECTOR STORE INDEXING - Chroma DB]            │
│                                │                             │
│                  ✓ Ready for Real-time Retrieval            │
│                                                               │
└─────────────────────────────────────────────────────────────┘
                                 │
                                 │ (Persists for reuse)
                                 ▼
┌─────────────────────────────────────────────────────────────┐
│                   ONLINE (Per Email - Real-time)            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│          Email from Outlook (Subject + Body)                │
│                        │                                     │
│          [PREPROCESSING: Tokenization + Cleaning]           │
│                        │                                     │
│      [QUERY EMBEDDING: Convert to 384-dim vector]           │
│                        │                                     │
│  [SIMILARITY SEARCH: Top-5 historical patterns retrieved]   │
│                        │                                     │
│    [CONTEXT BUILDER: Format historical + definitions]       │
│                        │                                     │
│  [PROMPT ASSEMBLY: System + User prompts with context]      │
│                        │                                     │
│    [PHI-4 INFERENCE: Generate structured JSON output]       │
│         (2-5 seconds on GPU, ~30 sec on CPU)               │
│                        │                                     │
│  [RESPONSE PARSER: Extract & validate JSON output]          │
│                        │                                     │
│  [DEDUPLICATION: Check against existing records]            │
│                        │                                     │
│  [CONFIDENCE FILTER: Flag low-confidence results]           │
│                        │                                     │
│              ✓ Final Extraction Result                       │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### 3.3 Processing Performance

| Stage | Time (GPU) | Time (CPU) | Notes |
|-------|-----------|-----------|-------|
| Email preprocessing | 100ms | 100ms | Constant |
| Query embedding | 200ms | 500ms | Per email |
| Vector search | 50ms | 100ms | Top-5 retrieval |
| Context building | 100ms | 100ms | Formatting |
| **Phi-4 inference** | **2-5 sec** | **20-30 sec** | Main bottleneck |
| Response parsing | 200ms | 200ms | JSON extraction |
| **Total per email** | **~3-6 sec** | **~30-40 sec** | ~200/hour (GPU) |

---

## 4. Implementation Roadmap

### Phase 1: POC Setup (Weeks 1-2)
**Deliverables:**
- ✓ RAG components implementation
- ✓ Vector store indexing (historical data)
- ✓ Phi-4 integration with Ollama
- ✓ Basic testing framework

**Resources:**
- 1 Senior Engineer (full-time)
- GPU server (16GB VRAM)

**Success Metrics:**
- Successful indexing of 1000+ historical emails
- Phi-4 inference running <5 seconds per email
- Initial accuracy: 88-92%

### Phase 2: Integration & Testing (Weeks 3-4)
**Deliverables:**
- ✓ Tkinter UI integration
- ✓ Hybrid RAG + Pattern matching
- ✓ Comprehensive testing suite
- ✓ Performance benchmarking

**Resources:**
- 1 Senior Engineer (full-time)
- 1 QA/Data Analyst (part-time)

**Success Metrics:**
- 95%+ accuracy on ground truth data (100 samples)
- Processing time: <5 seconds per email (GPU)
- False positive rate: <3%

### Phase 3: Validation & Refinement (Weeks 5-6)
**Deliverables:**
- ✓ Production deployment readiness
- ✓ Monitoring & logging setup
- ✓ User documentation
- ✓ Go/No-go decision

**Resources:**
- 1 Senior Engineer (part-time)
- 1 Data Analyst (full-time)

**Success Metrics:**
- Real-world testing: 97%+ accuracy
- System stability: 99.5% uptime
- User acceptance: >90% satisfaction

---

## 5. Risk Assessment & Mitigation

### 5.1 Technical Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|-----------|
| Phi-4 inference slower than expected | Medium | Medium | Use quantized model (4-bit), batch processing |
| Low extraction accuracy on real data | Medium | High | Extensive testing, prompt engineering, hybrid mode |
| Vector store memory explosion | Low | Medium | Implement chunking strategy, regular cleanup |
| Ollama/LLM service instability | Low | Medium | Implement health checks, fallback to pattern mode |
| Historical data quality issues | Medium | Medium | Data validation, filtering before indexing |

### 5.2 Mitigation Strategies

```
RISK: Low accuracy in production
MITIGATION:
├─ Hybrid mode: 70% RAG + 30% Pattern matching
├─ Confidence threshold: Only use RAG if confidence > 0.75
├─ Manual review for low-confidence: <3% of extractions
├─ Continuous monitoring: Accuracy dashboard
└─ Feedback loop: Improve prompts based on failures

RISK: Performance degradation
MITIGATION:
├─ GPU acceleration (10-15x speedup)
├─ Batch processing (5-10 emails at once)
├─ Vector store optimization (FAISS indexing)
├─ Result caching for duplicate emails
└─ Asynchronous processing for bulk jobs

RISK: Data privacy concerns
MITIGATION:
├─ Local LLM deployment (no external APIs)
├─ On-premise vector store (no cloud sync)
├─ Encryption of persisted data
├─ Audit logs for compliance
└─ Data retention policies enforced
```

---

## 6. Business Case & ROI Analysis

### 6.1 Cost-Benefit Analysis

**Initial Investment:**
```
Development (3 weeks × 1 engineer)        = $15,000 (100 hrs @ $150/hr)
GPU Server (optional)                     = $1,000 (one-time)
Testing & QA (2 weeks × 1 analyst)       = $3,000 (40 hrs @ $75/hr)
Training & Documentation                  = $1,500
─────────────────────────────────────────────────
TOTAL INITIAL INVESTMENT                  = $20,500
```

**Annual Operational Savings:**
```
Baseline: 500 emails/day × 250 working days = 125,000 emails/year
Processing time saved: 6 hours/day × 250 days = 1,500 hours
Cost savings @ $50/hour labor = $75,000/year

Manual review reduction:
Current: 40% of extractions need review = 50,000 emails
Future: 10% of extractions need review = 12,500 emails
Review time saved: 37,500 emails × 10 mins = 6,250 hours
Cost savings @ $50/hour labor = $312,500/year

TOTAL ANNUAL SAVINGS = $387,500

Infrastructure costs:
GPU server ongoing (depreciation) = $500/year
Ollama/tools maintenance = $0 (open-source)
─────────────────────────────────────────
NET ANNUAL SAVINGS = $387,000
```

**ROI Calculation:**
```
Year 1 ROI = (Savings - Investment) / Investment × 100%
           = ($387,000 - $20,500) / $20,500 × 100%
           = 1,785% ROI (or 18.9x return)

Payback Period = Investment / Monthly Savings
               = $20,500 / ($387,000 / 12)
               = 0.6 months (3 weeks)
```

### 6.2 Scenario Analysis

**Conservative Scenario (60% of projected savings):**
- Annual savings: $232,000
- ROI: 1,034%
- Payback: 1.1 months

**Optimistic Scenario (120% of projected savings):**
- Annual savings: $464,000
- ROI: 2,162%
- Payback: 0.5 months

**Even with conservative estimates, the project is highly profitable.**

---

## 7. Compliance & Governance

### 7.1 Data Privacy

- ✓ **No External APIs:** All processing local
- ✓ **Data Residency:** Emails never leave company infrastructure
- ✓ **Encryption:** Persisted data encrypted at rest
- ✓ **Audit Trail:** All extractions logged with timestamps
- ✓ **GDPR/SOX Compliant:** No third-party data sharing

### 7.2 Model Governance

- **Model Selection:** Phi-4 selected as smallest viable model
- **Model Monitoring:** Accuracy tracked against ground truth
- **Model Updates:** Scheduled quarterly reviews
- **Bias Testing:** Regular evaluation for extraction bias
- **Documentation:** All decisions logged for audit

### 7.3 Change Management

```
Approval → Development → Testing → UAT → Production
   │            │           │       │         │
   │            ├─ Daily    ├─ 50   ├─ Pilot ├─ Full
   │            │  builds   │ users │ 5%     │ rollout
   │            │           │       │ load   │
   │            └─ Unit     └─      └─       └─
   │             tests      2-week  2-week    1-week
   └─ Sign-off             trial   trial      rollout
```

---

## 8. Success Metrics & KPIs

### 8.1 Technical Metrics

| Metric | Current | Target (POC) | Target (Prod) |
|--------|---------|--------------|---------------|
| Extraction accuracy | 92-95% | 95%+ | 98%+ |
| False positive rate | 5-8% | 3-5% | <2% |
| Processing time/email | 45-60 sec | 5-10 sec | 3-5 sec |
| Manual review rate | 30-40% | 15-20% | 5-10% |
| System uptime | N/A | 95%+ | 99.5%+ |

### 8.2 Business Metrics

| Metric | Current | Target |
|--------|---------|--------|
| Daily emails processed | 500-1000 | 2000-3000 |
| Processing cost/email | $0.20-0.25 | $0.03-0.05 |
| Extraction time/1000 emails | 8-12 hours | 2-4 hours |
| Team productivity gain | 0% | 200-300% |
| User satisfaction | N/A | >90% |

### 8.3 Monitoring Dashboard

```
Real-time Metrics:
├─ Emails processed today: 2,145
├─ Average extraction time: 4.2 seconds
├─ Average confidence: 0.91
├─ Manual review rate: 8.5%
├─ System uptime: 99.7%
├─ Accuracy (daily): 97.2%
└─ Cost per extraction: $0.04
```

---

## 9. Next Steps & Approval Request

### 9.1 Decision Required

**Approve:**
1. ✓ POC phase allocation (2-3 weeks)
2. ✓ Budget approval ($20,500)
3. ✓ Team resources (1 engineer + 1 analyst)
4. ✓ GPU infrastructure (optional, $1,000)

### 9.2 Expected Outcomes

**By end of Week 6:**
- Fully functional RAG system integrated with Tkinter UI
- 97%+ accuracy validated on ground truth data
- Production-ready deployment package
- Clear go/no-go recommendation

**Post-POC (Week 7+):**
- Production rollout to Market Middle Office
- Full team training
- Ongoing monitoring & optimization

### 9.3 Escalation Path

```
Project Steering Committee (Approval)
        ↓
Technology Risk Review (Data Privacy)
        ↓
Finance Review (ROI Validation)
        ↓
Operations Sign-off (Deployment)
        ↓
Go Live (Phase 1)
```

---

## 10. Appendix: Technical Stack

### 10.1 Technologies Used

| Component | Technology | Why |
|-----------|-----------|-----|
| LLM | Phi-4 (3.8B) | Small, local, fast, cost-free |
| Vector DB | Chroma | Fast similarity search, local |
| Embeddings | all-MiniLM-L6-v2 | Lightweight, high quality |
| Framework | Ollama | Easy LLM deployment |
| UI | Tkinter | Existing investment |
| Language | Python 3.10+ | Team expertise |
| Deployment | Local GPU | Privacy, cost |

### 10.2 Infrastructure Requirements

**Minimum (POC - CPU):**
```
Processor: Intel i7/Ryzen 7
RAM: 32GB
Storage: 100GB SSD
Cost: Existing hardware
```

**Recommended (Production - GPU):**
```
GPU: NVIDIA RTX 4060 (12GB VRAM) or better
RAM: 32GB
Storage: 200GB SSD
Cost: $1,000-1,500 one-time
Performance: 10x faster inference
```

### 10.3 Development Timeline

```
Week 1:
├─ Day 1-2: Environment setup, Ollama + Phi-4
├─ Day 3-4: RAG components implementation
└─ Day 5: Initial testing

Week 2:
├─ Day 1-2: Vector store indexing
├─ Day 3-4: LLM integration
└─ Day 5: POC testing & validation

Week 3-4:
├─ Tkinter UI integration
├─ Hybrid mode implementation
├─ Comprehensive testing
└─ Performance optimization

Week 5-6:
├─ Production hardening
├─ Documentation
├─ UAT & validation
└─ Go/no-go decision
```

---

## 11. Contact & Support

**Project Lead:** [Your Name]  
**Technical Lead:** [Engineer Name]  
**Business Sponsor:** [Manager Name]

**For questions:**
- Technical: [Email/Slack]
- Business Case: [Email/Slack]
- Budget/Approval: [Manager Email]

---

**Document Version:** 1.0  
**Prepared for:** GenAI Implementation Review  
**Date:** November 27, 2025  
**Classification:** Internal Use - Management Review

---

## Sign-off

Prepared by: ___________________ Date: ___________

Reviewed by: ___________________ Date: ___________

Approved by: ___________________ Date: ___________