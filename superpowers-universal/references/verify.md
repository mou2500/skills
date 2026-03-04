# Phase 5: VERIFY - Check Quality

## Overview

**Goal:** Ensure work meets original intent before declaring complete.

**When to Use:** When all tasks appear complete.

**Success Criteria:** Work approved for completion.

## The Process

### Step 1: Check Completeness

**Question:** Did we do everything we said we would?

**Checklist:**
- [ ] All requirements from CLARIFY addressed
- [ ] All tasks from PLAN completed
- [ ] All outputs produced
- [ ] Nothing accidentally omitted

**Method:** Compare against original requirements and plan.

### Step 2: Verify Correctness

**Question:** Is the work accurate and sound?

**Checklist:**
- [ ] Facts are accurate
- [ ] Logic is sound
- [ ] No contradictions
- [ ] No obvious errors
- [ ] Calculations correct (if applicable)

**Method:** Review content for accuracy.

### Step 3: Assess Quality

**Question:** Does it meet the success criteria?

**Checklist:**
- [ ] Meets success criteria from CLARIFY
- [ ] Appropriate for intended audience
- [ ] Professional standard
- [ ] Well-crafted (not just functional)

**Method:** Compare against success criteria.

### Step 4: Confirm Intent Alignment

**Question:** Does this actually solve the original problem?

**Checklist:**
- [ ] Addresses the "why" from CLARIFY
- [ ] Matches user's implicit expectations
- [ ] Would satisfy the original need
- [ ] No scope drift

**Method:** Return to CLARIFY phase notes, check alignment.

## Two-Stage Review (For Important Work)

### Stage 1: Spec Compliance Review

**Focus:** Does output match what was designed?

**Questions:**
- Are all requirements implemented?
- Was anything added that wasn't asked for?
- Was anything omitted?
- Does structure match design?

**Example:**
```
**Spec Compliance Check:**

**Required (from DESIGN):**
- Opening with partnership acknowledgment ✓
- Clear delay statement ✓
- Brief technical context ✓
- Mitigation actions ✓
- Extension request ✓
- Quality commitment ✓

**Not Required but Added:**
- None

**Verdict:** Matches spec ✓
```

### Stage 2: Quality Review

**Focus:** Is it well-crafted?

**Questions:**
- Is it clear and understandable?
- Is the tone appropriate?
- Are there improvements possible?
- Would it benefit from another perspective?

**Example:**
```
**Quality Review:**

**Clarity:** ✓ All points are clear
**Tone:** ✓ Professional but warm
**Language:** ✓ No jargon, concise
**Flow:** ✓ Logical progression

**Potential Improvements:**
- Could add specific date in opening (minor)

**Verdict:** High quality, ready to send
```

## Verification Checklist Template

```
## Verification Report

### Completeness
- [ ] Requirement 1: [Status]
- [ ] Requirement 2: [Status]
- [ ] Requirement 3: [Status]

**Verdict:** [Complete / Incomplete]

---

### Correctness
- [ ] Facts verified: [Status]
- [ ] Logic checked: [Status]
- [ ] No contradictions: [Status]

**Verdict:** [Correct / Issues found]

---

### Quality
- [ ] Meets success criteria: [Status]
- [ ] Appropriate for audience: [Status]
- [ ] Professional standard: [Status]

**Verdict:** [High quality / Needs improvement]

---

### Intent Alignment
- [ ] Solves original problem: [Status]
- [ ] Matches expectations: [Status]
- [ ] No scope drift: [Status]

**Verdict:** [Aligned / Misaligned]

---

## Overall Assessment

**Status:** [Ready / Needs work]

**Issues Found:** [List or "None"]

**Recommendation:** [Approve / Revise]
```

## Examples

### Example 1: Email Verification

```
## Email Verification

### Completeness ✓
- [x] Informs of delay
- [x] Requests extension
- [x] Explains reason (brief)
- [x] Shows mitigation actions

### Correctness ✓
- [x] New date is accurate
- [x] Technical explanation is factual
- [x] No contradictions

### Quality ✓
- [x] Professional but warm tone
- [x] Appropriate length (< 300 words)
- [x] Clear ask
- [x] Technical detail minimal as requested

### Intent Alignment ✓
- [x] Maintains trust (primary goal)
- [x] Positions as quality investment
- [x] Clear extension request

## Overall Assessment

**Status:** Ready to send

**Issues Found:** None

**Recommendation:** Approve
```

### Example 2: Analysis Verification

