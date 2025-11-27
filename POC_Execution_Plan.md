# RAG-Based Email Entity Extraction
## POC Execution Plan & Detailed Specifications

---

## Document Overview

This document provides:
- ✓ Detailed POC execution roadmap
- ✓ Technical specifications for each component
- ✓ Testing strategy and acceptance criteria
- ✓ Risk mitigation procedures
- ✓ Go/No-go decision criteria

**Timeline:** 6 weeks | **Team:** 1 Engineer + 1 Analyst | **Budget:** $20,500

---

## Phase 1: Setup & Baseline (Week 1-2)

### Week 1: Environment & Infrastructure

#### Day 1-2: Infrastructure Setup
```
Activities:
├─ Provision GPU server (AWS EC2/On-prem)
├─ Install base OS (Ubuntu 20.04 LTS or Windows Server)
├─ Install Python 3.10+
├─ Create project directory structure
└─ Set up Git repository

Deliverables:
├─ Clean Python environment
├─ Virtual environment configured
├─ GPU drivers installed & verified
└─ Ollama running on local machine
```

**Success Criteria:**
- ✓ `python --version` shows 3.10+
- ✓ GPU detected and working: `nvidia-smi` shows VRAM
- ✓ Ollama service running: `curl http://localhost:11434/api/tags`

#### Day 3-4: Phi-4 LLM Setup
```bash
# Install Ollama
curl https://ollama.ai/install.sh | sh

# Pull Phi-4 model
ollama pull phi4

# Verify model
ollama list  # Should show phi4

# Test inference
curl http://localhost:11434/api/generate \
  -d '{"model":"phi4","prompt":"test","stream":false}'
```

**Success Criteria:**
- ✓ Phi-4 model downloaded (~2.5GB)
- ✓ Test inference completes in <10 seconds
- ✓ JSON response received with generated text

#### Day 5: Data Preparation
```
Activities:
├─ Export historical trade extractions (CSV)
├─ Extract 1000-5000 historical emails
├─ Prepare entity definitions JSON
├─ Create test dataset (100 emails with ground truth)
└─ Document data schema
```

**Deliverables:**
- `historical_extractions.csv` (1000+ rows)
- `entity_definitions.json`
- `test_emails.csv` (100 with ground truth)

### Week 2: Core RAG Components

#### Day 1: Vector Store Setup
```python
# Create VectorStoreManager
from rag_components import VectorStoreManager

# Initialize
vs = VectorStoreManager(
    model_name="all-MiniLM-L6-v2",
    persist_dir="./vectorstore"
)

# Index historical data
vs.index_documents(documents, metadata)

# Verify indexing
results = vs.retrieve("test query", k=5)
print(f"✓ Retrieved {len(results)} similar documents")
```

**Acceptance Criteria:**
- ✓ Vector store created with 1000+ documents
- ✓ Similarity search working (<500ms)
- ✓ Metadata preserved in results
- ✓ Persistent storage verified

#### Day 2: Phi-4 Integration
```python
from rag_components import Phi4Extractor

# Initialize
phi4 = Phi4Extractor()

# Test extraction
context = {
    "historical_patterns": [...],
    "entity_definitions": {...},
    "subject": "Test email",
    "sender": "trader@example.com",
    "date": "2025-11-27"
}

result = phi4.extract("Email body text", context)
print(json.dumps(result, indent=2))
```

**Acceptance Criteria:**
- ✓ Phi-4 inference <5 seconds
- ✓ JSON output valid and parseable
- ✓ Confidence scores generated (0.0-1.0)
- ✓ Error handling working (retries, fallbacks)

#### Day 3: RAG Orchestrator
```python
from rag_components import RAGOrchestrator

# Initialize
rag = RAGOrchestrator(vs, phi4)

# Test full pipeline
email_doc = EmailDocument(...)
result = rag.process_email(email_doc)

# Verify output structure
assert result.get('success') in [True, False]
assert 'confidence' in result
assert 'extraction_method' in result
```

**Acceptance Criteria:**
- ✓ End-to-end pipeline working
- ✓ Output schema consistent
- ✓ Performance <8 seconds per email
- ✓ Logging functional

#### Day 4-5: Testing & Validation
```
Activities:
├─ Unit tests for each component
├─ Integration tests for full pipeline
├─ Performance benchmarking
├─ Error handling verification
└─ Documentation update

Tests to run:
├─ Vector retrieval: 100 queries, verify accuracy
├─ LLM inference: 10 extractions, verify JSON validity
├─ Full pipeline: 50 emails, measure accuracy
└─ Error scenarios: Network issues, model overload
```

