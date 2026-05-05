# Findings and Recommendations — Pedestrian Analytics Pipeline

## Key Findings

### 1. Total Pedestrian Volume
- Over **574 million pedestrian movements** were recorded across all sensors
- This confirms very high levels of foot traffic in Melbourne's central city area

### 2. Peak Hours
- Morning peak: **8–9 AM** (commuter inflow)
- Evening peak: **5–6 PM** (commuter outflow)
- Midday activity is moderate with a steady decline after 7 PM

### 3. Busiest Locations
The top 5 busiest sensors were:

| Rank | Sensor ID | Location |
|------|-----------|----------|
| 1 | SouthB_T | Southbank |
| 2 | Swa31_T | Swanston Street |
| 3 | ElFi_T | Flinders Street |
| 4 | 261Will_T | William Street |
| 5 | QVN_T | Queen Victoria North |

### 4. Geographic Hotspots
- Highest density clusters appear in the **CBD**, particularly around:
  - Flinders Street Station
  - Swanston Street
  - RMIT University precinct
  - Southbank and Docklands (high during peak hours)

---

## Recommendations

### Infrastructure
- **Expand pedestrian pathways** and increase crosswalk capacity at Flinders Street, Swanston Street, and Southbank
- **Implement adaptive traffic signal control** during peak morning and evening hours to optimise pedestrian flow

### Safety
- **Enhance lighting and signage** in high-traffic zones
- **Schedule maintenance and safety checks** during off-peak hours (after 7 PM) to avoid disrupting peak flow

### Data Collection
- **Maintain 15-minute ingestion frequency** to ensure continuous, near real-time monitoring
- **Expand sensor network** to cover emerging high-traffic areas like Docklands

### Planning
- Use dashboard trend data for **long-term forecasting** of pedestrian volumes
- Integrate with **event scheduling data** to anticipate crowd surges during major city events
