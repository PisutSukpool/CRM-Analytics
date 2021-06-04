# Customer Lifetime Value Dashboard

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/Old_Version/Main_2.png?raw=true)

tableau public link -> https://public.tableau.com/app/profile/pisut5343/viz/CLV_Dashboard/Dashboard1

## Step by Step

Summary each months -> No. of Customer , Total Spending , Average Order Value

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/Summary_each_month_2.png?raw=true)

Objective : Explore purchasing of customers to observe the changes in each month to be greater or less in volume.

--------------------
No. of Customer 

   Label : 
   
               {FIXED MONTH([Shop Date]) , YEAR([Shop Date]) : COUNTD([Cust Code])}
   
   Area Chart : 
   
               Row -> COUNTD([Cust Code])  , Column -> Day([Shop Date])

Total Spending

   Label : 
   
               Formula -> {FIXED  MONTH([Shop Date]) , YEAR([Shop Date]) : SUM([Spend]) }
   
   Area Chart : 
   
               Row -> SUM([Spend]) , Column -> Day([Shop Date])

Average Order Value

   Formula -> Total Spending / No. of Orders

   Label : 
         
               {FIXED MONTH([Shop Date]) , YEAR([Shop Date]) : SUM([Spend])/COUNTD([Basket Id])}
   
   Area Chart : 
   
               Row -> SUM([Spend])/COUNTD([Basket Id]) , Column -> Day([Shop Date])

--------------------
Average revenue per user (ARPU) per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/ARPU_per_Month.png?raw=true)
   
   Formula -> Total Revenue / No. of Customer
   
              SUM([Spend])/COUNTD([Cust Code])
   
--------------------
Churn Rate per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/ChurnRate_per_Month.png?raw=true)

   Formula ->  ((Clients at the beginning of a month - Clients at the end of a month) / Clients at the beginning of a month) x 100
   
              (WINDOW_SUM(COUNTD([Cust Code]),-1,-1)  -  WINDOW_SUM(COUNTD([Cust Code]),0,0))/WINDOW_SUM(COUNTD([Cust Code]),-1,-1)
   
--------------------
Average customer lifespan per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/ALT_per_Month.png?raw=true)

   Formula -> 1 / Churn Rate
   
              1 / [Churn Rate]
              
--------------------
Customer Lifetime Value per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/CLV_per_Month.png?raw=true)

   Define
   
   T = Average number of transactions per month
   
   AOV = Average order value
   
   AGM = Average gross margin = 10 % (Assume)
   
   ALT = Average customer lifespan in months
   
   Formula -> T x AOV x AGM x ALT
   
              [ARPU x AGM] x [Average Customer Lifespan]
              
              (SUM([Spend])/COUNTD([Cust Code]) x 0.1 ) x (1 / [Churn Rate])
              
--------------------
Customer Movement per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/CustMove_per_Month_2.png?raw=true)

| Customer Type | Description |
| ------------- | ------------- |
| New           | First Transaction of user|
| Repeat        | Previous Month and Current Month have transaction  |
| Reactivate    | They disappeared but come back again |
| Churn         | Previous Month has transaction but Current Month doesn't have transaction |

   Formula ->
    
             IF NOT  ISNULL(LOOKUP(ATTR( [Cust Code] ),-1)) AND NOT ISNULL(LOOKUP(ATTR( [Cust Code] ),0)) THEN 'Repeat'
             ELSEIF  NOT ISNULL(LOOKUP(ATTR( [Cust Code] ),-1)) AND ISNULL(LOOKUP(ATTR( [Cust Code] ),0)) THEN 'Churn'
             ELSEIF  ISNULL(LOOKUP(ATTR( [Cust Code] ),-1)) AND ISNULL(LOOKUP(ATTR( [Cust Code] ),0)) THEN ''
             ELSEIF  ISNULL(LOOKUP(ATTR( [Cust Code] ),-3)) AND ISNULL(LOOKUP(ATTR( [Cust Code] ),-2)) 
                     AND isnull(LOOKUP(ATTR( [Cust Code] ),-1)) AND NOT ISNULL(LOOKUP(ATTR( [Cust Code] ),0)) THEN 'New' 
             ELSE 'Reactivate'
             END

--------------------
Spending MTD vs Last Month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/MTDvsLM_2.png?raw=true)

Objective : Compare spending between current month and last month to take action product which spending in current month less than last month. this part can select dimension to change point of view.

   Spending Current Month : Formula ->
      
                                       WINDOW_SUM(sum([Spend]),0,0)
                                       

   Spending Last Month : Formula ->
      
                                       WINDOW_SUM(sum([Spend]),-1,-1)        
                                       
--------------------
Customer Segmentation by RFM

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/RFM_2.png?raw=true)

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
