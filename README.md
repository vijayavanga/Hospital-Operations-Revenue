🏥 Hospital Operations & Revenue Analysis
📊 Power BI Project
________________________________________
📌 Overview
This project analyzes hospital operations and revenue to understand:
●	What drives hospital revenue
●	How efficiently resources like beds are utilized
The analysis focuses on patient complexity (CMI), case types, and operational metrics such as Length of Stay (LOS), Revenue per Patient, and Revenue per Day.
________________________________________
🎯 Objectives
●	Identify key drivers of hospital revenue
●	Compare contribution of different patient groups
●	Evaluate operational efficiency
●	Provide actionable recommendations for resource optimization
________________________________________
📂 Dataset
●	Source: Kaggle (Simulated Hospital Dataset)
●	Granularity: Patient-level (each row = one case)
Key Fields:
●	Case Type (Inpatient, Outpatient, Day Case)
●	Revenue
●	Length of Stay (LOS)
●	Case Mix Index (CMI)
●	Specialty
●	Patient demographics
________________________________________
🧹 Data Cleaning & Preparation
✅ 1. 🔍 Data Quality & Integrity Check
During data validation, I performed a cardinality check on the primary key (Case No) and identified inconsistencies between distinct and unique counts, indicating duplicate records.
Further investigation revealed multiple Case IDs assigned to different individuals with conflicting attributes such as Year of Birth and Nationality.
Since the correct record could not be verified, all affected records (~0.8% of the dataset) were removed to maintain data integrity.
👉 Key Principle:
When data accuracy cannot be guaranteed, it is better to exclude unreliable records than risk incorrect analysis.

________________________________________
✅ 2. Created CMI Categories
Category	Range
Low	< 0.90
Medium	0.90 – 1.10
High	> 1.10
👉 Simplifies comparison across patient groups
________________________________________
✅ 3. Length of Stay Handling
●	Outpatient (OP) → LOS = 0
●	Day Case (DC) → LOS = 1
●	Inpatient (IP) → Variable LOS
📌 Decision:
 Average LOS calculated using inpatients only
👉 Reason:
 Including OP and DC would distort true hospital bed utilization
________________________________________
🧮 Key DAX Measures
📌 Average LOS (Inpatients Only)
Avg LOS IP =
AVERAGEX(
   FILTER(
       cleanedData,
       cleanedData[Case type] = "IP" &&
       cleanedData[LOS] > 0
   ),
   cleanedData[LOS]
)
________________________________________
📌 Revenue per Patient
Revenue per Patient =
DIVIDE(
   SUM(cleanedData[Revenue]),
   COUNT(cleanedData[Case_No])
)
________________________________________
📌 Revenue per Day
Revenue per Day =
DIVIDE(
   SUM(cleanedData[Revenue]),
   SUM(cleanedData[LOS])
)
________________________________________
📌 Revenue Percentage
Revenue % =
DIVIDE(
   SUM(cleanedData[Revenue]),
   CALCULATE(SUM(cleanedData[Revenue]), ALL(cleanedData))
)
________________________________________
📌 Correlation (CMI vs Revenue per Day)
Correlation r =
VAR MeanX = AVERAGEX(VALUES(cleanedData[Month]), [Avg CMI])
VAR MeanY = AVERAGEX(VALUES(cleanedData[Month]), [Revenue per Day])

VAR Numerator =
   SUMX(
       VALUES(cleanedData[Month]),
       ([Avg CMI] - MeanX) * ([Revenue per Day] - MeanY)
   )

VAR DenominatorX =
   SQRT(SUMX(VALUES(cleanedData[Month]), POWER([Avg CMI] - MeanX, 2)))

VAR DenominatorY =
   SQRT(SUMX(VALUES(cleanedData[Month]), POWER([Revenue per Day] - MeanY, 2)))

RETURN
DIVIDE(Numerator, DenominatorX * DenominatorY)
________________________________________
📌 R-Squared
R Squared = POWER([Correlation r], 2)
________________________________________
📌 MonthShort = UPPER(FORMAT(cleanedData[Month], "MMM"))
________________________________________
📌 PatientCountPercentage = 
DIVIDE(
    COUNT(cleanedData[Case_No]),
    CALCULATE(COUNT(cleanedData[Case_No]), ALL(cleanedData))
)
________________________________________
🛠️ Additional Modeling Decisions
●	Created custom month formatting to improve visual clarity
●	Used DAX-based percentage calculations instead of built-in options for better control and consistency
●	Focused on measure-driven modeling rather than relying on default aggregations
________________________________________
📊 Key Insights
________________________________________
💰 1. Revenue is Driven by Patient Complexity
●	High & medium complexity patients generate most of the revenue
👉 Insight:
 Revenue depends more on type of patients than volume
________________________________________
🏥 2. Inpatient Cases Dominate Revenue
●	~93% of total revenue comes from inpatient cases
👉 Insight:
 Inpatient care is the hospital’s primary financial driver
________________________________________
⚖️ 3. Volume vs Value
●	Outpatients → High volume, low revenue
●	Inpatients → Lower volume, high revenue
👉 Insight:
 Not all patient groups contribute equally
________________________________________
🔍 4. Opportunity: Low Complexity Inpatients
●	Low CMI patients present in inpatient category
👉 Insight:
 Potential to shift suitable cases to:
●	Day Case
●	Outpatient
👉 Impact:
 Improves bed utilization
________________________________________
⏱️ 5. Length of Stay
●	Slight increase with complexity
●	No major variation
👉 Insight:
 Higher complexity does not significantly increase LOS
________________________________________
📈 6. Revenue per Patient
●	Strong increase from low → high complexity
👉 Insight:
 Complex cases generate higher financial value
________________________________________
⚡ 7. Revenue per Day (Efficiency)
●	High & medium complexity show strong performance
👉 Insight:
 These groups balance value and efficiency
________________________________________
🔄 8. Bed Turnover vs Revenue
●	High turnover ≠ high revenue
👉 Insight:
 Case mix matters more than speed
________________________________________
📉 9. Statistical Insight (Regression Analysis)
●	Correlation (r) ≈ -0.19
●	R² ≈ 0.04
👉 Insight:
 Patient complexity does not strongly explain revenue efficiency
________________________________________
🔥 Key Interpretation
High complexity patients generate more revenue overall, but longer stays can limit revenue per day.
👉 Trade-off Identified:
●	💰 Value (Revenue per Patient)
 vs
●	⚡ Efficiency (Revenue per Day)
________________________________________
🧠 Key Learnings
●	Importance of choosing the right metrics
●	Impact of data filtering on analysis (LOS case)
●	Difference between total value vs efficiency
●	Using statistical methods to validate assumptions
________________________________________
🎯 Recommendations
●	Prioritize high & medium complexity inpatient cases
●	Shift low-complexity inpatient cases where appropriate
●	Improve discharge planning to reduce LOS
●	Focus on optimizing case mix, not just increasing volume
________________________________________
🏁 Conclusion
Hospital performance is not just about treating more patients, but about treating the right mix of patients efficiently.
This project demonstrates how combining business understanding with data analysis can lead to actionable insights in healthcare operations.
________________________________________
💡 About This Project
This project reflects my approach to data analysis:
●	Clean and validate data first
●	Focus on meaningful metrics
●	Validate findings using statistical thinking
●	Translate insights into business recommendations
________________________________________
🚀 Next Steps
●	Incorporate cost data for deeper profitability analysis
●	Explore readmission rates
●	Analyze trends at finer time granularity
________________________________________
⭐ If you found this project useful, feel free to connect or share feedback!

