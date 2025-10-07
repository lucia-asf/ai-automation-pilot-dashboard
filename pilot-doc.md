
## Document Control

**Version:** 1.0  
**Author:** Lucia  
**Contributors:** FST Team (Stacie, Amy, Andrew)   
**Reviewed:**   
**Next Review:** November 3, 2025 (after Phase 3b completion)

**Distribution:**
- FST Team
- Insights Team
- Confluence: [Link to doc](https://sportsfoundation.atlassian.net/wiki/spaces/ARAI/pages/2511372379/Fundraising+campaign+screening+automation?focusedCommentId=2517106704)

### OpenAI Campaign Automation Pilot Documentation

**Project:** FST Fundraiser Review Board (FRB) AI Moderation  
**Period:** May 1 - November 3, 2025  
**Status:** Phase 3a Complete | Phase 3b In Progress  
**Last Updated:** October 7, 2025

---

## Executive Summary

The FST team requires an automated workflow to support fundraising campaign and page content moderation. As the Australian Sports Foundation continues to scale, human-based content moderation alone cannot meet safety, regulatory, and operational needs. This pilot demonstrates that AI-powered content moderation using OpenAI's API can help the FST team reclaim time currently spent on manual review.

**Phase 3a Results (Sep 5 - Oct 3, 2025):**
- **688 projects created** during pilot period
- **453 projects flagged** by AI (65.8% flagging rate)
- **15 confirmed violations** identified (3.3% true positive rate)
- **216 false positives** (47.7% of flagged content)
- **6.5% precision rate** indicates significant over-flagging requiring threshold refinement

**Key Finding:** While the AI successfully identified all policy violations, the high false positive rate (47.7%) created unsustainable manual review overhead. Threshold adjustments implemented on October 2, 2025 are being evaluated in Phase 3b.

**Recommendation:** Continue to Phase 3b with tightened thresholds. Evaluate for 1 month before deciding on full production deployment.

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
| **Phase 3a: Full Moderation v1** | Sep 5 - Oct 3 | ✅ Complete | AI classification with sentiment analysis, full results analyzed |
| **Phase 3b: Full Moderation v2** | Oct 3 - Nov 3 | 🔄 In Progress | Refined thresholds, reduced false positives (results pending) |

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
- **Primary:** FST team (Stacie Brand, Amy Bracher, Andrew Merritt)

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

**Only Content Data Analyzed:**
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
- ✅ Daily automated execution without manual intervention
- ✅ All policy violations identified (100% recall on confirmed cases)
- ⚠️ Acceptable false positive rate (<20% target, 47.7% actual)
- ⚠️ Precision rate (>50% target, 6.5% actual)
- ✅ Processing time <24 hours for all projects

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
Skewed due to migration projects.
- **Total projects created:** 688 (across 38 days)
- **Total tickets flagged:** 453 (65.8% flagging rate)
- **Average projects per day:** 18.1
- **Average tickets per day:** 14.2
- **Peak days:** Sep 17 (188 projects), Sep 29 (214 projects) - due to migration

**Performance Metrics (as of Oct 2, 2025):**
| Metric | Count | Percentage |
|--------|-------|------------|
| **True Positives** (In Progress → Done) | 15 | 3.3% of total projects |
| **False Positives** (Direct to Done) | 216 | 47.7% of flagged |
| **Currently Under Review** (In Progress) | 39 | 8.6% of flagged |
| **Awaiting Review** (To Do) | 183 | 40.4% of flagged |
| **Precision Rate** | 15/231 resolved | **6.5%** |
| **False Positive Rate** | 216/453 flagged | **47.7%** |


**Flagging Rate Analysis:**
- **Overall flagging:** 65.8% (453 of 688 projects)
- **Average daily flagging:** 85.7% (when projects were created)
- **Range:** 7.7% (best day) to 166.7% (low-volume days with statistical noise)
- **Bulk import days:** Sep 17 (68.1%), Sep 29 (86.4%)

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
- **Budget utilization:** 0.18% of $120/month October budget

**Time Savings:**
- **Manual review time saved:** Estimated XX hours/week by auto-clearing compliant projects
- **However:** High false positive rate (216 cases) created ~XX hours of unnecessary review
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
- 235 projects not flagged (34.2%) - saved ~XX hours
- 453 projects flagged requiring review
- 216 false positives = unnecessary ~XX hours
- **Net result:** Minimal/no time savings

---

### Phase 3b: Full Moderation v2 (Oct 3 - Nov 3, 2025)

**Objective:** Reduce false positive rate through threshold refinement

**Status:** 🔄 In Progress

**Changes Implemented (Oct 2, 2025):**
- Tightened confidence thresholds across all categories
- Reduced sensitivity by 30-40% based on Phase 3a learnings
- Adjusted keyword lists based on false positive analysis
- Refined "Personal Contact" category (highest FP rate in 3a)

**Expected Outcomes:**
- Target false positive rate: <20%
- Target precision rate: >40%
- Maintain 100% recall on policy violations

**Evaluation Period:** 1 month (until November 3, 2025)

**Metrics to Track:**
- Daily flagging rate
- Precision and recall
- False positive reduction
- User satisfaction (FST team feedback)
- Time savings vs manual review

**Results:** *To be added after Phase 3b completion*

---

## Challenges & Limitations

### Technical Challenges

**1. Over-flagging / Low Precision (6.5%)**
- **Issue:** AI flagged 93.5% of content incorrectly
- **Root cause:** Initial thresholds too sensitive, model detecting legitimate fundraising language
- **Impact:** Created more work than it saved
- **Resolution:** Threshold tightening in Phase 3b

**2. Confidence Score Miscalibration**
- **Issue:** Model most confident on false positives, least confident on true violations
- **Evidence:** 0% of true positives had >0.9 confidence
- **Impact:** Cannot rely on confidence scores for prioritization
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
- AI successfully identified all 15 confirmed policy violations
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
- Initial thresholds created 47.7% false positive rate
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
- Expanded fields analyzed (Story, Summary, Messages)

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
- Average 24 projects/day analyzed
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
- **Budget utilization:** Using only 0.18% of $120/month budget
- **Scalability headroom:** Can handle **300x current volume** within existing budget

**Cost at Scale:**
| Volume Multiplier | Annual Cost | Within Budget? |
|-------------------|-------------|----------------|
| Current (688 projects/month) | $2.48 | ✅ Yes ($0.20/month) |
| 2x volume (1,376 projects/month) | $4.95 | ✅ Yes ($0.41/month) |
| 5x volume (3,440 projects/month) | $12.38 | ✅ Yes ($1.03/month) |
| 10x volume (6,880 projects/month) | $24.77 | ✅ Yes ($2.06/month) |

**Benefits (Projected after Phase 3b optimization):**
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

## Recommendations & Next Steps

### Phase 3b Evaluation (Nov 3, 2025)

**Next Steps:**

✅ **Proceed to Full Production if:**
- Precision rate >40%
- False positive rate <20%
- FST team satisfaction score >X/10
- Net time savings >X hours/month

⚠️ **Additional Iteration Needed if:**
- Precision <40%%
- FST team identifies specific threshold adjustments
- Cost remains <$5/month

### Proposed Timeline for Next Phase

**Phase 4: Production Deployment (If Phase 3b Successful)**

| Milestone | Date | Activities |
|-----------|------|------------|
| Phase 3b Results | Nov 3, 2025 | Analyze 1 month of data with new thresholds |
| Next Steps | Nov 10, 2025 | Review results with stakeholders |
| Production Prep | Nov 11-24 | Documentation, training, monitoring setup |
| Full Rollout | Dec 1, 2025 | Switch to production mode |
| 30-Day Review | Jan 1, 2026 | Assess production performance |

### Resource Needs for Scale

**Technical:**
- ✅ Current infrastructure sufficient
- ✅ OpenAI API account (already setup)
- ✅ GitHub Actions (within free tier)

**Human:**
- ✅ 3 FST reviewers (current team adequate)
- ⚠️ Designate 1 "AI Champion" for threshold monitoring (X hrs/month)
- ⚠️ Brief training session for any new FST team members

**Budget:**
- **Year 1:** $2.48 OpenAI + $0 GitHub = **$2.48 total operational cost**
- **Year 2+:** $2.50-5.00 depending on volume growth
- **Monitoring tools (optional):** $0-20/month
- **Budget allocated:** $120/month available
- **Actual utilization:** 0.2% ($0.20 of $120)


### Clear Recommendation

**✅ Proceed with Phase 3b as planned**, then make final decision in November 2025 based on:

1. **If precision >40% in Phase 3b:** Deploy to production in December with monthly monitoring
2. **If precision <40%:** Run Phase 3c with further refinements for 1 more month

**Confidence Level:** Moderate (70%)
- Threshold adjustments show promise
- Cost is negligible ($22/year)
- FST team willing to continue testing
<!-- - Main risk: Model limitations may be fundamental rather than threshold-related -->

---

## Appendices

### Appendix A: Complete Phase 3a Data

**Link to Interactive Dashboard:**  
[GitHub Pages - AI Pilot Dashboard](https://lucia-asf.github.io/ai-automation-pilot-dashboard/)

**Jira Export:** See attached `Jira.csv` (453 tickets)  
**Salesforce Export:** See attached `pilot_projects_new_created.csv` (688 projects)

### Appendix B: Changelog

| Date | Change | Rationale |
|------|--------|-----------|
| Jun 3, 2025 | Added keyword list | FST team request for specific terms |
| Jun 18, 2025 | Implemented GitHub Actions | Enable daily automation |
| Aug 1, 2025 | Extended keyword flagging to Messages field | Catch violations in supporter messages |
| Aug 22, 2025 | Script revision for field coverage | Ensure all text fields analyzed |
| Sep 5, 2025 | Added sentiment analysis | Detect tone and intent beyond keywords |
| Sep 9, 2025 | Version control via GitHub | Track all script changes |
| Oct 2, 2025 | Tightened all thresholds | Reduce Phase 3a false positives |

### Appendix C: Sample Jira Tickets

**True Positive Example:**
```
Summary: [Moderation] PE-011115 — Inducement (0.85) — 2025-10-03
Category: Inducement
Confidence: 0.85
Severity: High
Evidence: "We need to raise funds ASAP to cover his $750 deposit, 
which is due October 15, 2025, to secure his spot."
Resolution: Contacted fundraiser to revise campaign language
```

**False Positive Example:**
```
Summary: [Moderation] PE-011136 — Sponsorship (0.62) — 2025-09-25
Category: Sponsorship
Confidence: 0.62
Severity: Medium
Evidence: "Thank you to our sponsors for supporting our team"
Resolution: Within guidelines. No action required.
```

### Appendix D: Industry Benchmarks

**Comparison to Similar Platforms:**

| Platform | Approach | Precision Rate | Notes |
|----------|----------|----------------|-------|
| **Kickstarter** | ML + Human Review | ~40-60% | Fast-track obvious cases |
| **GoFundMe** | ML Filters + 24/7 Team | ~35-50% | Community reporting supplement |
| **ASF Phase 3a** | OpenAI API + Human Review | 6.5% | Initial deployment, needs tuning |
| **ASF Target** | OpenAI API + Human Review | 40%+ | Post-optimization goal |

**Key Takeaway:** Our initial 6.5% precision is below industry standard but expected for first deployment. Target of 40% aligns with industry norms.

---
