## 📋 Executive Summary
This project transformed a raw, fragmented HR survey dataset into a high-performance, interactive **Diagnostic Dashboard**. The goal was to move beyond simple "Ad Hoc" data requests and provide HR leadership with a persistent "Tactical HUD" to monitor organizational health. 

By engineering a custom 5-star sentiment scale and utilizing advanced M-code for job-level classification, this dashboard identifies specific **"Conflict Zones"** (low-performing departments or questions) and allows for precision decision-making. The final result is a corporate-grade reporting tool that balances descriptive statistics (What is happening?) with diagnostic insights (Why is it happening?).

---

## 🛠️ Data Cleaning & Engineering Strategy
Before deployment, the data underwent a rigorous cleaning process to ensure maximum signal and minimum noise:

* **Source Migration:** Transitioned the data source from a multi-layered Excel Workbook to a flat CSV file for faster performance and better scalability.
* **Synthetic Data Generation:** To create a more robust 5-star distribution, the original `Response` column was updated using the `RANDBETWEEN(1,5)` function. This provided a realistic bell curve for sentiment analysis.
* **Column Pruning:** Deleted the redundant `Response Text` column to streamline the model and reduce "Chart Junk."
* **Sentiment Categorization:** Used a **Conditional Column** in Power Query to translate raw scores into actionable tactical categories:
    * **Negative:** Scores 1–2 (Requires immediate reinforcement)
    * **Neutral:** Score 3 (The stalemate/middle ground)
    * **Positive:** Scores 4–5 (High-morale zones)

---

## 💻 Power Query & M-Code Deep Dive
The "Engine Room" of this project involved custom M-code to handle complex organizational hierarchies. I wrote a custom `if...then...else` statement in the **Advanced Editor** to unify the job roles.

**Logic Explanation:**
```powerquery
each if [Director] = 1 then "Director" 
else if [Manager] = 1 then "Manager" 
else if [Supervisor] = 1 then "Supervisor" 
else "Staff"
