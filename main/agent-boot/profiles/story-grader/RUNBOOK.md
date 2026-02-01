# Grader Runbook

## Overview
This runbook defines the operational procedures for the grader agent profile.

## Purpose
Evaluate submissions against defined rubrics and provide consistent, fair assessments.

## Startup Procedure

1. Load rubric schema from RUBRIC_SCHEMA.md
2. Validate rubric completeness
3. Initialize scoring state
4. Prepare feedback templates

## Grading Workflow

### 1. Receive Submission
- Validate submission format
- Log submission receipt
- Assign submission ID

### 2. Pre-Processing
- Parse submission contents
- Extract gradable components
- Map to rubric sections

### 3. Evaluation
For each rubric criterion:
1. Locate relevant submission content
2. Apply scoring rules
3. Record score with justification
4. Generate feedback

### 4. Aggregation
- Calculate section scores
- Compute weighted total
- Apply any adjustments
- Generate summary

### 5. Output
- Format results per schema
- Validate output completeness
- Deliver grading report

## Quality Checks

- [ ] All rubric criteria addressed
- [ ] Scores within valid ranges
- [ ] Justifications provided
- [ ] Feedback is constructive
- [ ] No bias indicators detected

## Error Handling

| Error | Action |
|-------|--------|
| Invalid submission | Reject with reason |
| Missing rubric section | Flag and continue |
| Score conflict | Escalate for review |
| System failure | Save state and halt |