**Test Report Format:**
```json
{
  "vector_retrieval": {
    "tests_run": 100,
    "passed": 100,
    "avg_time_ms": 120,
    "accuracy": "100%"
  },
  "phi4_inference": {
    "tests_run": 10,
    "passed": 10,
    "avg_time_sec": 3.2,
    "json_valid": "100%"
  },
  "full_pipeline": {
    "tests_run": 50,
    "passed": 50,
    "avg_accuracy": "88.5%",
    "status": "PASS"
  }
}
```

**Deliverables:**
- Complete test suite
- Performance baseline report
- Component documentation
- Known issues & workarounds

---

## Phase 2: Enhancement & Integration (Week 3-4)

### Week 3: Tkinter UI Integration

#### Day 1-2: UI Component Integration
```python
# Integrate RAG with existing Tkinter app
from tkinter_rag_integration import OutlookEmailHarvesterWithRAG

class EnhancedApp(OutlookEmailHarvesterWithRAG):
    def __init__(self, root):
        super().__init__(root, enable_rag=True)
        self.rag_pipeline = initialize_rag_pipeline()
```

**Modifications to Existing Code:**
- ✓ Add RAG initialization on app startup
- ✓ Add extraction method selector (RAG/Pattern/Hybrid)
- ✓ Display confidence scores in tree
- ✓ Add RAG results tab

#### Day 3-4: Hybrid Mode Implementation
```python
from rag_components import ResultAggregator

# Initialize aggregator
aggregator = ResultAggregator(
    weights={"rag": 0.7, "pattern": 0.3}
)

# For each email, aggregate results
rag_result = rag.process_email(email_doc)
pattern_result = pattern_extractor.extract(email_data)
final_result = aggregator.aggregate(rag_result, pattern_result)

# Use final_result with merged confidence
```

**Acceptance Criteria:**
- ✓ UI responsive during extraction
- ✓ Hybrid mode working correctly
- ✓ Confidence scores displayed
- ✓ Export includes method used

#### Day 5: Comprehensive Testing
```
Test Scenarios:
├─ UI responsiveness with 100 emails
├─ Thread safety (concurrent extractions)
├─ Error handling (API down, out of memory)
├─ Hybrid vs RAG-only comparison
└─ Export functionality (3-sheet Excel)

Performance Benchmarks:
├─ Single email extraction: <6 sec
├─ 10-email batch: <60 sec (avg 6 sec/email)
├─ 100-email batch: Memory usage <2GB
└─ UI remains responsive: <100ms latency
```

### Week 4: Accuracy & Performance Optimization

#### Day 1-2: Accuracy Testing
```
Ground Truth Dataset:
├─ 100 emails with manually verified extractions
├─ Coverage: diverse trade types, formats, edge cases
├─ Metrics: precision, recall, F1-score

Test Procedure:
1. Run RAG extraction on 100 emails
2. Compare against ground truth
3. Calculate accuracy per field
4. Identify failure patterns
5. Generate accuracy report

Expected Results:
├─ Trade ID accuracy: 96%+
├─ Amount accuracy: 95%+
├─ Activity type: 98%+
├─ Overall: 95%+
```

**Accuracy Report Template:**
```
Field               | Extracted | Correct | Accuracy | Confidence
─────────────────────────────────────────────────────────────────
Trade ID            | 100       | 96      | 96.0%    | 0.92
Customer            | 99        | 95      | 95.0%    | 0.88
Amount              | 100       | 95      | 95.0%    | 0.90
Currency            | 100       | 100     | 100%     | 0.99
Activity Type       | 98        | 96      | 97.9%    | 0.94
─────────────────────────────────────────────────────────────────
OVERALL             | 497       | 482     | 96.9%    | 0.93
```

#### Day 3: Performance Optimization
```python
# Profile code
import cProfile
import pstats

profiler = cProfile.Profile()
profiler.enable()

# Run extraction
for email in batch:
    rag.process_email(email)

profiler.disable()
stats = pstats.Stats(profiler)
stats.sort_stats('cumulative')
stats.print_stats(20)  # Top 20 slowest functions
```

