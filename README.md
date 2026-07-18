You are an Excel calculation and scorecard modeling expert.

Create a Production Score Calculator using the following logic.

OBJECTIVE
Calculate a Final Production Rate based on multiple production skills, their performance against target, and assigned weightages.

INPUT FIELDS PER SKILL

- Skill Name
- Actual Performance
- Production Hours
- Target Performance
- Weightage
- Previous Rating

REFERENCE TABLE

Each skill has rating thresholds for:
- Rating 5 (Maximum Rating)
- Rating 4
- Rating 3 (Baseline Rating = 1.0)
- Rating 2 (Minimum Rating)
- Rating 1

Example:

PART D AHT
Rating 5 = 1.1788
Rating 4 = 1.0892
Rating 3 = 1.0000
Rating 2 = 0.8500
Rating 1 = 0

FAX CPH
Rating 5 = 1.2727
Rating 4 = 1.1682
Rating 3 = 1.0000
Rating 2 = 0.8955
Rating 1 = 0

GLP-1 CPH
Rating 5 = 1.2733
Rating 4 = 1.1680
Rating 3 = 1.0000
Rating 2 = 0.8950
Rating 1 = 0

CALCULATION STEPS

STEP 1 – Performance Ratio

For each skill calculate:

PerformanceRatio = Actual / Target

Examples:
- Fax CPH = 12 / 11 = 1.090909
- GLP-1 CPH = 17 / 15 = 1.133333

STEP 2 – Determine Rating Band

Compare the PerformanceRatio against the reference thresholds.

Store:

- MaxRatingThreshold (Rating 5 threshold)
- MinRatingThreshold (Rating 3 threshold if above baseline)
  OR
- Rating 2 threshold if below baseline

The purpose is to determine where the employee falls between two rating levels.

STEP 3 – Calculate Fractional Progress

Calculate how far the current performance is between the lower and upper rating thresholds.

Formula:

FractionalProgress =
(PerformanceRatio - MinRatingThreshold)
/
(MaxRatingThreshold - MinRatingThreshold)

Limit the value to:

- Minimum = 0
- Maximum = 1

STEP 4 – Calculate New Production Rate Per Skill

Use linear interpolation between ratings.

Examples:

If performance is fully at Rating 3 level:
NewProdRatePerSkill = 3

If performance is fully at Rating 5 level:
NewProdRatePerSkill = 5

Formula:

NewProdRatePerSkill =
LowerRating +
(FractionalProgress × (UpperRating - LowerRating))

Round to desired precision.

STEP 5 – Calculate Skill Weight

Determine each skill's production contribution.

Formula:

SkillWeight =
ProductionHours
/
TotalProductionHours

or use the supplied Weightage field if already provided.

All skill weights must sum to 1.0000.

STEP 6 – Calculate Weighted Skill Score

WeightedSkillScore =
NewProdRatePerSkill × SkillWeight

STEP 7 – Calculate Final Production Rate

FinalProductionRate =
SUM(WeightedSkillScore for all skills)

Equivalent formula:

FinalProductionRate =
SUMPRODUCT(
NewProdRatePerSkill,
SkillWeight
)

OUTPUT REQUIRED

For each skill display:

- Skill Name
- Actual
- Target
- Performance Ratio
- Min Rating Threshold
- Max Rating Threshold
- Fractional Progress
- New Production Rate Per Skill
- Skill Weight
- Weighted Skill Score

Then display:

FINAL PRODUCTION RATE

VALIDATION RULES

- Weight totals must equal 100%
- Fractional Progress must be capped between 0 and 1
- Ratings cannot exceed the maximum rating value
- Ratings cannot fall below the minimum rating value
- Ignore skills with zero weightage in the final score

The calculation should dynamically update whenever Actual, Target, Hours, Weightage, or Reference Rating thresholds change.
