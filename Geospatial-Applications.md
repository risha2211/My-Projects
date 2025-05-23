# Geospatial Applications (Maps)

## Overview  
Developed a robust Python-based geospatial application capable of visualizing and tracking any location worldwide using precise latitude and longitude coordinates. The project integrates multiple Python libraries to create an interactive, layered map with spatial analysis capabilities — all running locally or inside Jupyter notebooks.

## Technologies & Tools Used  
- **Python 3.5 / 3.6**: Core programming language during initial development.  
- **Folium**: For creating interactive Leaflet.js maps with Python; used to render map tiles, markers, and popups.  
- **Geopandas**: Extended Pandas for geospatial data handling; used for reading, manipulating, and visualizing shapefiles and GeoJSON data.  
- **Shapely**: Provides geometric operations like buffering, intersection, and distance calculation on map features.  
- **Pandas**: For preprocessing coordinate datasets, handling CSV inputs, and data cleaning.  
- **Requests**: To pull external map data and real-time geospatial data from APIs (e.g., OpenStreetMap).  
- **Pyproj**: To handle coordinate reference system transformations, ensuring all coordinates align properly on the map.  
- **Jupyter Notebooks**: For interactive development, step-by-step coding, and visualization during initial project creation.  
- **Google Colab**: Used in later iterations for cloud-based demos and easy sharing without local setup.  

## Architecture & Implementation Details  
- **Data Input**: Coordinates and datasets were input via CSV files or user input in notebooks, validated using Pandas for data integrity.  
- **Coordinate Handling**: Coordinates were converted between geographic (lat/lon) and projected coordinate systems using Pyproj to enable accurate spatial calculations.  
- **Map Rendering**: Folium created base maps with OpenStreetMap or Mapbox tiles; custom markers and popups displayed metadata linked to each coordinate.  
- **Layer Management**: Multiple feature layers (points, polygons, heatmaps) managed through Folium’s `FeatureGroup` and `LayerControl` classes, allowing toggling.  
- **Spatial Operations**: Using Shapely and Geopandas, implemented buffering zones around points (e.g., 5 km radius), spatial joins to identify nearby features, and distance calculations between coordinates.  
- **Dynamic Visualization**: Incorporated heatmaps (via `folium.plugins.HeatMap`) to visualize cluster density dynamically based on input data.  
- **Export & Sharing**: Generated standalone HTML files from Folium maps for sharing or embedding. The notebook workflow allowed iterative development and visualization refinement.  

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
