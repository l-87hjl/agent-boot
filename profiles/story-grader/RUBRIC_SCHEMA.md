# Rubric Schema

## Schema Version
1.0.0

## Structure

```yaml
rubric:
  name: string
  version: string
  total_points: number
  sections:
    - name: string
      weight: number (0-1)
      criteria:
        - name: string
          points: number
          levels:
            - score: number
              description: string
```

## Example Rubric

```yaml
rubric:
  name: "Code Quality Assessment"
  version: "1.0"
  total_points: 100
  sections:
    - name: "Functionality"
      weight: 0.4
      criteria:
        - name: "Correctness"
          points: 25
          levels:
            - score: 25
              description: "All requirements met, no bugs"
            - score: 20
              description: "Most requirements met, minor bugs"
            - score: 10
              description: "Some requirements met, significant bugs"
            - score: 0
              description: "Does not function"

    - name: "Code Quality"
      weight: 0.3
      criteria:
        - name: "Readability"
          points: 15
          levels:
            - score: 15
              description: "Excellent clarity and organization"
            - score: 10
              description: "Good readability, minor issues"
            - score: 5
              description: "Difficult to follow"
            - score: 0
              description: "Unreadable"

    - name: "Documentation"
      weight: 0.3
      criteria:
        - name: "Comments"
          points: 10
          levels:
            - score: 10
              description: "Comprehensive, helpful comments"
            - score: 5
              description: "Adequate comments"
            - score: 0
              description: "No meaningful comments"
```

## Validation Rules

1. All weights must sum to 1.0
2. Scores must be non-negative
3. Levels must be in descending score order
4. Each criterion must have at least 2 levels
5. Descriptions must be non-empty

## Output Format

```json
{
  "submission_id": "string",
  "rubric_version": "string",
  "scores": {
    "section_name": {
      "criterion_name": {
        "score": "number",
        "max_score": "number",
        "level_matched": "string",
        "justification": "string"
      }
    }
  },
  "total_score": "number",
  "percentage": "number",
  "feedback": "string"
}
```
