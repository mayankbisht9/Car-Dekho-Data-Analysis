DAX Measures Used in The DASHBOARD

Total Revenue = 
SUM('FACT_CAR_SALES'[selling_price])

Average Selling Price = 
AVERAGE('FACT_CAR_SALES'[selling_price])

Total Cars = 
COUNTROWS('FACT_CAR_SALES')

Market Health Score = 
VAR DiversityScore = DIVIDE([Active Brands], 50, 1) * 25
VAR VolumeScore = DIVIDE([Total Cars], 20000, 1) * 25
VAR PriceScore = IF([Average Selling Price] > 300000, 25, 
    DIVIDE([Average Selling Price], 300000) * 25)
VAR EfficiencyScore = DIVIDE([Average Mileage], 30, 1) * 25
RETURN ROUND(DiversityScore + VolumeScore + PriceScore + EfficiencyScore, 0)


Pricing Dynamics Page

DAX
Dealer Premium = 
CALCULATE([Average Selling Price], 'FACT_CAR_SALES'[seller_type] = "Dealer") -
CALCULATE([Average Selling Price], 'FACT_CAR_SALES'[seller_type] = "Individual")

Automatic Premium = 
CALCULATE([Average Selling Price], 'FACT_CAR_SALES'[transmission_type] = "Automatic") -
CALCULATE([Average Selling Price], 'FACT_CAR_SALES'[transmission_type] = "Manual")




## Sales & Valuation Page

DAX
Average Mileage = 
AVERAGE('FACT_CAR_SALES'[mileage])

Best Value Cars = 
CALCULATE([Total Cars], 
    'FACT_CAR_SALES'[price_per_seat] < [Average Price per Seat] &&
    'FACT_CAR_SALES'[mileage] > [Average Mileage])

