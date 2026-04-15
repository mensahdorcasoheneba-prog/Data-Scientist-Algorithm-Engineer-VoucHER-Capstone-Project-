# VoucHER MVP – Credibility & Matching Logic

# Credibility Scoring Logic

## Objective

Generate a Credibility Score (0–100) for each SME profile.

This score determines:
- Marketplace visibility
- Eligibility for premium jobs
- Trust Passport visualization


## Why rule-based algorithm
We selected a rule-based credibility engine because the platform is in its early-stage and lacks sufficient historical data for predictive modeling. Additionally, the system must be fully explainable and allow transparency to support trust-building among underserved SMEs. If someone asks "Why didn't I get matched?", we can look at the code and say, "Because your score was 34 and the job needed 70.", it is self-explanatory and not complex which builds trust. A rule-based approach allows us to explicitly encode fairness constraints, mitigate cold-start exclusion, and prevent bias amplification while maintaining computational simplicity within the time constraints.


# Score Formula

Total Score (0-100) =  
(Verification_Status × 0.20) +  
(Review_Rating × 0.60) +  
(Completion_Rate × 0.20)

Each component is normalized to 0–100 before weighting.


# Breakdown of the different components:

| Component | Weight | Rationale |
|------------|--------|-----------|
| Verification | 20% | Encourages formalization but prevents exclusion |
| Reviews | 60% | Market-validated trust signal |
| Completion Rate | 20% | Delivery reliability measure |


## Verification Status (20%)

Verification is capped at 20% to prevent structural bias against
informal but high-performing SMEs.
Verification status is set only after backend validation.

IF Google_Vision_Match == TRUE:
    Verification_Raw = 100
ELSE:
    Verification_Raw = 0

Weighted Contribution:
Verification_Score = Verification_Raw × 0.20

Max Contribution: 20 points

-----------------------------------------------------------------------------------
### Verification Assumption:

The Data Science logic assumes that verification status
(is_verified = TRUE) is set only after secure backend validation
in accordance with Cybersecurity Team requirements.

The scoring engine does not validate documents directly.
-----------------------------------------------------------------------------------

## Review Rating (60%)

Working with 60% market-validated trust signal

Formula:
Review_Raw = (Average_Star_Rating / 5) × 100

Example:
4.0 stars → (4/5)*100 = 80 raw

Weighted:
Review_Score = Review_Raw × 0.60

Max Contribution: 60 points

Default Rule:
If no reviews → Average_Star_Rating = 0

## Completion Rate (20%)

Formula:
Completion_Raw = (Jobs_Completed / Jobs_Assigned) × 100

Example:
9 completed / 10 assigned = 90 raw

Weighted:
Completion_Score = Completion_Raw × 0.20

Edge Case Rule:
If Jobs_Assigned = 0:
    Completion_Raw = 100 (Benefit of doubt strategy)

# Final Score

Credibility_Score = 
Verification_Score +
Review_Score +
Completion_Score

We can round to nearest integer.

# Example of Calculation

SME Data:
is_verified = TRUE
average_rating = 4.2
jobs_completed = 8
jobs_assigned = 10

Step 1:
Verification = 100 × 0.20 = 20

Step 2:
Review = (4.2/5)*100 = 84
84 × 0.60 = 50.4

Step 3:
Completion = (8/10)*100 = 80
80 × 0.20 = 16

Total = 20 + 50.4 + 16 = 86.4
Final Score = 86

-----------------------------------------------------------------------------------
### Score Recalculation Events:
Credibility score must be recalculated when:
- is_verified changes
- average_rating changes
- jobs_completed changes
- jobs_assigned changes
-----------------------------------------------------------------------------------

# The Matching Algorithm

A Job is shown to SME if ALL conditions are met:

1. Job_Budget >= SME_Min_Rate
2. Job_Category == SME_Category
3. SME_Credibility_Score >= Job_Min_Score_Requirement

# Match Percentage (Frontend Badge)

Suggested Formula:

Match_Percentage = 
(Score_Fit + Budget_Fit) / 2

Where:
Score_Fit = SME_Score / Job_Min_Score
Budget_Fit = Job_Budget / SME_Min_Rate

Cap at 100%.
-----------------------------------------------------------------------------------
### Matching recalculation events:

- SME score changes
- Job budget changes
- Job minimum score changes
- SME minimum rate changes
-----------------------------------------------------------------------------------

If Job_Min_Score = 0,
Score_Fit = 1 (default).

If SME_Min_Rate = 0,
Budget_Fit = 1 (default).
-----------------------------------------------------------------------------------

## Fairness Constraint:

New SMEs without reviews shall default to:
average_rating = 3.0
completion_rate = 1.0 (if no assigned jobs)

This prevents cold-start exclusion.


# END OF DOCUMENT
