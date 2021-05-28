# Customer Lifetime Value Dashboard

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/Main.png?raw=true)

tableau public link -> https://public.tableau.com/app/profile/pisut5343/viz/CLV_Dashboard/Dashboard1

## Story 

Summary each months -> No. of Customer , Total Spending , Average Order Value

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/Summary_each_month.png?raw=true)
--------------------
No. of Customer 

   Label : Formula -> 
   
                      { FIXED MONTH([Shop Date]) , YEAR([Shop Date]) : COUNTD([Cust Code])}
   
   Area Chart : Row -> 
           
                      COUNTD([Cust Code])  , Column -> Day([Shop Date])

Total Spending

   Label : Formula -> 
    
                       {FIXED  MONTH([Shop Date]) , YEAR([Shop Date]) : SUM([Spend]) }
   
   Area Chart : Row -> 
   
                       SUM([Spend]) , Column -> Day([Shop Date])

Average Order Value

   Label : Formula -> Total Spending / No. of Orders
         
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

--------------------
Customer Lifetime Value per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/CLV_per_Month.png?raw=true)

--------------------
Customer Movement per month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/CustMove_per_Month.png?raw=true)

--------------------
Spending MTD vs Last Month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/MTDvsLM.png?raw=true)

--------------------
Customer Segmentation by RFM

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/RFM.png?raw=true)

--------------------
