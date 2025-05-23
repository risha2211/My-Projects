# Geospatial Applications (Maps)

## Overview  
Developed a robust Python-based geospatial application capable of visualizing and tracking any location worldwide using precise latitude and longitude coordinates. The project integrates multiple Python libraries to create an interactive, layered map with spatial analysis capabilities- all running locally or inside notebooks.

## Technologies & Tools Used  
- **Python 3.6**: Core programming language 
- **Folium**: For creating interactive Leaflet.js maps with Python to render map tiles, markers, and popups.  
- **Geopandas**: Extended Pandas for geospatial data handling to read, manipulate, and visualize shapefiles and GeoJSON data.  
- **Shapely**: For geometric operations - buffering, intersection, and distance calculation on map features.  
- **Pandas**: For preprocessing coordinate datasets, handling CSV inputs, and data cleaning.  
- **Requests**: To pull external map data and real-time geospatial data from OpenStreetMap's API.  
- **Pyproj**: To align transformations properly on the map.  
- **Jupyter Notebooks** & **Google Colab** (Used in later iterations)

## Architecture & Implementation Details  
- **Data Input**: Coordinates and datasets were input via CSV files or user input in notebooks.  
- **Coordinate Handling**: Coordinates were converted between geographic (lat/lon) and projected coordinate systems using Pyproj.  
- **Map Rendering**: Folium created base maps with OpenStreetMap, custom markers and popups displayed metadata linked to each coordinate.  
- **Layer Management**: Multiple feature layers (points, polygons, heatmaps) managed through Folium’s `FeatureGroup` and `LayerControl` classes, allowing toggling.  
- **Spatial Operations**: Using Shapely and Geopandas, implemented buffering zones around points and distance calculations between coordinates.  
- **Dynamic Visualization**: Incorporated heatmaps (via `folium.plugins.HeatMap`) to visualize cluster density based on input data.  
- **Export & Sharing**: Generated standalone HTML files for sharing or embedding.  

## Sample Code Snippet

```python
import folium
from folium.plugins import HeatMap
import geopandas as gpd
from shapely.geometry import Point
import pandas as pd
from pyproj import Transformer

# Load coordinate data
df = pd.read_csv('coordinates.csv')
gdf = gpd.GeoDataFrame(df, geometry=gpd.points_from_xy(df.longitude, df.latitude))

# Reproject coordinates to metric system for spatial calculations
transformer = Transformer.from_crs("epsg:4326", "epsg:3857", always_xy=True)
gdf['geometry'] = gdf['geometry'].apply(lambda point: Point(*transformer.transform(point.x, point.y)))

# Initialize map centered at mean coordinate
map_center = [df.latitude.mean(), df.longitude.mean()]
m = folium.Map(location=map_center, zoom_start=10)

# Add markers
for idx, row in df.iterrows():
    folium.Marker(
        location=[row['latitude'], row['longitude']],
        popup=f"Location: {row['name']}"
    ).add_to(m)

# Add heatmap layer
heat_data = [[row['latitude'], row['longitude']] for idx, row in df.iterrows()]
HeatMap(heat_data).add_to(m)

# Save map as HTML
m.save('geospatial_map.html')
```
![Image](https://github.com/user-attachments/assets/65ffa566-be90-4a9d-a63a-bfd89a626ec8)
The coordinate data used in this project was sourced from publicly available datasets.

## Impact & Use Cases  
- **Urban Planning**: Helps visualize infrastructure layouts and plan new developments based on spatial clustering.  
- **Real-Time Tracking**: Forms the base for integrating live GPS feeds for delivery or transport tracking.  
- **Logistics Optimization**: Assists courier and fleet systems in finding the most efficient delivery paths by identifying high-density zones.  
- **Disaster Response**: Useful for identifying critical zones, shelters, and at-risk areas during natural calamities using geospatial overlays.