**Optimization Options:**
```
If inference too slow:
├─ Use GPU: 10-15x faster
├─ Quantize model: 4-bit GGUF (~3GB vs 8GB)
├─ Batch processing: 5-10 emails together
└─ Async processing: Non-blocking extraction

If memory usage high:
├─ Reduce vector store size: Cache frequently used
├─ Limit batch size: Process 5 emails at a time
├─ Clear cache periodically: After 100 extractions
└─ Use lighter embedding model: bge-small-en

If accuracy insufficient:
├─ Improve historical data: Better examples
├─ Refine prompts: Better instructions for Phi-4
├─ Increase top-K: Retrieve 10 instead of 5
└─ Lower confidence threshold: Accept 0.6 vs 0.75
```

#### Day 4-5: UAT Preparation
```
UAT Scope:
├─ End-to-end workflow (fetch → extract → export)
├─ 100 real emails from production
├─ Comparison with manual extraction
├─ Performance measurement
├─ User feedback collection

UAT Success Criteria:
├─ Accuracy ≥ 95% on real data
├─ Processing time ≤ 5 sec/email (GPU)
├─ Zero critical bugs
├─ User satisfaction ≥ 80%
├─ All edge cases handled
```

---

## Phase 3: Validation & Production Prep (Week 5-6)

### Week 5: User Acceptance Testing (UAT)

#### Day 1-2: UAT Execution
```
UAT Plan:
1. Select 2-3 business users
2. Provide training on new features
3. Run on 50-100 real production emails
4. Collect feedback and issues
5. Document any bugs or feature requests

Testing Checklist:
├─ [ ] Extraction accuracy acceptable
├─ [ ] Processing speed acceptable
├─ [ ] UI intuitive and responsive
├─ [ ] Export format correct
├─ [ ] No critical bugs found
├─ [ ] Error messages clear
├─ [ ] Documentation sufficient
└─ [ ] User satisfied with results
```

**UAT Report Format:**
```
UAT Completion Report
═══════════════════════════════════════════════════════════════
Date: 2025-12-12
Testers: John Smith, Jane Doe, Mike Johnson
Emails Tested: 75
Duration: 2 hours

RESULTS
───────────────────────────────────────────────────────────────
Extraction Accuracy: 96.5% (PASS - Target: 95%)
Processing Time: 4.2 sec/email (PASS - Target: 5 sec)
Manual Review Needed: 8.5% (PASS - Target: 10%)
Critical Issues Found: 0 (PASS)
Major Issues Found: 2 (Minor: 5)

ISSUES
───────────────────────────────────────────────────────────────
[✓ RESOLVED] Low confidence for ambiguous trade IDs
[✓ RESOLVED] Export format didn't match expected columns

SIGN-OFF
───────────────────────────────────────────────────────────────
Business Approval: _____________________ Date: ________
Technical Lead: _____________________ Date: ________
```

#### Day 3-4: Bug Fixes & Documentation
```
Bug Fix Process:
1. Categorize issues (Critical/Major/Minor)
2. Prioritize by impact
3. Fix and test
4. Verify with users
5. Document workarounds if needed

Documentation Requirements:
├─ User Guide (how to use new features)
├─ Administrator Guide (setup, monitoring)
├─ API Documentation (technical reference)
├─ Troubleshooting Guide (common issues)
└─ FAQ
```

#### Day 5: Go/No-Go Decision
```
Decision Criteria Matrix:
┌────────────────────────┬─────────────┬──────────────┐
│ Criterion              │ Target      │ Actual       │
├────────────────────────┼─────────────┼──────────────┤
│ Accuracy               │ ≥ 95%       │ 96.5%  ✓     │
│ Processing Time        │ ≤ 5 sec     │ 4.2 sec ✓    │
│ Manual Review Rate     │ ≤ 10%       │ 8.5%  ✓      │
│ Critical Bugs          │ = 0         │ 0      ✓     │
│ User Satisfaction      │ ≥ 80%       │ 92%    ✓     │
│ System Stability       │ ≥ 95%       │ 99.2%  ✓     │
│ Data Privacy           │ Compliant   │ Yes    ✓     │
└────────────────────────┴─────────────┴──────────────┘

Decision: GO TO PRODUCTION ✓
```

### Week 6: Production Deployment

#### Day 1: Production Environment Prep
```
Pre-deployment Checklist:
├─ [ ] Environment mirrors UAT
├─ [ ] Vector store backed up
├─ [ ] Monitoring configured
├─ [ ] Rollback plan documented
├─ [ ] Team trained on procedures
├─ [ ] Support contacts assigned
├─ [ ] Logging aggregation ready
└─ [ ] Incident response plan activated
```

