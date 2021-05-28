# Customer Lifetime Value Dashboard

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/Main.png?raw=true)

tableau public link -> https://public.tableau.com/app/profile/pisut5343/viz/CLV_Dashboard/Dashboard1

## Story 

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2005/Summary_each_month.png?raw=true)

No. of Customer 
   Label : Formula -> { FIXED month([Shop Date]) , year([Shop Date]) : COUNTD([Cust Code])}
   Area Chart : Row -> COUNTD([Cust Code]  , Column -> Day([Shop Date])
