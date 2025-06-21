# Optimized Safe Route Planner

This project implements an optimized route planning system that combines Modified Dijkstra's and A* algorithms to find the safest and most efficient routes between locations. The system takes into account various safety factors and road conditions to provide optimal routing solutions.

## Features

- Modified Dijkstra's algorithm implementation for finding shortest paths
- A* algorithm implementation with safety-based heuristics
- Real-time safety data integration (crime, weather, lighting, accidents)
- Interactive route visualization with color-coded risk levels
- RESTful API for route planning and safety information
- Multiple routing modes (safest, fastest, shortest)
- Real-time safety alerts and incident reporting
- Performance optimization for large networks

## Requirements

- Python 3.8+
- NetworkX
- NumPy
- Matplotlib
- Folium (for map visualization)
- FastAPI (for API server)
- Geopy (for distance calculations)
- Pandas (for data handling)

## Installation

1. Clone this repository
2. Install the required dependencies:
```bash
pip install -r requirements.txt
```

3. Set up environment variables:
```bash
# Create a .env file with your API keys
CRIME_API_KEY=your_crime_api_key
WEATHER_API_KEY=your_weather_api_key
```

## Usage

### API Server

Start the API server:
```bash
python api.py
```

The server will be available at `http://localhost:8000`

### API Endpoints

- `POST /locations` - Add a new location
- `POST /connections` - Add a connection between locations
- `POST /routes` - Find a route between locations
- `GET /safety/{location_id}` - Get safety information for a location

### Example API Usage

```python
import requests

# Add a location
location_data = {
    "id": "Home",
    "coordinates": (40.7128, -74.0060),
    "safety_score": 0.9
}
response = requests.post("http://localhost:8000/locations", json=location_data)

# Find a route
route_data = {
    "start": "Home",
    "end": "Work",
    "use_astar": True,
    "mode": "safest"
}
response = requests.post("http://localhost:8000/routes", json=route_data)
```

### Direct Usage

```python
from route_planner import SafeRoutePlanner
from safety_data import SafetyDataManager
from route_visualizer import RouteVisualizer

# Initialize components
planner = SafeRoutePlanner()
safety_manager = SafetyDataManager()
visualizer = RouteVisualizer()

# Add locations and connections
planner.add_location("A", {"safety_score": 0.9, "coordinates": (40.7128, -74.0060)})
planner.add_location("B", {"safety_score": 0.8, "coordinates": (40.7589, -73.9851)})
planner.add_connection("A", "B", {
    "distance": 10,
    "safety_factor": 0.85,
    "traffic_factor": 1.2
})

# Find the safest route
route, cost = planner.find_safest_route("A", "B")

# Visualize the route
map_obj = visualizer.create_safety_map(route, planner.locations)
visualizer.save_map(map_obj, "route_map.html")
```

## Project Structure

- `route_planner.py`: Main implementation of the route planning algorithms
- `safety_data.py`: Safety data management and risk calculation
- `route_visualizer.py`: Route visualization with safety indicators
- `api.py`: FastAPI backend server
- `test_route_planner.py`: Unit tests
- `sample_data.py`: Sample data and usage examples
- `requirements.txt`: Project dependencies
- `README.md`: Project documentation

## Algorithm Details

### Modified Dijkstra's Algorithm
The modified version takes into account safety scores and road conditions when calculating the shortest path. It uses a composite cost function that combines:
- Distance
- Safety score
- Road conditions
- Traffic factors

### A* Algorithm
The A* implementation uses a heuristic function that estimates the remaining cost to the destination while considering safety factors. This makes it more efficient than Dijkstra's algorithm for large networks.

### Safety Scoring
The safety score is calculated using multiple factors:
- Crime data
- Street lighting
- Weather conditions
- Accident history
- Traffic patterns

## Real-time Features

- Live safety updates
- Incident reporting
- Dynamic route adjustments
- Safety alerts
- Risk level visualization

## License

MIT License 
