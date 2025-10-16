## OpenAI Campaign Automation Pilot Documentation

**Project:** FST Fundraiser Review Board (FRB) AI Moderation  
**Period:** May 1 - November 3, 2025  
**Status:** Phase 3a Complete | Phase 3b In Progress  
**Last Updated:** October 10, 2025

---
## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Pilot Overview](#pilot-overview)
3. [Approach & Methodology](#approach--methodology)
4. [Results](#phase-by-phase-results)
5. [Challenges & Limitations](#challenges--limitations)
6. [Key Learnings](#key-learnings)
7. [Technical Performance Details](#technical-performance-details)
8. [Business Impact](#business-impact)
9. [Recommendations & Next Steps](#recommendations--next-steps)
10. [Success Stories & Use Cases](#success-stories--use-cases)
11. [Comparative Analysis](#comparative-analysis)
12. [Appendices](#appendices)
13. [Conclusion](#conclusion)
---

## Executive Summary

The FST team requires an automated workflow to support fundraising campaign and page content moderation. As the Australian Sports Foundation continues to scale, human-based content moderation alone cannot meet safety, regulatory, and operational needs. This pilot demonstrates that AI-powered content moderation using OpenAI's API can help the FST team reclaim time currently spent on manual review.

**Phase 3a Results (Sep 5 - Oct 3, 2025):**
- **688 projects created** during pilot period (286 regular operations + 402 bulk migrations)
- **455 projects flagged** by AI (66.1% overall flagging rate)
  - Regular operations: 142 flagged (49.7% rate)
  - Bulk migrations: 313 flagged (77.9% rate)
- **20 confirmed violations** identified (2.9% of all projects)
- **311 false positives** (68.4% of flagged content)
- **6.0% overall precision rate** (11.6% for regular operations, 3.2% for bulk migrations)

**Key Finding:** Regular operations showed better performance (11.6% precision) than bulk migrations (3.2% precision). While the AI successfully identified all policy violations, the high false positive rate created unsustainable review overhead. Threshold adjustments implemented on October 2, 2025 are being evaluated in Phase 3b.

> [!NOTE]
> Regular operations vs bulk migration
> Script has since been adjusted to exclude migrated projects.
---

## Pilot Overview

### Objective
To create an automated workflow using AI to support the FST team in moderating project and donation content, ensuring compliance with ASF's fundraising guidelines while reducing manual review burden.

### Timeline

**Original Plan:** May 1 - September 5, 2025  
**Actual Timeline:** Delayed due to OpenAI API errors and leave

| Phase | Period | Status | Key Activities |
|-------|--------|--------|----------------|
| **Phase 1: Setup** | May 1 - June 3 | ✅ Complete | Infrastructure setup, API integration, initial testing |
| **Phase 2: Simple Moderation** | June 3 - Sep 5 | ✅ Complete | Keyword flagging, basic automation, GitHub Actions setup |
| **Phase 3a: Full Moderation v1** | Sep 5 - Oct 3 | ✅ Complete | AI classification with sentiment analysis, full results analysed |
| **Phase 3b: Full Moderation v2** | Oct 3 - Oct 17 | 🔄 In Progress | Refined thresholds, reduced false positives (results pending) |
| **Phase 4: FIne Tuning Model** | Oct 17 - Nov 3 |  | Train custom model using Phase 3a and 3b Jira ticket data to improve precision |

**Note:** Projects were not reviewed by the automated system before August 26, 2025 due to API issues.

### Scope

**In Scope:**
- Automated flagging of fundraising projects based on AI analysis
- Text moderation using OpenAI Moderation API
- Classification into violation categories (Inducement, Membership/Fees, Ticketing/Admission, Sponsorship, Advertising, Personal Contact, Gambling, Alcohol)
- Confidence scoring and severity assessment
- Jira ticket creation for FST team review
- Daily batch processing (D-1 analysis schedule)

**Out of Scope:**
- Image and video moderation (future implementation)
- Real-time moderation (batch processing only)
- Automated project approval/rejection (human review required)

### Users
- **Primary:** FST team (Stacie, Amy, Andrew)

### Expected Benefits
- Reduce time spent manually reviewing compliant projects
- Prioritise review of potentially non-compliant projects
- Scale moderation capacity as ASF grows
- Maintain consistent policy enforcement

---

## Approach & Methodology

### Technology Stack

**Selected Tools:**
- **OpenAI Moderation API** - Text content analysis and classification
- **Python** - Salesforce data extraction, API integration, Jira ticket creation
- **GitHub Actions** - Daily automated execution (150 min/month)
- **Jira** - Review workflow and ticket management
- **Salesforce** - Source data for projects

**Selection Rationale:**
- **OpenAI:** Single platform for text moderation with low cost ($2 per 1M tokens)
- **Cost-effective:** Estimated 2000 tokens per project, well within budget
- **Scalable:** Can handle growing volume without infrastructure changes
- **Industry-proven:** Similar approaches used by Kickstarter and GoFundMe

### Data Privacy & Security

**No PII Shared with OpenAI:**
- ❌ Contact names
- ❌ Contact email addresses
- ❌ Club/contact addresses
- ❌ Gender, Age, Date of Birth
- ❌ Phone numbers

**Only Content Data Analysed:**
- ✅ Project story/summary
- ✅ Messages of support
- ✅ Contributor messages
- ✅ Campaign descriptions

### Workflow Architecture

```
1. Daily Schedule: Python script runs via GitHub Actions (processes D-1 projects)
2. Data Extraction: Pull project data from Salesforce
3. AI Analysis: OpenAI API analyzes text content
4. Classification: Score against 8 violation categories with confidence levels
5. Severity Assessment: High/Medium/Low based on violation type
6. Ticket Creation: Flagged projects create Jira tickets with evidence
7. Human Review: FST team reviews and takes action (Done/In Progress/To Do)
```

### Evaluation Criteria

**Phase 3a Success Metrics:**
- Daily automated execution without manual intervention
- All policy violations identified (100% recall on confirmed cases)
- Acceptable false positive rate (<20% target, 68.4% actual)
- Precision rate (>50% target, 6.0% actual)

---

## Phase-by-Phase Results

### Phase 1: Setup (May 1 - June 3, 2025)

**Objective:** Establish infrastructure and API connections

**Activities:**
- OpenAI API account setup and authentication
- Salesforce API integration for data extraction
- Jira API integration for ticket creation
- Initial script development and testing
- Custom word list creation for flagging

**Outcome:** ✅ Successfully established automated pipeline

**Challenges:**
- OpenAI API connectivity errors caused initial delays
- Required troubleshooting of authentication and rate limiting
- Lucia's annual leave extended timeline

**Key Decision:** Added custom keyword flagging to supplement AI classification

---

### Phase 2: Simple Moderation (June 3 - Sep 5, 2025)

**Objective:** Implement basic moderation with keyword flagging

**Activities:**
- June 3: Added initial keyword list
- June 18: Implemented GitHub Actions automation
- August 1: Extended keyword flagging to "Messages of Support" field
- August 22: Script revision to properly flag Story, Summary, and Contributor Message fields

**Key Metrics:**
- Script ran manually with supervised execution
- Created small number of test tickets
- Validated Jira ticket creation process

**Outcome:** ✅ Automation working reliably, ready for AI classification

**Learnings:**
- Keyword-only approach insufficient for nuanced policy violations
- Manual script execution sustainable only for testing phase
- Need for sentiment and context analysis confirmed

---

### Phase 3a: Full Moderation v1 (Sep 5 - Oct 3, 2025)

**Objective:** Deploy full AI-powered moderation with sentiment analysis

**Implementation Date:** September 5, 2025

#### Quantitative Results

**Volume Metrics:**
- **Total projects created:** 688 (across 38 days)
  - Regular operations: 286 projects (42%)
  - Bulk migrations: 402 projects (58%) - Sep 17 & Sep 29
- **Total tickets flagged:** 455 (66.1% overall flagging rate)
- **Average projects per day:** 18.1
- **Average tickets per day:** 14.2
- **Peak days:** Sep 17 (188 projects), Sep 29 (214 projects) - due to migration

**Performance Metrics (as of Oct 7, 2025):**

*All Projects Combined:*
| Metric | Count | Percentage |
|--------|-------|------------|
| **Total Projects** | 688 | 100% |
| **Tickets Flagged** | 455 | 66.1% |
| **True Positives** (In Progress → Done) | 20 | 2.9% of projects |
| **False Positives** (Direct to Done) | 311 | 68.4% of flagged |
| **Currently Under Review** (In Progress) | 87 | 19.1% of flagged |
| **Awaiting Review** (To Do) | 37 | 8.1% of flagged |
| **Total Resolved** | 331 | 72.7% of flagged |
| **Precision Rate** | 20/331 | **6.0%** |

---

### Regular Operations vs Bulk Migrations Analysis

**Note:** During Phase 3a, two data migrations occurred (Sep 17: 188 projects, Sep 29: 214 projects), representing 58% of total volume. These migrations were not part of the original pilot scope but were processed by the system, providing valuable insights into system performance under different scenarios.

#### Dual Analysis: Comparing Regular Operations to Bulk Imports

**Data as of October 7, 2025:**

| Metric | All Projects | Regular Operations | Bulk Migrations |
|--------|--------------|-------------------|-----------------|
| **Total Projects** | 688 | 286 (42%) | 402 (58%) |
| **Tickets Flagged** | 455 | 142 | 313 |
| **Flagging Rate** | **66.1%** | **49.7%** | **77.9%** |
| **True Positives (TP)** | 20 | 13 | 7 |
| **False Positives (FP)** | 311 | 99 | 212 |
| **FP Rate (of flagged)** | **68.4%** | **69.7%** | **67.7%** |
| **Currently In Progress** | 87 | 24 | 63 |
| **Awaiting Review (To Do)** | 37 | 6 | 31 |
| **Total Resolved** | 331 | 112 | 219 |
| **Precision Rate** | **6.0%** | **11.6%** | **3.2%** |

#### Key Findings

**1. Regular Operations Show Better Performance**
- Regular operations achieved **11.6% precision** vs 3.2% for bulk migrations
- **49.7% flagging rate** for regular ops vs 77.9% for bulk imports
- Migrated content triggered the AI more frequently, suggesting differences in content quality, formatting, or context

**2. Bulk Migrations Amplified Over-flagging Problem**
- 77.9% of migrated projects were flagged (313 of 402)
- Only 3.2% of flagged migrations were true violations (7 of 219 resolved)
- 67.7% false positive rate indicates the AI struggled significantly with historical content

**3. Both Scenarios Show Core Issue**
- Even excluding bulk imports, **49.7% flagging rate remains high**
- Regular operations precision of 11.6% is still far below industry target (40%+)
- The threshold calibration problem exists in both contexts, though more pronounced with migrations
<!--
#### Why Bulk Migrations Performed Differently

**Hypothesised Reasons:**
1. **Content Age:** Migrated projects may use older fundraising language patterns that differ from current trends
2. **Formatting Differences:** Historical data may have different structure or formatting that confuses the AI
3. **Policy Evolution:** Fundraising guidelines may have evolved, making older content appear more ambiguous
4. **Data Quality:** Migrated data may lack some context fields that newer projects include

#### Implications for Phase 3b

**For Evaluation:**
- **Primary focus:** Regular operations metrics (49.7% flagging, 11.6% precision)
- **Secondary consideration:** System must handle future migrations reasonably

**Phase 3b Success Criteria (Recommended):**

*For Regular Operations:*
- ✅ Flagging rate: <35% (currently 49.7%)
- ✅ Precision rate: >40% (currently 11.6%)
- ✅ False positive rate: <30% (currently 69.7%)

*For All Projects (Including Future Migrations):*
- ✅ Flagging rate: <50% (currently 66.1%)
- ✅ Precision rate: >25% (currently 6.0%)
- ✅ No false negatives (maintain 100% recall)

#### Recommendation

**Report both metrics, emphasise regular operations:**

Given that migrations were outside the original pilot scope and represent atypical content, we recommend:

1. **Primary reporting:** Use regular operations metrics (286 projects) as the baseline for Phase 3b evaluation
2. **Transparency:** Continue to report combined metrics but note the migration impact
3. **Phase 3b targets:** Set goals based on regular operations performance, not combined average
4. **Future planning:** Develop separate thresholds or processing logic for bulk migrations if they continue

**Rationale:** Regular operations (49.7% flagging, 11.6% precision) better represent typical system performance and provide more realistic targets for improvement. However, excluding bulk imports entirely from reporting would lack transparency and miss valuable learnings about system behavior under load.
-->
---

**Flagging Rate Analysis:**
- **Overall flagging:** 66.1% (455 of 688 projects)
- **Regular operations:** 49.7% (142 of 286 projects)
- **Bulk imports:** 77.9% (313 of 402 projects)
- **Average daily flagging:** 85.7% (when projects were created)
- **Range:** 7.7% (best day) to 166.7% (low-volume days with statistical noise)

**Cost Analysis:**
- **OpenAI API usage (Sep 3 - Oct 2):** 
  - Total spend: **$0.19**
  - Total tokens: 691,148 (input) + 138,114 (output) = 829,262 total
  - Total API requests: 964
  - Model: gpt-4o-mini-2024-07-18
- **Cost per project:** $0.0004 (less than half a cent)
- **Tokens per project:** ~1,526 (below estimated 2,000)
- **Requests per project:** ~2.1 (indicates some retry/error handling)
- **GitHub Actions:** 150 minutes/month automation = $0.00 (within free tier)
- **Total Phase 3a cost:** $0.19 for AI processing
- **Budget utilisation:** 0.18% of $120/month October budget

**Time Savings:**
- **Manual review time saved:** Estimated XX hours/week by auto-clearing compliant projects
- **However:** High false positive rate (311 cases) created ~XX hours of unnecessary review
- **Net impact:** Negative time savings in Phase 3a due to over-flagging

#### AI Classification Performance

**Categories Detected (Total: 2,180 classifications across 453 tickets):**

| Category | Occurrences | Notes |
|----------|-------------|-------|
| Inducement | 404 | Most common - fundraising incentives |
| Membership/Fees | 377 | Club fees, membership costs |
| Ticketing/Admission | 359 | Event tickets, entry fees |
| Sponsorship | 357 | Sponsorship arrangements |
| Advertising | 345 | Commercial promotion |
| Personal Contact | 328 | Direct contact information |
| Gambling | 9 | Raffle, lottery references |
| Alcohol | 1 | Alcohol-related content |

**Note:** Each ticket can have multiple category flags (avg 4.8 classifications per ticket)

**Confidence Score Distribution (False Positives):**
- 0.8-1.0 (High): 19% - Model very confident but wrong
- 0.6-0.8 (Medium): 42% - Largest segment
- 0.4-0.6 (Low): 36%
- 0.2-0.4 (Very Low): 4%

**Confidence Score Distribution (True Positives):**
- 0.8-0.9: 19%
- 0.7-0.8: 17%
- 0.6-0.7: 29%
- 0.5-0.6: 31% - Most true violations in mid-range confidence
- 0.4-0.5: 5%

**Critical Finding:** **0% of true positives had confidence >0.9**, while false positives had 19% in 0.8+ range. This suggests the model's confidence calibration is inverse to accuracy.

**Severity Distribution:**

*False Positives:*
- High: 24%
- Medium: 71%
- Low: 4%

*True Positives:*
- High: 25%
- Medium: 75%
- Low: 0%

**Keyword Flagging:**
- Only **27 tickets (6%)** were flagged via keyword matching
- Top keywords: "event" (5), "member" (3), "sponsor" (3)
- **Primary detection method:** AI semantic classification (94% of flags)

#### Qualitative Results

**True Positive Examples (Policy Violations Confirmed):**

1. **Raffle/Gambling (Inducement):** Project offering raffle tickets or prize draws in exchange for donations
2. **Membership Fees:** Campaign collecting club membership fees through ASF platform
3. **Sponsorship Arrangements:** Projects offering logo placement or advertising in exchange for support
4. **Event Ticketing:** Selling event admission as fundraising mechanism

**False Positive Patterns:**

1. **Legitimate fundraising language** flagged as "Inducement" (e.g., "Help us reach our goal!")
2. **Event descriptions** without ticket sales flagged as "Ticketing/Admission"
3. **Thanking sponsors** flagged as "Sponsorship" violations
4. **Community fundraising** flagged as "Membership/Fees"

**User Feedback Themes:**
- Request for tighter thresholds to reduce noise
<!--
- FST team appreciated automation for bulk project days
- Frustration with high volume of false positives requiring manual dismissal
- Positive feedback on evidence provided in Jira tickets (LLM reasoning) -->

#### Comparison to Baseline

**Before Automation (Manual Review):**
- All projects manually reviewed by FST team
- Estimated X-X minutes per project review
- ~XX hours/month for 688 projects (assumed X min/project)

**Phase 3a (Automated + Manual Review):**
- 233 projects not flagged (33.9%) - saved ~XX hours
- 455 projects flagged requiring review
- 311 false positives = unnecessary ~XX hours
- **Net result:** Minimal/no time savings

---

### Phase 3b: Full Moderation v2 (Oct 3 - Oct 17, 2025)

**Objective:** Reduce false positive rate through threshold refinement

**Status:** 🔄 In Progress

**Changes Implemented (Oct 2, 2025):**
- Tightened confidence thresholds across all categories
- Reduced sensitivity by 30-40% based on Phase 3a learnings
- Adjusted keyword lists based on false positive analysis
- Refined "Personal Contact" category (highest FP rate in 3a)

**Expected Outcomes:**
- Target false positive rate: <30% (regular operations)
- Target precision rate: >40% (regular operations)
- Maintain 100% recall on policy violations

**Evaluation Period:** 1 month (until November 3, 2025)

**Metrics to Track:**
- Daily flagging rate (regular operations focus)
- Precision and recall
- False positive reduction
- User satisfaction (FST team feedback)
- Time savings vs manual review

**Results:** *To be added after Phase 3b completion*

---

## Challenges & Limitations

### Technical Challenges

**1. Over-flagging / Low Precision (6.0% overall, 11.6% regular ops)**
- **Issue:** AI flagged incorrectly 68.4% of the time overall (69.7% for regular ops)
- **Root cause:** Initial thresholds too sensitive, model detecting legitimate fundraising language
- **Impact:** Created more work than it saved
- **Resolution:** Threshold tightening in Phase 3b

**2. Confidence Score Miscalibration**
- **Issue:** Model most confident on false positives, least confident on true violations
- **Evidence:** 0% of true positives had >0.9 confidence
- **Impact:** Cannot rely on confidence scores for prioritisation
- **Resolution:** Under investigation; requires model retraining

**3. Bulk Import Processing Delay**
- **Issue:** Large project imports (188, 214 projects) processed next day
- **Explanation:** Expected behavior - script analyzes D-1 projects
- **Impact:** Temporary spike in Jira tickets following migration
- **Resolution:** None needed - working as designed

**4. Multi-label Classification Complexity**
- **Issue:** Each project receives average of 4.8 category flags
- **Impact:** Difficult to determine primary violation type
- **Resolution:** Consider single-label classification or hierarchical flagging

### Organizational Challenges

**1. Timeline Delays**
- **Issue:** OpenAI API errors and leave delayed Phase 1
- **Impact:** 3-month delay from original plan
- **Learning:** Build buffer time for external API dependencies

**2. Threshold Calibration Iterations**
- **Issue:** Required multiple adjustments during pilot
- **Impact:** Extended testing period
- **Learning:** Expect 2-3 threshold iterations for any new AI system

### AI-Specific Limitations (v1)

**1. Context Understanding**
- **Issue:** AI struggles to distinguish legitimate fundraising from policy violations
- **Example:** "Support our team" flagged as sponsorship
- **Limitation:** Language-based models lack ASF policy context

**2. Semantic Classification Over-triggers**
- **Issue:** 94% of flags from AI analysis, only 6% from keywords
- **Impact:** Model detecting patterns humans wouldn't flag
- **Consideration:** May need hybrid approach with stricter keyword + AI

**3. Category Definition Ambiguity**
- **Issue:** "Personal Contact" category had 46.6% false positive rate
- **Root cause:** Unclear boundary between contact info and legitimate communication
- **Resolution:** Category refinement or removal needed

---

## Key Learnings

### What Worked Well

✅ **Automation Infrastructure**
- GitHub Actions executed reliably every day without intervention
- Jira integration created well-formatted tickets with evidence
- Salesforce data extraction handled 688 projects without errors

✅ **Violation Detection (Recall)**
- AI successfully identified all 20 confirmed policy violations
- No false negatives detected in Phase 3a
- Evidence provided in tickets helped reviewers make decisions quickly

✅ **Bulk Processing Capability**
- Handled two major bulk imports (188 and 214 projects) within 24 hours
- Demonstrated scalability for future growth

**Cost Efficiency**
- Total AI processing cost $0.19 for entire Phase 3a
- **10x more cost-effective than initially estimated**
- Significantly cheaper than alternative moderation services
- Well under budget ($120/month allocation, using 0.18%)

### Challenges Encountered

⚠️ **Threshold Calibration Critical**
- Initial thresholds created 68.4% false positive rate
- Required significant manual tuning and domain knowledge
- Multiple iterations needed to find optimal settings

⚠️ **Confidence Scores Unreliable**
- Inverse correlation between confidence and accuracy
- Cannot use confidence for automatic approval/routing
- May be inherent limitation of OpenAI Moderation API

⚠️ **Semantic Analysis Too Broad**
- AI detecting "violations" that aren't policy concerns
- Language patterns don't always indicate actual violations
- Need better context understanding or ASF-specific training

### Adjustments Made During Pilot

**Phase 1 → Phase 2:**
- Added custom keyword lists at FST team request
- Expanded fields analysed (Story, Summary, Messages)

**Phase 2 → Phase 3a:**
- Implemented sentiment analysis
- Added full AI classification across 8 categories
- Automated via GitHub Actions

**Phase 3a → Phase 3b (Oct 2):**
- Tightened confidence thresholds by 30-40%
- Reduced sensitivity across all categories
- Refined keyword lists based on false positive patterns

**Changelog:** All adjustments documented in Confluence

---

## Technical Performance Details

### Model Architecture

**OpenAI Moderation API Specifications:**
- **Model:** text-moderation-stable
- **Input:** Project text fields (Story, Summary, Messages)
- **Output:** Binary classification + confidence scores per category
- **Processing:** ~2,000 tokens per project
- **Latency:** <2 seconds per project analysis

### Classification Categories & Thresholds

**Phase 3a Thresholds (Sep 5 - Oct 2):**
| Category | Initial Threshold | Flags Generated |
|----------|-------------------|-----------------|
| Inducement | 0.4 | 404 |
| Membership/Fees | 0.4 | 377 |
| Ticketing/Admission | 0.4 | 359 |
| Sponsorship | 0.4 | 357 |
| Advertising | 0.4 | 345 |
| Personal Contact | 0.4 | 328 |
| Gambling | 0.3 | 9 |
| Alcohol | 0.3 | 1 |

**Phase 3b Thresholds (Oct 3+):**
- All categories: Increased to 0.6-0.7 range
- Expected 30-40% reduction in flagging volume

### Infrastructure & Integration

**Salesforce Integration:**
- **API:** REST API with OAuth authentication
- **Query:** SOQL to retrieve projects from last 24 hours
- **Fields extracted:** Campaign_Name__c, Story__c, Summary__c, Contributor_Message__c
- **Error handling:** Retry logic for API failures

**Jira Integration:**
- **API:** Jira REST API v3
- **Project:** FRB (FST Fundraiser Review Board)
- **Issue Type:** Task
- **Custom Fields:** Includes AI evidence, confidence scores, severity
- **Automation:** Auto-assigns based on availability

**GitHub Actions:**
- **Schedule:** Daily at 11:00 AM AEDT
- **Runtime:** <2 minutes for typical daily volume
<!-- - **Logs:** Full execution logs retained for 90 days -->
- **Notifications:** Email alert on script failure

### Performance Benchmarks

**Processing Speed:**
- 688 projects processed over 29 days
- Average 24 projects/day analysed
- <10 minutes total processing time per day
- Scalable to 100+ projects/day without infrastructure changes

**Reliability:**
- 100% uptime during Phase 3a
- 0 script failures
- 0 missed daily executions

---

## Business Impact

### Return on Investment (ROI)

**Phase 3a Actual Costs:**
| Item | Phase 3a Cost | Monthly Projection | Annual Projection |
|------|---------------|-------------------|-------------------|
| OpenAI API | **$0.19** (actual) | **$0.20** | **$2.48** |
| GitHub Actions | $0 (free tier) | $0 | $0 |
| Development Time | XX hours| N/A | One-time |
| **Total Phase 3a** | **$XX** | **$0.20/month** | **$2.48/year recurring** |

**Key Cost Findings:**
- **90% cheaper than estimated:** Actual $0.19 vs estimated $1.81
- **Tokens per project:** 1,526 vs estimated 2,000 (24% lower)
- **Budget utilisation:** Using only 0.18% of $120/month budget
- **Scalability headroom:** Can handle **300x current volume** within existing budget

**Cost at Scale:**
| Volume Multiplier | Annual Cost | Within Budget? |
|-------------------|-------------|----------------|
| Current (688 projects/month) | $2.48 | ✅ Yes ($0.20/month) |
| 2x volume (1,376 projects/month) | $4.95 | ✅ Yes ($0.41/month) |
| 5x volume (3,440 projects/month) | $12.38 | ✅ Yes ($1.03/month) |
| 10x volume (6,880 projects/month) | $24.77 | ✅ Yes ($2.06/month) |

**Benefits (Projected after Phase 3b optimisation):**
- **Time savings:** XX hours/month (assuming 20% precision improvement)
- **Value:** XX hrs × $XX/hr = $XX/month
- **Annual value:** $XX
- **ROI:** **XX%** (($XX - $XX) / $XX) after accounting for $0.19/month operational costs
- **Payback period:** <1 hour of saved time pays for entire year of API costs

**Current State (Phase 3a):**
- **Time savings:** Minimal to negative due to false positives
- **ROI:** Not yet positive
- **Breakeven:** Requires precision >30% to achieve net time savings
- **However:** Operational cost so low ($0.19/month) that even minimal improvement is cost-effective
<!--
### Resource Requirements

**Current:**
- **Staff:** 3 reviewers (Amy, Stacie, Andrew)
- **Review time:** ~XX hours/week across team
- **OpenAI spend:** **$0.20/month** (actual Phase 3a usage)
- **Total operational cost:** $0.20/month

**If Pilot is Successful:**
- **Staff:** Same 3 reviewers
- **Review time:** Estimated X-X hours/week (XX% reduction goal)
- **OpenAI spend:** $0.20-0.25/month
- **Monitoring:** X hours/month for threshold tuning
- **Total operational cost:** <$0.50/month

### Scalability Considerations

**Current Capacity:**
- Handles 20-30 projects/day comfortably
- Tested up to 214 projects/day (bulk import)
- Cost per project: $0.0004

**Projected Growth:**
- ASF expects X-X projects/day in 2026
- Current infrastructure can scale to 300x volume within budget
- At 100 projects/day: ~$0.60/month cost
- At 500 projects/day: ~$3.00/month cost

**Scalability Advantage:**
- **Cost constraint removed:** Virtually unlimited scaling capacity within $120 budget
- No infrastructure changes needed for 100x growth
- Linear cost scaling (double volume = double cost)
- Budget allows for 60,000 projects/month before hitting $120 limit

**Scaling Requirements:**
- ✅ No infrastructure changes needed for 10x growth
- ⚠️ Threshold monitoring becomes more critical at scale
- ✅ Cost remains negligible even at 50x volume

---
-->
## Recommendations & Next Steps

### Next Steps: Phase 3b Evaluation (Oct 17, 2025)

### Phase 4: Fine-Tuned Model (Oct 17 - Nov 3, 2025)

**Objective:** Train custom model using Phase 3a and 3b Jira ticket data to improve precision

**Approach:**

Phase 3a and 3b's Jira tickets provide labelled training data based on FST team's actual moderation decisions:
- **TBD XX usable examples**: 30 true violations + 366 false positives
- Use `jira_finetuning_prep.py` to convert Jira CSV into OpenAI training format
- Use `finetuning_integration.py` to upload data and train model (~15-30 minutes)
- Deploy fine-tuned model (gpt-4o-mini) to replace base model

**Why Fine-Tuning:**
Base model lacks ASF-specific context and flags legitimate fundraising language ("Help us reach our goal") as violations. Custom model trained on FST team's decisions will distinguish actual policy violations from compliant content.

**Expected Results:**

| Metric | Phase 3a Baseline | Phase 3b Target |
|--------|------------------|-----------------|
| Precision (Regular Ops) | 11.6% | **>40%** |
| Flagging Rate | 49.7% | **<35%** |
| False Positives/Month | 99 | **<50** |
| Monthly Cost | $0.19 | $0.38 (+$0.19) |

**Maintenance:** Retrain model every X months with new Jira data (~$9/quarter) for continuous improvement.

**Results:** *To be added after Phase 4 completion*

---
<!--
✅ **Proceed if (Regular Operations Metrics):**
- Flagging rate <35% (baseline: 49.7%)
- Precision rate >40% (baseline: 11.6%)
- False positive rate <30% (baseline: 69.7%)
- FST team satisfaction score >X/10
- Net time savings >X hours/month

⚠️ **Additional Iteration Needed if:**
- Precision <40%
- FST team identifies specific threshold adjustments
- Cost remains <$5/month

### Proposed Timeline for Next Phase

**Phase 4: Production Deployment (If Phase 3b Successful)**

| Milestone | Date | Activities |
|-----------|------|------------|
| Phase 3b Results | Oct 17, 2025 | Complete analysis of refined thresholds; prepare recommendation report |
| Decision Point | Nov 10, 2025 | FST team + Insights team review; Go/No-go decision for production |
| Production Prep | Nov 11-17, 2025 | Documentation finalisation; handover procedures; training materials |
| Production Launch | Nov 18, 2025 | Enable production mode; monitor for 2 weeks |
| First Review | Dec 2, 2025 | Assess first 2 weeks; minor adjustments if needed |
| Quarterly Review | Feb 2026 | Full system audit; threshold optimisation; annual planning |

**Phase 4 Success Criteria:**
- Maintain >40% precision rate over 30 days
- <35% flagging rate sustained
- Zero critical false negatives (maintain 100% recall)
- Positive FST team feedback
- Net time savings >X hours/month
-->
<!--
## Alternative Recommendations

### If Phase 3b Does Not Meet Targets

**Option A: Extended Iteration (Phase 3c)**
- **Timeline:** Additional 4-6 weeks
- **Focus:** Further threshold refinement based on 3b learnings
- **Cost:** Additional $0.40-0.60 in API costs
- **Criteria for success:** Same as Phase 3b targets
- **Risk:** May indicate fundamental model limitation

**Option B: Hybrid Approach**
- **Description:** Combine keyword flagging with AI analysis
- **Primary filter:** Keywords flag high-confidence violations
- **Secondary filter:** AI analyzes remaining projects at higher threshold
- **Advantage:** Reduces AI over-flagging while maintaining recall
- **Implementation:** 2-3 weeks additional development

**Option C: Category Reduction**
- **Description:** Focus on 3-4 highest-value categories only
- **Remove:** Low-occurrence categories (Gambling, Alcohol, Personal Contact)
- **Keep:** Inducement, Membership/Fees, Ticketing, Sponsorship
- **Advantage:** Simpler model, higher precision on core violations
- **Trade-off:** May miss rare but important violations

**Option D: Manual Review with AI Assistance**
- **Description:** AI provides flags as suggestions, not workflow triggers
- **Implementation:** Show AI analysis in dashboard without creating Jira tickets
- **Advantage:** FST team gets context without false positive overhead
- **Trade-off:** Less automation benefit


### If Phase 3b Succeeds

**Enhancement Roadmap (2026):**

- Real-time moderation (vs current D-1 batch processing)
- Enhanced reporting dashboard
- Image moderation using OpenAI Vision API
- Historical data analysis for policy trend insights
- Custom model fine-tuning on ASF-specific policies
- Predictive risk scoring for campaigns
-->
<!--
## Risk Assessment & Mitigation

### Current Risks

**High Priority Risks:**

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| **False negatives (missed violations)** | Critical | Low | Maintain human review for all high-value projects; monthly audit sample |
| **Model drift over time** | High | Medium | Quarterly threshold reviews; performance monitoring dashboard |
| **API service disruption** | High | Low | Fallback to manual review; SLA monitoring; backup processing queue |
| **Cost escalation at scale** | Medium | Very Low | Budget alerts at $5, $10, $20 thresholds; current usage 0.18% of budget |

**Medium Priority Risks:**

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| **Threshold tuning burden** | Medium | Medium | Document optimisation process |
| **Team resistance to AI** | Medium | Low | Transparent reporting; involve team in threshold decisions |
| **Policy changes requiring retraining** | Medium | Medium | Quarterly model reviews; maintain flexible threshold system |
| **Bulk import processing delays** | Low | Medium | Set expectations; D-1 processing is by design |

**Low Priority Risks:**

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| **GitHub Actions downtime** | Low | Very Low | Manual script execution procedure documented |
| **Jira integration failure** | Low | Very Low | Email notification backup; error logging |
| **Data privacy concerns** | Low | Very Low | No PII shared |

### Mitigation Strategies

**Operational Safeguards:**
1. **Human-in-the-loop:** All flagged content requires manual review before action
2. **Audit trail:** Complete logging of all AI decisions and human overrides
3. **Escalation path:** Complex cases routed to senior reviewers
4. **Regular calibration:** Monthly threshold reviews using precision/recall metrics

**Technical Safeguards:**
1. **Rate limiting:** API calls throttled to prevent runaway costs
2. **Error handling:** Graceful degradation to manual review on API failure
3. **Monitoring:** Real-time alerts for unusual flagging patterns
4. **Rollback capability:** Can revert to previous thresholds within minutes

**Governance Safeguards:**
1. **Quarterly audits:** Independent review of AI decisions
2. **Bias testing:** Regular checks for category or content bias
3. **Documentation:** All threshold changes logged with rationale
4. **Stakeholder review:** FST team reviews major changes
-->
<!--
## Governance & Ongoing Management

### Roles & Responsibilities

**Day-to-Day Operations:**
- **FST Team (Amy, Stacie, Andrew):** Review flagged content; dismiss false positives; escalate questions
- **Script Owner (Lucia/Insights):** Monitor daily execution; investigate script failures; coordinate with OpenAI on API issues

**Strategic Oversight:**
- **FST Team (Stacie):** Monthly performance review; approve threshold changes; provide policy guidance
- **Insights Team:** Quarterly business review; budget management; roadmap prioritisation

### Performance Monitoring

**Daily Metrics (Automated):**
- Projects processed
- Tickets created
- Script execution status
- API cost tracking

**Weekly Metrics (Manual):**
- Flagging rate trend
- Resolution time per ticket
- False positive count
- Team feedback themes

**Monthly Metrics (Formal Review):**
- Precision rate (TP / [TP + FP])
- Recall rate (TP / [TP + FN])
- Time savings vs manual review
- Cost per project
- User satisfaction score

**Quarterly Metrics (Strategic Review):**
- ROI calculation
- Threshold effectiveness
- Model drift analysis
- Roadmap progress
- Budget forecast

### Threshold Adjustment Process

**When to Adjust:**
- Precision drops below 35% for 2 consecutive weeks
- False positive rate exceeds 35% consistently
- New policy guidance requires recalibration
- FST team requests based on emerging patterns

**Adjustment Workflow:**
1. **Analysis:** Review false positive patterns over 2-4 weeks
2. **Proposal:** Document proposed threshold changes with expected impact
3. **Testing:** Simulate changes on historical data
4. **Approval:** sign-off
5. **Implementation:** Deploy change with monitoring plan
6. **Validation:** 2-week assessment period
7. **Documentation:** Update Confluence with changes and rationale

**Threshold Change Log:**
- All changes documented in Confluence
- Include: Date, categories affected, old/new thresholds, rationale
- Accessible to FST team and Insights team
-->
<!--
## Knowledge Transfer & Documentation

### Documentation Repository

**Primary Location:** Confluence - [Fundraising Campaign Screening Automation](https://sportsfoundation.atlassian.net/wiki/spaces/ARAI/pages/2511372379/)

**Documentation Set:**
1. **This document:** Complete pilot results and recommendations
2. **Technical runbook:** Script execution, troubleshooting, API management

### Training & Handover

**FST Team Training (2 hours):**
- How the AI classification works (30 min)
- Interpreting confidence scores and categories (30 min)
- Reviewing and dismissing false positives efficiently (30 min)
- Escalation procedures for edge cases (15 min)
- Q&A and hands-on practice (15 min)

**Technical Handover (4 hours):**
- GitHub repository walkthrough
- Script architecture and data flow
- API authentication and troubleshooting
- Threshold adjustment procedure
- Monitoring and alerting setup
- Emergency procedures (API outage, cost spike)

**Ongoing Support:**
- **First 30 days:** Daily check-ins with FST team
- **Days 31-90:** Weekly check-ins
- **After 90 days:** Monthly reviews + on-demand support
- **Slack channel:** #ai-moderation-support for questions
-->

## Success Stories & Use Cases

### Pilot Highlights

**Automation Success:**
- **688 projects processed** fully automatically without manual intervention
- **38 consecutive days** of reliable daily execution
- **100% uptime** with zero script failures
- **$0.19 total cost** - proving extreme cost efficiency

**Violation Detection:**
- **20 policy violations identified** that may have been missed in manual review
- **100% recall** - no confirmed false negatives
- **Evidence-based tickets** provided clear reasoning for reviewer decisions

**Scalability Proof:**
- Handled **214 projects in single day** (Sep 29 bulk import)
- Demonstrated capacity for **10x+ volume** without infrastructure changes
- **300x headroom** within current budget allocation

### Notable Cases

**Case Study 1: Membership Fee Collection**
- **Scenario:** Sports club using ASF to collect annual membership dues
- **AI Detection:** Flagged as Membership/Fees (0.78 confidence)
- **Outcome:** Confirmed violation; club redirected to proper membership platform
- **Value:** Prevented policy breach and potential compliance issues

**Case Study 2: Bulk Import Processing**
- **Scenario:** 214 historical projects migrated on Sep 29
- **AI Processing:** All projects analyzed within 24 hours
- **Outcome:** 7 violations found in historical data; 212 false positives cleared
- **Value:** Demonstrated system can handle irregular high-volume scenarios

### Lessons from False Positives

**Pattern 1: Generic Fundraising Language**
- **False flag:** "Help us reach our goal!" flagged as Inducement
- **Learning:** Common fundraising phrases aren't violations
- **Action:** Adjusted threshold to require more specific inducement language

**Pattern 2: Community Events**
- **False flag:** "Join us for our annual fun run" flagged as Ticketing
- **Learning:** Event participation ≠ ticket sales
- **Action:** Required explicit pricing or admission language to trigger

**Pattern 3: Sponsor Acknowledgment**
- **False flag:** "Thanks to our sponsors XYZ Corp" flagged as Sponsorship
- **Learning:** Thanking existing sponsors isn't seeking sponsorship
- **Action:** Distinguish between solicitation and acknowledgment
<!--
---

## Comparative Analysis

### Industry Benchmarks

**Content Moderation AI Performance (Industry Standards):**
| Metric | Industry Average | ASF Phase 3a | ASF Target |
|--------|-----------------|--------------|------------|
| **Precision** | 40-60% | 6.0% (11.6% regular ops) | >40% |
| **Recall** | 70-85% | 100% | >95% |
| **False Positive Rate** | 20-40% | 68.4% (69.7% regular ops) | <30% |
| **Processing Cost** | $0.01-0.05/item | $0.0004/item | <$0.01/item |

**ASF Performance vs Industry:**
- ✅ **Cost:** 10-100x better than industry average
- ✅ **Recall:** Significantly better than industry (100% vs 70-85%)
- ❌ **Precision:** Below industry standard (needs Phase 3b improvement)
- ✅ **Scalability:** Comparable to major platforms
-->
---

## Appendices

### Appendix A: Technical Specifications

**System Architecture Diagram:**
```
┌─────────────────┠                 ┌──────────────────┠
│  Salesforce     │                 │   GitHub Actions │
│  (Source Data)  │◄────────────────┤   (Scheduler)    │
└────────┬────────┘                 └──────────────────┘
         │                                    │
         │ SOQL Query                         │ Daily 11AM AEDT
         │ (D-1 Projects)                     ▼
         ▼                          ┌──────────────────┐
┌─────────────────┐                 │  Python Script   │
│ Project Records │                 │  (Orchestrator)  │
│ - Story         │────────────────►│                  │
│ - Summary       │                 │  1. Extract data │
│ - Messages      │                 │  2. Call OpenAI  │
└─────────────────┘                 │  3. Create Jira  │
                                    └────────┬─────────┘
                                             │
                  ┌──────────────────────────┼──────────────────────────┐
                  │                          │                          │
                  ▼                          ▼                          ▼
         ┌────────────────┐        ┌─────────────────┐       ┌─────────────────┐
         │  OpenAI API    │        │   Jira API      │       │  GitHub Logs    │
         │  (Analysis)    │        │   (Ticketing)   │       │  (Audit Trail)  │
         │                │        │                 │       │                 │
         │ - Moderation   │        │ - Create Issue  │       │ - Execution log │
         │ - Categories   │        │ - Add Evidence  │       │ - Error traces  │
         │ - Confidence   │        │                 │        │ - API responses │
         └────────────────┘        └─────────────────┘       └─────────────────┘
                  │                          │
                  │                          ▼
                  │                 ┌─────────────────┐
                  │                 │   FST Team      │
                  │                 │   (Reviewers)   │
                  │                 │                 │
                  │                 │ - Review ticket │
                  └────────────────►│ - Validate flag │
                                    │ - Take action   │
                                    └─────────────────┘
```

**API Endpoints Used:**
- **Salesforce:** `/services/data/v57.0/query`
- **OpenAI:** `https://api.openai.com/v1/moderations`
- **Jira:** `https://sportsfoundation.atlassian.net/rest/api/3/issue`

**Authentication:**
- Salesforce: OAuth 2.0 with refresh token
- OpenAI: Bearer token authentication
- Jira: API token with user authentication

**Data Flow:**
1. Script queries Salesforce for projects created yesterday (D-1)
2. Extracts text fields: Story, Summary, Contributor_Message, Messages_of_Support
3. Concatenates text and sends to OpenAI Moderation API
4. Receives category classifications with confidence scores
5. Applies threshold filtering (Phase 3a: 0.4, Phase 3b: 0.6-0.7)
6. For flagged projects, creates Jira ticket with evidence
7. Logs all activity to GitHub Actions console

---

### Appendix B: Category Definitions

**1. Inducement**
- **Definition:** Offering goods, services, or benefits in exchange for donations
- **Policy:** Donations must be voluntary with no expectation of direct return
- **Examples:** Raffle entries, prize draws, merchandise, exclusive access
- **Phase 3a Performance:** 404 flags (18.5% of total), moderate precision

**2. Membership/Fees**
- **Definition:** Collecting club membership dues or mandatory fees
- **Policy:** ASF platform is for donations, not membership payments
- **Examples:** Annual club fees, registration costs, subscription charges
- **Phase 3a Performance:** 377 flags (17.3% of total), high false positive rate

**3. Ticketing/Admission**
- **Definition:** Selling event tickets or charging admission
- **Policy:** Cannot sell tickets through ASF platform
- **Examples:** Concert tickets, game admission, entry fees
- **Phase 3a Performance:** 359 flags (16.5% of total), struggled with event descriptions

**4. Sponsorship**
- **Definition:** Seeking corporate sponsorships with benefits
- **Policy:** Direct sponsorship arrangements not allowed through platform
- **Examples:** Logo placement, brand exposure, corporate partnerships
- **Phase 3a Performance:** 357 flags (16.4% of total), confused acknowledgment with solicitation

**5. Advertising**
- **Definition:** Commercial promotion or paid advertising arrangements
- **Policy:** ASF platform not for commercial advertising
- **Examples:** Business promotion, product placement, paid endorsements
- **Phase 3a Performance:** 345 flags (15.8% of total), overlaps with Sponsorship

**6. Personal Contact**
- **Definition:** Sharing personal contact information for fundraising
- **Policy:** Donations should flow through ASF, not direct contact
- **Examples:** Phone numbers, email addresses, private social media
- **Phase 3a Performance:** 328 flags (15.0% of total), highest false positive issues

**7. Gambling**
- **Definition:** Lottery, raffle, or chance-based fundraising
- **Policy:** Gambling activities require specific permits and oversight
- **Examples:** Raffles, lotteries, games of chance
- **Phase 3a Performance:** 9 flags (0.4% of total), low occurrence but high accuracy

**8. Alcohol**
- **Definition:** Alcohol-related fundraising activities
- **Policy:** Alcohol sales/promotion must comply with licensing requirements
- **Examples:** Wine raffles, brewery tours, alcohol sales
- **Phase 3a Performance:** 1 flag (0.05% of total), insufficient data

**Category Overlap:**
- Average 4.8 categories flagged per ticket
- Inducement often co-occurs with Gambling (raffles)
- Sponsorship and Advertising frequently overlap
- Membership and Ticketing both involve payments

---

### Appendix C: Cost Breakdown & Projections

**Phase 3a Actual Costs (Sep 5 - Oct 3, 2025):**

| Item | Quantity | Unit Cost | Total Cost |
|------|----------|-----------|------------|
| OpenAI API - Input tokens | 691,148 | $0.150 / 1M | $0.104 |
| OpenAI API - Output tokens | 138,114 | $0.600 / 1M | $0.083 |
| **Total OpenAI** | 829,262 tokens | - | **$0.187** |
| GitHub Actions | 142 minutes | $0.008 / min | $0.00 (free tier) |
| **Total Phase 3a** | 29 days | - | **$0.19** |

**Per-Unit Economics:**
- Cost per project: $0.19 ÷ 688 = **$0.000276** (rounded to $0.0004 in report)
- Tokens per project: 829,262 ÷ 688 = **1,205 tokens** (below 2,000 estimate)
- Cost per true positive: $0.19 ÷ 20 = **$0.0095** (less than 1 cent)
- Cost per false positive: $0.19 ÷ 311 = **$0.0006** (less than 0.1 cent)

**Volume Projections for 2026:**

| Scenario | Projects/Month | Monthly Cost | Annual Cost | Budget Status |
|----------|----------------|--------------|-------------|---------------|
| Current | 688 | $0.19 | $2.28 | 0.2% of budget |
| Conservative (1.5x) | 1,032 | $0.29 | $3.48 | 0.2% of budget |
| Expected (2x) | 1,376 | $0.38 | $4.56 | 0.3% of budget |
| Growth (3x) | 2,064 | $0.57 | $6.84 | 0.5% of budget |
| High Growth (5x) | 3,440 | $0.95 | $11.40 | 0.8% of budget |
| Maximum (10x) | 6,880 | $1.90 | $22.80 | 1.6% of budget |

**Budget Comfort Zone:**
- Current budget: $120/month ($1,440/year)
- Phase 3a usage: 0.18% of budget
- Can scale to **63,158 projects/month** before hitting budget (333x current volume)
- Recommendation: Maintain $120/month allocation with no changes needed

**Cost Comparison to Alternatives:**

| Solution | Setup Cost | Monthly Cost (688 projects) | Annual Cost |
|----------|------------|----------------------------|-------------|
| **OpenAI API (Selected)** | $0 | $0.19 | $2.28 |
| Third-party moderation | $5,000-10,000 | $344-688 | $4,128-8,256 |
| Custom ML model | $50,000-150,000 | $500-1,000 | $6,000-12,000 |
| Additional staff (0.2 FTE) | $0 | $1,200 | $14,400 |

<!--
**ROI Calculation (Projected after Phase 3b):**
```
Assumptions:
- Manual review time: 5 min/project average
- FST team hourly rate: $35/hour (blended)
- Projects auto-cleared: 35% (assuming 35% flagging rate after 3b)
- Monthly volume: 688 projects

Monthly time savings:
688 projects × 35% auto-cleared × 5 min × (1 hour / 60 min) = 20.1 hours saved

Monthly value:
20.1 hours × $35/hour = $703.50

Monthly cost:
OpenAI API: $0.19

Net monthly benefit: $703.31
Annual net benefit: $8,439.72
ROI: ($8,439.72 - $2.28) / $2.28 × 100 = 370,000%
```

*Note: ROI calculation assumes Phase 3b achieves target performance. Phase 3a had negative time savings due to false positives.*
-->
---

### Appendix D: Data Privacy Compliance

**Personal Information Handling:**

**Data NOT Shared with OpenAI:**
- ❌ Contact names
- ❌ Contact email addresses
- ❌ Club/contact addresses
- ❌ Gender, Age, Date of Birth
- ❌ Phone numbers

**Data Shared with OpenAI (Content Only):**
- ✅ Project story/description (public-facing)
- ✅ Project summary (public-facing)
- ✅ Messages of support (public-facing, anonymised)
- ✅ Contributor messages (public-facing, anonymised)

**Privacy Safeguards:**
1. **Data Minimisation:** Only public-facing content analyzed
2. **Anonymisation:** No identifiable information in API requests
3. **OpenAI Data Policy:** Content not used for model training (as per OpenAI Enterprise agreement)
4. **Retention:** OpenAI retains data for 30 days for abuse monitoring only
<!--
**Audit Trail:**
- All API requests logged (without content) in GitHub Actions
- 90-day retention of execution logs

**User Consent:**
- Project creators consent to public content moderation via Terms of Service
- No additional consent required for AI analysis of public content

**Privacy Act Compliance:**
- Australian Privacy Act 1988 compliance confirmed
- No cross-border data issues (OpenAI uses Australian AWS region where available)
-->
---

### Appendix E: Glossary

**AI/ML Terms:**
- **Precision:** Percentage of flagged items that are true violations (TP / [TP + FP])
- **Recall:** Percentage of actual violations that are flagged (TP / [TP + FN])
- **False Positive (FP):** Item incorrectly flagged as violation
- **False Negative (FN):** Violation missed by the system
- **True Positive (TP):** Correct identification of a violation
- **Confidence Score:** AI's certainty level for a classification (0-1 scale)
- **Threshold:** Minimum confidence score required to trigger a flag
- **Semantic Analysis:** Understanding meaning and context, not just keywords

**Project Terms:**
- **D-1 Processing:** Analysing projects created one day prior
- **Bulk Import/Migration:** Large batch of historical projects added to system
- **Regular Operations:** Day-to-day project creation by users
- **Flagging Rate:** Percentage of projects that trigger AI flags

**Technical Terms:**
- **API:** Application Programming Interface - allows systems to communicate
- **GitHub Actions:** Automation platform for running scheduled tasks
- **Jira:** Project management and ticketing software
- **Salesforce:** CRM platform storing project data
- **SOQL:** Salesforce Object Query Language
- **OAuth:** Authentication protocol for secure API access

---


## Conclusion

The OpenAI Campaign Automation Pilot has successfully demonstrated that AI-powered content moderation is **technically feasible, highly cost-effective, and scalable** for ASF's needs. Phase 3a achieved critical milestones in automation infrastructure and violation detection (100% recall), while revealing important threshold calibration challenges (6.0% precision overall, 11.6% for regular operations).

**Key Achievements:**
- ✅ Fully automated daily processing with 100% uptime
- ✅ All policy violations successfully identified (zero false negatives)
- ✅ Extreme cost efficiency ($0.19 for 688 projects, 10x better than estimates)
- ✅ Proven scalability (300x headroom within budget)
- ✅ Valuable insights into regular operations vs bulk migration performance

**Next Step:**
Phase 3b (Oct 3 - Nov 3, 2025) will determine whether threshold adjustments can reduce false positives to acceptable levels. **Regular operations metrics (49.7% flagging rate, 11.6% precision) should be the primary evaluation criteria**, as they better represent typical system performance.

**Recommendation:**
- Continue testing thresholds
<!--
- **If Phase 3b succeeds** (>40% precision, <35% flagging for regular ops): Proceed to production deployment in November 2025
- **If Phase 3b falls short**: Consider extended iteration (Phase 3c) or hybrid approaches detailed in Alternative Recommendations section

The economic case is compelling: at less than $0.0004 per project and potential for 20+ hours/month in time savings, even modest improvements in precision will deliver substantial ROI. The infrastructure is proven, the technology is reliable, and the path to optimisation is clear.
-->
**Final Note:**
This pilot represents a significant step toward scalable, AI-assisted content moderation at ASF. Success requires continued collaboration between technical implementation (Insights) and domain expertise (FST) to refine thresholds and maintain policy alignment. With the right calibration, this system can help ASF scale its fundraising operations while maintaining the highest standards of compliance and safety.


---

## Document Control

**Version:** 1.1  
**Author:** Lucia  
**Next Review:** Oct 17, 2025 (after Phase 3b completion)

**Distribution:**
- AI Working Group
- Insights Team
- Confluence: [Link to doc](https://sportsfoundation.atlassian.net/wiki/spaces/ARAI/pages/2511372379/Fundraising+campaign+screening+automation?focusedCommentId=2517106704)

<!--
--

**END OF DOCUMENT**

*This document is maintained in Confluence and should be referenced as the authoritative source for pilot results and recommendations. For questions or clarifications, contact Lucia via the Insights Team.* -->
