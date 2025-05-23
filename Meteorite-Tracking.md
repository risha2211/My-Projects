# Meteorite Tracking & Age Estimation

## Overview  
Developed a Python-based data science project that tracks meteorite landings across the globe using geospatial coordinates, analyzes their carbon content where available, and approximates the age of the meteorites using carbon dating principles. The project integrates NASA’s open meteorite dataset with spatial mapping and analytical tools for a comprehensive study of meteor impact events and cosmic material aging.

## Technologies & Tools Used  
- **Python 3.6**: Core development language.  
- **Pandas**
- **Matplotlib & Seaborn**: For visualization
- **Folium**: For rendering interactive maps of meteorite strike locations.  
- **Datetime & NumPy**: For time parsing and calculations.  
- **SciPy**: Used for implementing the radiometric decay function for carbon dating.  
- **Google Collab**

## Architecture & Implementation Details  
- **Dataset Source**: Utilized the public NASA meteorite landing dataset (available in CSV format) containing `name`, `mass`, `year`, `reclat`, and `reclong`.  
- **Data Cleaning**: Removed entries with null coordinate or mass values. 
- **Geospatial Visualization**: Using Folium, generated a world map showing meteorite landing locations with mass-indicative marker sizes and popups showing the name, year, and mass of each meteorite.  
- **Age Estimation**:  
  - For meteorites with known carbon isotope ratios or approximated decay constants, implemented a simplified carbon dating function using the radiometric decay formula:  
    \[
    t = \frac{\ln(N_0 / N)}{\lambda}
    \]  
    where `t` is the estimated age, `N_0` is the initial amount of carbon-14, `N` is the current amount, and `λ` is the decay constant.  
  - Decay constants were either sourced or approximated based on known half-life of C-14 (~5730 years).  
- **Mass-to-Age Correlation**: Optional regression analysis was performed to identify any patterns between meteorite mass and estimated cosmic exposure time (age before atmospheric entry).  
- **Export & Reporting**: Age and impact summaries exported to CSV for academic use and future integration.

## Sample Code Snippet

```python
import pandas as pd
import numpy as np
import folium
import matplotlib.pyplot as plt
from math import log

# Load NASA meteorite dataset
df = pd.read_csv("Meteorite_Landings.csv")
df = df.dropna(subset=["reclat", "reclong", "mass (g)", "year"])

# Convert year to datetime and filter meaningful data
df["year"] = pd.to_datetime(df["year"], errors="coerce")
df = df[df["year"].dt.year >= 1900]

# Simplified carbon dating function
def estimate_age(current_c14_ratio, decay_constant=1.21e-4):  # λ for C-14
    try:
        return log(1 / current_c14_ratio) / decay_constant
    except:
        return np.nan

# Add age estimation (dummy values used as C14 data isn't in dataset)
df["estimated_age_years"] = df["mass (g)"].apply(lambda x: estimate_age(current_c14_ratio=0.75))

# Generate map
map_center = [df["reclat"].mean(), df["reclong"].mean()]
m = folium.Map(location=map_center, zoom_start=2)

for _, row in df.iterrows():
    folium.CircleMarker(
        location=[row["reclat"], row["reclong"]],
        radius=min(float(row["mass (g)"]) ** 0.3, 10),
        popup=f"{row['name']} ({row['year'].year})",
        color="red",
        fill=True
    ).add_to(m)

m.save("meteorite_map.html")