#### Day 2-3: Pilot Deployment
```
Pilot Phase:
├─ Deploy to 1 team (5-10 users)
├─ Monitor for 48 hours
├─ Collect feedback
├─ Make any urgent fixes
├─ Get team sign-off

Monitoring Metrics:
├─ Extraction accuracy (daily)
├─ Processing performance (real-time)
├─ Error rates (<1%)
├─ User feedback (daily surveys)
└─ System health (uptime, memory)
```

#### Day 4: Full Production Rollout
```
Rollout Plan:
├─ 08:00 - Communication to all users
├─ 08:30 - Deploy to production
├─ 09:00 - Verification by team
├─ 09:30 - Open to Market Middle Office
├─ 10:00 - Open to Market Security
├─ 10:30 - Monitor for issues

Rollout Success Criteria:
├─ Zero critical incidents in first 24 hours
├─ Accuracy maintained ≥ 95%
├─ Processing time < 5 seconds
├─ Zero unplanned downtime
├─ User feedback positive
```

#### Day 5: Handoff & Knowledge Transfer
```
Handoff Activities:
├─ Knowledge transfer to operations team
├─ Documentation finalization
├─ Monitoring dashboard setup
├─ Support escalation procedures
├─ Lessons learned documentation

Post-Deployment Review:
├─ Week 1: Daily check-ins
├─ Week 2-4: Weekly review
├─ Month 2+: Monthly review
├─ Quarterly: Full performance audit
```

---

## Testing Strategy

### Unit Testing

```python
# test_vector_store.py
def test_vector_indexing():
    vs = VectorStoreManager()
    docs = ["Test 1", "Test 2", "Test 3"]
    meta = [{"id": i} for i in range(3)]
    vs.index_documents(docs, meta)
    assert vs.collection.count() >= 3

def test_similarity_search():
    vs = VectorStoreManager()
    results = vs.retrieve("test query", k=5)
    assert len(results) <= 5
    assert all(r[2] >= 0.65 for r in results)  # Similarity score
```

### Integration Testing

```python
# test_rag_pipeline.py
def test_end_to_end_extraction():
    rag, vs, phi4 = initialize_rag_pipeline()
    email = create_test_email()
    result = rag.process_email(email)
    
    assert result.get('success') is True
    assert 'trade_id' in result
    assert 0 <= result.get('confidence', 0) <= 1.0
    assert result['total_time'] < 8  # seconds
```

### Performance Testing

```python
# test_performance.py
def test_batch_processing():
    rag, _, _ = initialize_rag_pipeline()
    emails = [create_test_email() for _ in range(100)]
    
    start = time.time()
    results = [rag.process_email(e) for e in emails]
    elapsed = time.time() - start
    
    avg_time = elapsed / 100
    assert avg_time < 6  # seconds per email
    assert all(r.get('success') for r in results)
```

---

## Monitoring & Alerting

### Metrics to Track

```
Real-time Metrics:
├─ Extraction accuracy (daily average)
├─ Processing time per email
├─ Manual review rate
├─ System uptime
├─ Error rate
├─ Average confidence score
└─ User satisfaction (surveys)

Alerting Thresholds:
├─ Accuracy < 94%: ALERT
├─ Processing time > 8 sec: WARNING
├─ Manual review rate > 15%: WARNING
├─ Error rate > 2%: ALERT
├─ Uptime < 95%: ALERT
└─ No updates > 4 hours: WARNING
```

### Dashboard (Grafana/Custom)

```
┌─────────────────────────────────────────────┐
│  RAG Extraction System - Daily Dashboard    │
├─────────────────────────────────────────────┤
│                                             │
│ Accuracy:  96.5%  ████████████ [TARGET 95%]│
│ Processing: 4.2s  ██████ [TARGET 5s]       │
│ Manual Review: 8.5% ████ [TARGET 10%]      │
│ Uptime: 99.7%  ███████████ [TARGET 95%]    │
│                                             │
│ Today: 2,145 emails processed               │
│ Errors: 8 (0.4%)                           │
│ Avg Confidence: 0.91                        │
│ User Satisfaction: 94%                      │
│                                             │
│ Last Updated: 14:32 UTC                     │
└─────────────────────────────────────────────┘
```

