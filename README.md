
# Agency NB & Onboarding Goal Tracking

## Dashboard Objective  
The Agency Onboarding Report gives a high level overview of onboarded agencies.

## Business Purpose  
This dashboard allows the Agency team to review all onboarded agencies at a high level, such as by agency count by ASM or Domicile State.  ITD data is displayed, with a highlight on the current month.

Agency information can be viewed by:
- Appointment Date
- Attribution Source
- Affinity Code
- ASM
- Domicile State

## Data Sources 
- `branch-dbt-prod.dbt_sigma.sigma_agency_engagement` – ITD Agency Engagement Information.
- `branch-dbt-prod.dbt_sigma.sigma_agency_goal_tracking` – ITD Agency Goal Tracking Information.

## Key Logic & Assumptions  
n/a

## Metric Definitions & Calculations  
Appointed Agency Engagement v2:
| Metric Name  | Description  | Calculation Logic |
|-------------|-------------|-----------------------------------------|
| Monthly Buckets (by Monthly Offer Counts) - **Monthly Appointed Agencies** | Total appointed agencies | `Sum([appointed_agencies_count])` |
| Monthly Buckets (by Monthly Offer Counts) - **Running Total Appointed Agencies** | A count of the distinct agencies | `CountDistinct([agency_name])` |


Agency Onboarding Information V2 sheet:
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
