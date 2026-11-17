# wind-turbine-SCADA-analysis
Power BI dashboard analysing one year of 10 minute wind turbine SCADA data

## Problem Statement
A wind turbine often produces less power than the manufacturer's power curve promises. This project analyses one year of 10-minute SCADA data from a single turbine to estimate how much energy was produced, how much was lost, and under which wind conditions the losses occurred.

## Dataset
Wind Turbine SCADA Dataset (Kaggle): https://www.kaggle.com/datasets/berkerisen/wind-turbine-scada-dataset

One turbine, 10-minute readings through 2018 (about 50,530 rows): active power, wind speed, theoretical power curve and wind direction.

## Tools Used
- Microsoft Excel (data cleaning, status flags, loss calculation)
- Power BI Desktop (DAX measures, 2-page dashboard)

## Method
1. Set negative active power to 0 (the turbine draws a small amount of power when idle).
2. Flagged each 10-minute interval:
   - Downtime/Curtailment: wind speed above 4 m/s and power at or below 0
   - Low Wind Idle: wind speed below 3.5 m/s
   - Normal: everything else
3. Converted 10-minute average power (kW) to energy (kWh) by dividing by 6.
4. Power curve loss = theoretical energy minus actual energy, counted only in Normal intervals and never below zero.
5. Grouped wind speed into 0.5 m/s bins and wind direction into 30-degree sectors for the charts.
6. Built the dashboard in Power BI.

## Key Findings
- Total energy produced: about [11,013] MWh
- Average wind speed: 7.56 m/s
- Estimated availability: 96.46% (a proxy, see Limitations)
- Energy lost versus the manufacturer's curve while running normally: about [1,142] MWh
- Operating breakdown: Normal 76.88%, Low Wind Idle 19.58%, Downtime/Curtailment 3.54%
- Monthly production was highest in August and March and lowest in July.
- Power curve: the gap between actual and theoretical power is widest at about 9.5-11.5 m/s, peaking near 10.5 m/s at roughly 450 kW. Above about 13 m/s both curves sit close to the rated power of about 3,600 kW.
- Wind direction: loss as a share of theoretical energy was highest for winds from the 0-30 degree sector (16.22%, 3,793 intervals) and lowest for the 180-210 degree sector (5.04%, 6,935 intervals). Some sectors with high percentages, such as 120-150 degrees (15.09%), rest on fewer than 500 intervals. Average wind speed also varies widely between sectors (from 5.04 m/s to 11.62 m/s), so the effect of direction cannot be separated from the effect of wind speed with this dataset.

## Limitations
- The dataset has no fault or status codes, so availability is an estimate and Downtime cannot be separated from Curtailment.
- Wind speed is measured by one nacelle anemometer, so readings are affected by the rotor.
- The dataset covers one turbine, so wake losses cannot be assessed. Direction effects could come from terrain, yaw misalignment or sensor error, and would need more data to confirm.

## Dashboard Screenshots
### Executive Overview
![Overview](Overview.png)

### Power Curve Analysis
![Power Curve Analysis](Power%20Curve%20Analysis.png)

## Files
- `WinSCADA Dashboard.pbix`: Power BI file
- `WinSCADA Dashboard.pdf`: exported dashboard