---

## Success Criteria & Go/No-Go Decision

### POC Success Criteria (Week 6)

```
Requirement              | Minimum    | Target     | Status
────────────────────────────────────────────────────────
Accuracy                 | 93%        | 95%+       | [✓ PASS]
Processing Time          | 8 sec      | 5 sec      | [✓ PASS]
Manual Review Rate       | 15%        | 10%        | [✓ PASS]
System Uptime            | 90%        | 95%+       | [✓ PASS]
User Satisfaction        | 70%        | 80%+       | [✓ PASS]
Data Privacy             | Compliant  | Compliant  | [✓ PASS]
Critical Bugs            | 0          | 0          | [✓ PASS]
Documentation Complete   | Yes        | Yes        | [✓ PASS]
────────────────────────────────────────────────────────
OVERALL: GO TO PRODUCTION
```

### Production Decision Matrix

```
✓ GO TO PRODUCTION if:
  • Accuracy ≥ 95%
  • Processing time ≤ 5 sec
  • Manual review rate ≤ 10%
  • Critical bugs = 0
  • User satisfaction ≥ 80%
  • Data privacy compliant

✗ NO-GO if:
  • Any critical blocker uncovered
  • Accuracy < 92%
  • Processing time > 8 sec
  • Security/compliance issues
  • Major data loss risk
```

---

## Rollback Plan

### If Production Issues Found

```
Severity Level | Action
──────────────────────────────────────
CRITICAL       | Immediate rollback to pattern-only mode
               | Impact: 30-40% manual review (temporary)
               | Recovery: <15 minutes
               
MAJOR          | Disable RAG, continue with patterns
               | Fix identified issue
               | Re-deploy after validation
               
MINOR          | Monitor closely
               | Deploy patch in next cycle
               | Workaround provided
```

### Rollback Procedure

```bash
# 1. Stop RAG service
systemctl stop rag-service

# 2. Restore vector store from backup
cp -r vectorstore.backup_20251212 vectorstore

# 3. Revert Tkinter app to pattern-only mode
git checkout app-tag-pattern-only

# 4. Restart with fallback
python main.py --rag-disabled

# 5. Verify system
curl http://localhost:5000/health  # Should return 200
```

---

## Post-Deployment Support Plan

### Week 1: Intense Monitoring
```
Daily Activities:
├─ 09:00 - Check overnight logs
├─ 12:00 - Mid-day review
├─ 17:00 - End-of-day summary
└─ 24/7 - On-call for critical issues

Review Points:
├─ Accuracy trending
├─ User feedback
├─ Performance metrics
├─ Error patterns
└─ Improvement opportunities
```

### Week 2-4: Transition to Normal Support
```
├─ Daily: Automated metric collection
├─ Weekly: Performance review meeting
├─ Monthly: Full system audit
└─ Quarterly: Strategic planning
```

### Continuous Improvement
```
Feedback Loop:
1. Collect user feedback
2. Analyze extraction failures
3. Identify improvement opportunities
4. Update prompts/thresholds
5. A/B test improvements
6. Deploy improvements
7. Measure impact
8. Repeat

Expected Improvements Over Time:
├─ Month 1: 0-2% accuracy gain
├─ Month 2: 1-3% accuracy gain
├─ Month 3: 0-2% accuracy gain
├─ Plateau after 3-4 months
```

---

## Acceptance & Sign-off

### By Project Stakeholders

```
Technical Lead (Engineering):
✓ Code quality acceptable
✓ Architecture sound
✓ Performance acceptable
✓ Documentation complete

Name: _________________________ Date: _________


Business Sponsor (Market Middle Office):
✓ ROI justified
✓ Functionality meets needs
✓ Accuracy acceptable
✓ Ready for production

Name: _________________________ Date: _________


Data/Security Officer:
✓ Data privacy compliant
✓ Security controls in place
✓ Audit trail implemented
✓ Compliance verified

Name: _________________________ Date: _________


Operations (IT):
✓ Infrastructure in place
✓ Monitoring configured
✓ Support procedures documented
✓ Incident response ready

Name: _________________________ Date: _________


Executive Sponsor:
✓ POC successful
✓ Business case validated
✓ Authorization for full deployment

Name: _________________________ Date: _________
```

---

**Document Version:** 1.0  
**Prepared for:** POC Execution  
**Date:** November 27, 2025  
**Classification:** Internal - Team Confidential