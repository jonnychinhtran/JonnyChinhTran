
# Agency NB & Onboarding Goal Tracking

## Dashboard Objective  
The Agency NB & Onboarding Goal Tracking is used to review the engagement levels of appointed agencies and to conduct an overall assessment of onboarding progress.

## Business Purpose  

The **Appointed Agency Engagement v2** sheet helps the team review the Offer Bucket (base number and percentage of offers) of agencies, broken down monthly by location or individual agency:

Appointed agency engagement v2 can be viewed by:
- Appointment Year/Month
- ASM
- Domicile State
- Regional Director
 The **Agency Onboarding** sheet helps the team monitor Agency goals by Offer, Policy, and Bundle:
Agency onboarding can be viewed by:
- Appointment Year/Month
- ASM
- Domicile State
- Regional Director
- Network Name
- Days Left in Onboarding Period
- Offer Goal Flag
- NB Effective Policy Goal Flag
- NB Effective Bundle Goal Flag

The **Agency onboarding information V2** sheet allows the Agency team to review all onboarded agencies at a high level, such as by agency count by ASM or Domicile State.  ITD data is displayed, with a highlight on the current month and can be viewed by:
- Appointment Date
- ASM
- Domicile State
- Afinity Code
- Total Offers
- Total Policies Sold

## Data Sources 
- `branch-dbt-prod.dbt_sigma.sigma_agency_engagement` – ITD Agency Engagement Information.
- `branch-dbt-prod.dbt_sigma.sigma_agency_goal_tracking` – ITD Agency Goal Tracking Information.

## Key Logic & Assumptions  
n/a

## Metric Definitions & Calculations  
**Appointed Agency Engagement v2**:
| Metric Name  | Description  | Calculation Logic |
|-------------|-------------|-----------------------------------------|
| Monthly Buckets (by Monthly Offer Counts) - **Monthly Appointed Agencies** | Total appointed agencies | `Sum([appointed_agencies_count])` |
| Monthly Buckets (by Monthly Offer Counts) - **Running Total Appointed Agencies** | A count of the distinct agencies | `CountDistinct([agency_name])` |
| Monthly Buckets (by Monthly Offer Counts) - **20+ Offer Bucket** | A count of distinct agencies with more than 20 offer buckets | `CountDistinct([agency_name] if [offer_bucket] = "20+")` |
| Monthly Buckets (by Monthly Offer Counts) - **% 20+ Offer Bucket** | The percentage of the total distinct agencies that have more than 20 offer buckets | `CountDistinct([agency_name] if [offer_bucket] = "20+")/CountDistinct([agency_name])` |
| Monthly Buckets (by Monthly Offer Counts) - **1-19 Offer Bucket** | A count of distinct agencies with 1–19 offer buckets | `CountDistinct([agency_name] if [offer_bucket] = "1-19")` |
| Monthly Buckets (by Monthly Offer Counts) - **% 1-19 Offer Bucket** | The percentage of the total distinct agencies that have 1–19 offer buckets | `CountDistinct([agency_name] if [offer_bucket] = "1-19")/CountDistinct([agency_name])` |
| Monthly Buckets (by Monthly Offer Counts) - **Zero Offer Bucket** | A count of distinct agencies with zero offer buckets | `CountDistinct([agency_name] if [offer_bucket] = "0")` |
| Monthly Buckets (by Monthly Offer Counts) - **% Zero Offer Bucket** | The percentage of the total distinct agencies that have zero offer buckets | `CountDistinct([agency_name] if [offer_bucket] = "0")/CountDistinct([agency_name])` |

**Agency Onboarding**:
| Metric Name  | Description  | Calculation Logic |
|-------------|-------------|-----------------------------------------|
| Agency Goal Performance with First Login in "Selected Year" - **Days Left in Onboarding Period** </br> _(ex:  Agency Goal Performance with First Login in 2025)_ | The number of days left in onboarding period | `If([onboarding_evaluation_end_date] <= Today(), "90 Day Period Over", Text(DateDiff("day", Today(), [onboarding_evaluation_end_date]))) and if [affinity_code_first_login_year] is Selected Year` |
| Agency Goal Performance with First Login in "Selected Year" - **Offers Goal Flag** </br> _(ex:  Agency Goal Performance with First Login in 2025)_ | The status of offers goal flag | `If([d90_offer_count] >= [Set-Offer-Goal], "Goal Met", "Goal Not Met") and if [affinity_code_first_login_year] is Selected Year`
| Agency Goal Performance with First Login in "Selected Year" - **NB Effective Policy Goal Flag** </br> _(ex:  Agency Goal Performance with First Login in 2025)_ | The status of NB effective policy goal flag | `If([d90_written_policy_count] >= [Set-Policy-Goal], "Goal Met", "Goal Not Met") and if [affinity_code_first_login_year] is Selected Year`
| Agency Goal Performance with First Login in "Selected Year" - **NB Effective Bundle Goal Flag** </br> _(ex:  Agency Goal Performance with First Login in 2025)_ | The status of NB effective bundle goal flag | `If(CountDistinct([account_id] if [d90_has_auto_home_bundle] = true or [d90_has_auto_condo_bundle] = true) >= [Set-Bundle-Goal], "Goal Met", "Goal Not Met") and if [affinity_code_first_login_year] is Selected Year`
| Agency Goal Performance with First Login in "Selected Year" - *All Goals Flag** </br> _(ex:  Agency Goal Performance with First Login in 2025)_ | The status of all goal flag | `If(Both _Offers Goal Flag_ and _NB Effective Policy Goal Flag_ and _NB Effective Bundle Goal Flag_ are marked as "Goal Met", "Goal Met", "Goal Not Met") and if [affinity_code_first_login_year] is Selected Year`

**Agency Onboarding Information V2 sheet**:
| Metric Name  | Description  | Calculation Logic |
|-------------|-------------|-----------------------------------------|
| **"Current Month" + Agencies Onboarded** </br> _ex: Apr Agencies Onboarded_ | A count of the distinct agencies onboarded this month | `CountDistinct([agency_name] if [appointment_date] is the current month)` |
 | ITD Offers and Policies by Agencies - **Total Offers** | Total number of ITD offers | `Sum([itd_offer_count])`| 
 | ITD Offers and Policies by Agencies - **Total Policies Sold** | Total number of ITD policies sold | `Sum([itd_sold_policy_count])`|
 | Current Month Onboarded Agencies Offers and Policies - **Total Offers** | Total number of offers for the current month | `Sum([itd_offer_count]) where [appointment_date] is the current month` |
 | Current Month Onboarded Agencies Offers and Policies - **Total Policies Sold** | Total number of policies sold for the current month | `Sum([itd_sold_policy_count]) where [appointment_date] is the current month`|

## Refresh Cadence  
This dashboard is updated **daily**. The refresh schedule follows the times below, depending on whether it's **Standard Time** (EST) or **Daylight Savings Time** (EDT).

| Time Zone        | Standard Time (EST) | Daylight Savings Time (EDT) |
|------------------|---------------------|-----------------------------|
| **Eastern Time** | 8:00 AM (EST)       | 9:00 AM (EDT)               |

> **Note**: The schedule adjusts for Daylight Savings Time changes in Spring and Fall.

## Support & Requests  
For questions, reach out to **the Data Office**.  
For enhancement requests, please submit a [ticket request](<https://github.com/gobranch/dataoffice/issues/new/choose>).

## Links  
- [Sigma Dashboard](https://app.sigmacomputing.com/branch/workbook/Agency-NB-and-Onboarding-Goal-Tracking-1v4s63tOPVh09oZ3GKSsyK)
