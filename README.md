
# Veichi On-Grid Inverter Sizing Tool

This Python script assists with sizing and selecting Veichi on-grid inverters for solar PV systems. It allows users to input their energy consumption, solar resource availability, and panel specifications, then outputs the optimal inverter model and a compliant string and panel layout based on the inverter's voltage limits and power handling capabilities.

## Features

- Automatically selects the best-suited inverter model based on AC power output.
- Calculates:
  - Solar PV size required to meet daily energy needs.
  - Number of inverters required.
  - MPPT-compliant maximum and minimum string sizes.
  - Number of strings per inverter.
  - Total number of strings and total number of panels.
  - Total installed PV power.
- Adjustable parameters for location, panel specs, and energy coverage goals.
- Simple, well-commented Python code for ease of adaptation.

---

## Inputs

The script allows the following inputs (edit directly in the script):

- **`panel_watt`**: Power rating of each panel (W)
- **`panel_vmp`**: Voltage at maximum power (Vmp)
- **`panel_voc`**: Open circuit voltage (Voc)
- **`daily_consumption`**: Average daily electricity use in kWh
- **`percent_to_cover`**: Fraction of daily load to be covered by solar (e.g. 0.7 for 70%)
- **`sunshine_hours`**: Average peak sun hours per day at the installation site

Inverter models are pre-configured in the script with the following specifications:
- AC output power (kW)
- MPPT voltage window (min and max)
- Total number of string inputs

---

## Calculation Methodology

### Step 1: Required Solar Generation
```

solar\_kw\_needed = (daily\_consumption × percent\_to\_cover) ÷ sunshine\_hours

```
This gives the total solar PV capacity required to meet the desired share of daily consumption.

### Step 2: Inverter Selection
For each inverter, the number of units required is calculated:
```

inverter\_count = ceil(solar\_kw\_needed ÷ inverter\_AC\_output)

```
The inverter model requiring the fewest units is selected.

### Step 3: String Sizing
Each inverter has a defined MPPT voltage range. Panel Voc and Vmp values are used to compute string size limits:

- **Maximum String Size**:
```

max\_string\_size = floor(Max MPPT Voltage ÷ Panel Voc)

```

- **Minimum String Size**:
```

min\_string\_size = ceil(Min MPPT Voltage ÷ Panel Vmp)

```

### Step 4: String Count
Based on required PV power and selected inverter configuration:
```

strings\_per\_inverter = ceil((solar\_kw\_needed × 1000) ÷ (inverter\_count × panel\_watt × max\_string\_size))
total\_strings = inverter\_count × strings\_per\_inverter

```

### Step 5: Panel Count and PV Power
```

panels\_needed = total\_strings × max\_string\_size
total\_pv\_kw = panels\_needed × panel\_watt ÷ 1000

---

## Notes

* This tool assumes equal string lengths and uniform irradiation.
* Does **not** validate current limits (e.g., max input current or Isc x strings).
* Safety margins (temperature derating, oversizing buffers) are not included by default and should be added manually if needed.
* Use as a preliminary design tool only. Final system design should be verified with detailed engineering checks and Veichi documentation.

---
## Excel File 

https://webnicpremium-my.sharepoint.com/:x:/g/personal/christiaan_cedarsolar_com/EdkLuEVkvV9PpvzUBLQ_svAB1nP4k33ZG7p6nuXXOzJXmg?e=GbnzDg

