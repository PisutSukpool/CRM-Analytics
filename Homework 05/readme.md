# Customer Lifetime Value Dashboard

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/Dashboard.png?raw=true)

tableau public link -> https://public.tableau.com/app/profile/pisut5343/viz/CustomerLifetimeValueDashboard_v2/CustomerLifetimeValue

## Step by Step

## Aggregate Supermarket Dataset

Prepare on Bigquery

1.Customer Movement

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/z_prep_agg_dataset_1.png?raw=true)

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/z_prep_agg_dataset_2.png?raw=true)

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/z_prep_agg_dataset_3_2.png?raw=true)

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/z_prep_agg_dataset_4_2.png?raw=true)

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/z_prep_agg_dataset_6.png?raw=true)

2. Churn Rate & Average Customer Lifespan

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/z_prep_agg_dataset_7.png?raw=true)


## Create Dashboard
Summary each months -> No. of Customer , No. of Transaction , Total Spending , Average Order Value

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/ByMonth.png?raw=true)

Objective : Explore purchasing of customers to observe the changes in each month to be greater or less in volume.

--------------------
No. of Customer 

   Label : 
   
               {FIXED MONTH([Shop Date]) , YEAR([Shop Date]) : COUNTD([Cust Code])}
   
   Line Chart : 
   
               Row -> COUNTD([Cust Code])  , Column -> Day([Shop Date])

Total Spending

   Label : 
   
               Formula -> {FIXED MONTH([Shop Date]),YEAR([Shop Date]) 
                            : SUM(IF ISNULL([Cust Code]) THEN 0 ELSE [Spend] END)}
   
   Line Chart : 
   
               Row -> SUM([Spend]) , Column -> Day([Shop Date])

Average Order Value

   Formula -> Total Spending / No. of Orders

   Label : 
         
               {FIXED MONTH([Shop Date]),YEAR([Shop Date]) 
                  : SUM(IF NOT ISNULL([Cust Code]) THEN [Spend] ELSE 0 END)/
                    COUNTD(IF NOT ISNULL([Cust Code]) then [Basket Id] end)
               }
   
   Line Chart : 
   
               Row -> SUM([Spend])/COUNTD([Basket Id]) , Column -> Day([Shop Date])

--------------------
Average revenue per user (ARPU) per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/ARPU.png?raw=true)
   
   Formula -> Total Revenue / No. of Customer
   
              SUM([Spend])/COUNTD([Cust Code])
   
--------------------
Churn Rate per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/ChurnRate.png?raw=true)

   Formula ->  ((Clients at the beginning of a month - Clients at the end of a month) / Clients at the beginning of a month) x 100
         
              Row -> AVG(Churn Rate)     
              Column -> (DATEPART('year', [Shop Date])*100 + DATEPART('month', [Shop Date]))
   
--------------------
Average customer lifespan per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/ALT.png?raw=true)

   Formula -> 1 / Churn Rate
   
              Row -> AVG(ALT)     
              Column -> (DATEPART('year', [Shop Date])*100 + DATEPART('month', [Shop Date]))
              
--------------------
Customer Lifetime Value per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/CLV.png?raw=true)

   Define
   
   T = Average number of transactions per month
   
   AOV = Average order value
   
   AGM = Average gross margin = 10 % (Assume)
   
   ALT = Average customer lifespan in months
   
   Formula -> T x AOV x AGM x ALT
   
              [Average Revenue per User] x [AGM] x [Average Customer Lifespan]
              
              SUM([Spend])/COUNTD([Cust Code]) x 0.1 x AVG([ALT])
              
--------------------
Customer Movement per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/CustMove.png?raw=true)

