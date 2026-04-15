# Data-Science
This contains the logic.md as well as the seed-data.json needed by backend.

VoucHER MVP – Credibility & Matching System
Overview
VoucHER is a rule-based credibility scoring and job-matching platform designed to connect Small-to-Medium Enterprises (SMEs) with relevant graphic design jobs while building trust through transparent, explainable algorithms.

How It Works
1. Credibility Score (0–100)
Each SME profile receives a transparent credibility score based on three factors:

Factor	Weight	What It Measures
Verification	20%	Business verification status (verified = 100, unverified = 0)
Reviews	60%	Average star rating converted to 0–100 scale
Completion Rate	20%	% of assigned jobs successfully completed
Example: An SME with verified status, 4.2-star rating, and 80% completion rate scores 86/100.

2. Job Matching
An SME sees a job only if ALL three conditions are met:

✅ Budget ≥ SME's minimum rate
✅ Job category matches SME's expertise
✅ SME's credibility score ≥ job's minimum requirement

3. Match Percentage Badge (Frontend)
Shows how well an SME fits the job:

Code
Match % = (SME Score / Job Min Score + Job Budget / SME Min Rate) ÷ 2
Capped at 100% for easy visualization.

Why Rule-Based?
Explainable: "Why wasn't I matched?" → Clear answer from the code
Fair: Prevents structural bias against informal but high-performing SMEs
Cold-start friendly: New SMEs default to 3.0 stars and 100% completion to avoid exclusion
Simple & trustworthy: No black-box AI—transparent logic builds confidence

Seed Data Included
20 Graphic Design SMEs with varying verification status, ratings, and completion rates
20 Open Jobs with budgets ranging from ₦300K–₦800K and score requirements of 60–82