```
## Analysis Verification

### Completeness ✓
- [x] Data analyzed (Q2 sales)
- [x] Hypotheses tested (competitor, pricing)
- [x] Alternative causes explored
- [x] Top 3 causes identified and ranked

### Correctness ✓
- [x] Data sources reliable
- [x] Correlations valid
- [x] Conclusions supported by evidence
- [x] No logical fallacies

### Quality ✓
- [x] Clear presentation
- [x] Evidence for each claim
- [x] Actionable insights
- [x] Appropriate depth

### Intent Alignment ✓
- [x] Identifies causes (not solutions)
- [x] Ranked by impact
- [x] Sufficient for decision-making

## Overall Assessment

**Status:** Ready for presentation

**Top 3 Causes:**
1. Competitor launch (40% impact)
2. Pricing sensitivity (35% impact)
3. Sales team turnover (25% impact)

**Recommendation:** Approve
```

### Example 3: Plan Verification

```
## Event Plan Verification

### Completeness ⚠️
- [x] Venue selected
- [x] Agenda designed
- [x] Logistics planned
- [ ] Budget breakdown (MISSING)
- [x] Timeline created

### Correctness ✓
- [x] Dates are feasible
- [x] Venue capacity sufficient
- [x] Timeline realistic

### Quality ✓
- [x] Detailed enough to execute
- [x] Clear responsibilities
- [x] Verification checkpoints included

### Intent Alignment ✓
- [x] 70/30 strategy/team building split
- [x] Within budget range
- [x] Achieves stated goals

## Overall Assessment

**Status:** Needs minor addition

**Issues Found:**
- Missing detailed budget breakdown

**Recommendation:** Add budget section, then approve

**Next Step:** Create budget breakdown task
```

## Handling Verification Outcomes

### If Everything Checks Out

```
**Verification Complete ✓**

All checks passed. Work is:
- Complete
- Correct
- High quality
- Aligned with intent

**Ready for delivery/completion.**
```

### If Minor Issues Found

```
**Verification Complete with Minor Issues**

**Issues:**
1. [Minor issue 1]
2. [Minor issue 2]

**Recommendation:** Quick fixes (5 minutes), then approve.

Shall I make these adjustments?
```

### If Major Issues Found

```
**Verification Found Major Issues**

**Critical Issues:**
1. [Issue 1 with impact]
2. [Issue 2 with impact]

**Recommendation:** Return to EXECUTE phase to address.

**Estimated Fix Time:** [X minutes/hours]

Proceeding with fixes...
```

## Common Verification Failures

### Scope Drift

**Symptom:** Output includes things not requested

**Fix:** Remove extras, return to original scope

### Missing Requirements

**Symptom:** Original requirements not addressed

**Fix:** Return to EXECUTE, complete missing parts

### Quality Issues

**Symptom:** Meets requirements but poorly executed

**Fix:** Refine and polish

### Intent Misalignment

**Symptom:** Technically correct but doesn't solve problem

**Fix:** Return to CLARIFY, reassess understanding

## Anti-Patterns

### ❌ Self-Congratulation Without Checking

```
"Done! This looks great!"
```

**Problem:** No actual verification performed

### ✅ Systematic Verification

```
"Let me verify this against requirements...

[Perform checks]

Verification complete. All checks passed."
```

### ❌ Declaring Done When Issues Exist

```
"There are a few small issues but overall it's done."
```

**Problem:** "Done" means done, not "mostly done"

### ✅ Honest Assessment

```
"Found 2 issues during verification. Fixing now, then will re-verify."
```

## Re-Verification Loop

If issues found:

```
VERIFY → Issues found → EXECUTE (fix) → VERIFY (check again)
```

**Keep looping until verification passes.**

## Transition to Complete

**When to Finish:**
- All verification checks passed
- No issues remaining
- User approval (if needed)

**How to Finish:**
```
**Work Complete ✓**

**Deliverables:**
- [List outputs]

**Verification Summary:**
- Completeness: ✓
- Correctness: ✓
- Quality: ✓
- Intent Alignment: ✓

**Ready for [delivery/use/next steps].**
```

## Quick Checklist

- [ ] Checked completeness
- [ ] Verified correctness
- [ ] Assessed quality
- [ ] Confirmed intent alignment
- [ ] Two-stage review (if important work)
- [ ] All issues resolved
- [ ] Work approved for completion

## Remember

**Verification is not optional.**

Declaring work complete without verification is:
- Unprofessional
- Risky
- Disrespectful to the user

**The Rule:** If you haven't verified, you're not done.

**The Standard:** "Done" means verified and approved, not "I finished the tasks."