| Customer Type | Description |
| ------------- | ------------- |
| New           | First transaction of user|
| Repeat        | Previous month has transaction and within 3 months there is still transaction |
| Reactivate    | Previous month has transaction and there is transaction after 3 months. |
| Churn         | Previous month has transaction but it doesn't have transaction for 3 months |

   Formula ->
             
             New = {FIXED [Month Year]        : AVG([New Cnt])}    
             Repeat = {FIXED [Month Year]     : AVG([Repeat Cnt])}    
             Reactivate = {FIXED [Month Year] : AVG([Reactivated Cnt])}
             Churn = {FIXED [Month Year]      : -AVG([Churn Cnt])}
             
             SUM([New]) , SUM([Repeat]) , SUM([Reactivate]) , SUM([Churn])

--------------------
Spending MTD vs Last Month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/MTDvsLM.png?raw=true)

Objective : Compare spending between current month and last month to take action product which spending in current month less than last month. this part can select dimension to change point of view.

   Spending Current Month : Formula ->
      
                                       WINDOW_SUM(sum([Spend]),0,0)
                                       

   Spending Last Month : Formula ->
      
                                       WINDOW_SUM(sum([Spend]),-1,-1)        
                                       
--------------------
Customer Segmentation by RFM

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/Segment.png?raw=true)

Objective : Choose customer to recommend product that spending in current month less than last month.

Segment Table

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/RFM_ref.png?raw=true)

Segmentation Reference : https://blog.rsquaredacademy.com/customer-segmentation-using-rfm-analysis/

   Recency (R) : Formula -> Change latest date that a customer has transaction to R Score (1-5)
   
                  IF ROUND(RANK_PERCENTILE([Max Shope Date])/0.2,0) = 0 THEN 1
                  ELSE ROUND(RANK_PERCENTILE([Max Shope Date])/0.2,0) END
                  
   Frequency (F) : Formula -> Change frequency that a customer come to shop and has transaction to F Score (1-5)
   
                  IF ROUND(RANK_PERCENTILE(COUNT([Shop Date]))/0.2,0) = 0 THEN 1
                  ELSE ROUND(RANK_PERCENTILE(COUNT([Shop Date]))/0.2,0) END
                  
   Monetary (M) : Formula -> Change average order value (AOV) to M Score (1-5)
   
                  IF ROUND(RANK_PERCENTILE([Average order value])/0.2,0) = 0 THEN 1
                  ELSE ROUND(RANK_PERCENTILE([Average order value])/0.2,0) END   
   
   Segment Condition :
   
                  IF         [R] >= 4 and [F] >= 4 and [M] >=4                                           then 'Champions'
                  ELSEIF     [R] >= 2 and [R] <= 4 and [F] >= 3 and [F] <= 4 and [M] >= 4                then 'Loyal Customers'
                  ELSEIF     [R] >= 3 and [F] >= 1 and [F] <= 3 and [M] >= 1 and [M] <= 3                then 'Potential Loyalist' 
                  ELSEIF     [R] >= 4 and [F] < 2  and [M] < 2                                           then 'New Customers'
                  ELSEIF     [R] >= 3 and [R] <= 4 and [F] < 2  and [M] < 2                              then 'Promising'
                  ELSEIF     [R] >= 3 and [R] <= 4 and [F] >= 3 and [F] <= 4  and [M] >= 3 and [M] <= 4  then 'Need Attention'
                  ELSEIF     [R] >= 2 and [R] <= 3 and [F] < 3  and [M] < 3                              then 'About To Sleep'
                  ELSEIF     [R] < 3 and [F] >= 2  and [M] >= 2                                          then 'At Risk'
                  ELSEIF     [R] < 2 and [F] >= 4  and [M] >= 4                                          then 'Can’t Lose Them'
                  ELSEIF     [R] >= 2 and [R] <= 3 and [F] >= 2 and [F] <= 3  and [M] >= 2 and [M] <= 3  then 'Hibernating'
                  ELSEIF     [R] < 2 and [F] < 2 and [M] < 2                                             then 'Lost'
                  ELSE                                                                                        'Other'
                  END
                  
--------------------
