Data Analytics Assessment

This repository contains SQL solutions for analyzing customer behavior based on a financial services dataset. The solutions address four business scenarios, each involving querying MySQL tables to generate insights.


Q1: High-Value Customers with Multiple Products

Scenario: The business wants to identify customers who have both a funded savings plan and a funded investment plan. This helps highlight cross-selling opportunities.

Approach:

-Joined the `users_customuser`, `savings_savingsaccount`, and `plans_plan` tables.
-Filtered records to only include those with a `transaction_status` of 'funded' for savings and `status_id` of 'funded' for investments.
-Counted the number of funded plans per customer and summed deposit amounts.
-Displayed customers with both savings and investment plans, sorted by total deposits.

Challenges:

-Ensuring accurate aggregation without duplicate joins.
-Filtering for only funded plans correctly using CASE and aggregate functions.


Q2: Frequency Segmentation

Scenario: Segment customers based on the average number of monthly transactions into High, Medium, or Low Frequency users.

Approach:

-Aggregated transactions per customer by month.
-Calculated each customer’s average monthly transaction count.
-Categorized users based on defined thresholds (>=10, 3-9, ≤2 transactions).
-Aggregated results by segment to get customer count and average transactions.

Challenges:

-Handling users with months of inactivity.
-Accurate use of `DATE_FORMAT` and `AVG()` over monthly transaction counts.


Q3: Account Inactivity Alert

Scenario: Identify accounts (savings or investments) with no inflow transactions in the last 365 days.

Approach:

-Queried `savings_savingsaccount` and `plans_plan` separately for accounts with transaction or withdrawal dates older than a year.
-Combined results using `UNION ALL`.
-Calculated `inactivity_days` using `DATEDIFF()`.

Challenges:

-Handling NULL transaction/withdrawal dates.
-Ensuring both account types are included with consistent column formatting.


Q4: Customer Lifetime Value (CLV) Estimation

Scenario: Estimate CLV based on tenure and transaction volume, assuming profit per transaction is 0.1% of transaction value.

Approach:

-Calculated tenure using `TIMESTAMPDIFF(MONTH, date_joined, CURDATE())`.
-Summed all `confirmed_amount` from savings accounts as total transaction value.
-Calculated average profit per transaction (0.001 \* total transaction value / total transactions).
-Used the formula: `(total_transactions / tenure_months) * 12 * avg_profit_per_transaction` to estimate CLV.

Challenges:

-Handling accounts with very short tenure (to avoid division by zero).
-Dealing with currency stored in kobo (conversion required).


All queries are written in MySQL and organized into separate files.
