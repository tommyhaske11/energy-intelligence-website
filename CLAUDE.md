# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Energy Intelligence Website - a real-time energy market analysis dashboard with live oil/gas prices, historical data visualization, and energy news aggregation focused on AI and data center energy consumption.

## Development Commands

```bash
# Install dependencies
pip install -r requirements.txt

# Run application locally
python energy_website_complete.py

# Application will be available at http://localhost:8000
```

## Architecture

### Single-File Application Structure
The entire application is contained in `energy_website_complete.py` (1920 lines) with:
- Flask backend with CORS enabled
- Embedded HTML/CSS/JavaScript frontend served as Python strings
- Background thread for periodic data updates (60-second intervals)
- RESTful API endpoints for data access

### Key API Endpoints
- `/` - Main dashboard page with tabbed interface
- `/api/live-data` - Returns current market prices (oil, gas)
- `/api/trending-articles` - Returns energy-related news articles
- `/api/oil-history/<oil_type>` - Returns historical data for Brent or WTI crude
- `/api/sector-comparison` - Returns energy sector comparison data (fossil vs renewables)
- `/api/renewable-trends` - Returns renewable energy trends over time
- `/api/regional-prices` - Returns regional energy price variations
- `/api/carbon-pricing` - Returns carbon pricing and emissions data

### External API Dependencies
1. **EIA API** (U.S. Energy Information Administration)
   - Provides historical oil price data
   - API Key: `2k4MqSPa5XgbneHOffnGIfgd17eThUsOqKIH1XIS`

2. **NewsAPI** 
   - Provides energy and financial news
   - API Key: `5354c0e4c78b4075a88c1fa87ba33b2e`

### Data Update Mechanism
- `update_data()` function runs in a background thread
- Updates occur every 60 seconds
- Includes fallback data generation when APIs are unavailable

## Important Implementation Details

### Frontend Architecture
- Chart.js for data visualization (loaded from CDN)
- SpaceX-inspired dark theme design  
- **Tabbed Interface**: Two main tabs - "Market Overview" and "Sector Comparison"
- Responsive grid layout for market data display
- Real-time updates via periodic API polling

### Tab System
- **Market Overview Tab**: Original dashboard with live oil/gas prices, news, and insights
  - Shows Strategic Intelligence and Financial Intelligence sections
- **Sector Comparison Tab**: Clean, focused comparison dashboard featuring:
  - Fossil fuels vs renewables price comparison (bar chart)
  - Energy mix trends over time (line chart)
  - Regional price variations (radar chart)
  - Carbon pricing and emissions data (dual-axis line chart)
  - Renewable energy metrics (solar/wind LCOE, carbon prices, electricity rates)
  - **Strategic Intelligence and Financial Intelligence sections are hidden** for focused analysis

### Deployment Configuration
- Railway-ready with `Procfile`
- Dynamic port configuration via `PORT` environment variable
- Default port: 8000
- Host: 0.0.0.0 (allows external connections)

### Error Handling
- Comprehensive try-catch blocks for API calls
- Fallback to realistic sample data when APIs fail
- Error logging to console

## Common Development Tasks

### Modifying API Data Sources
- Oil price data logic: Look for `fetch_oil_prices()` function
- News aggregation: Search for `fetch_trending_articles()` function
- Live market data: Check `generate_sample_data()` for structure

### Updating Frontend
- HTML/CSS/JavaScript are embedded as strings in the Python file
- Search for `html_content = """` to find the main template
- CSS styling starts after `<style>` tag
- JavaScript logic starts after `<script>` tag
- **Tab functionality**: `switchTab()` function handles tab switching and data loading
- **Comparison charts**: Individual chart creation functions for each visualization type

### Adding New API Endpoints
- Add route decorators after existing routes (around line 400+)
- Follow the pattern of existing endpoints
- Ensure CORS is handled (already configured globally)

## Testing Approach
No formal test suite exists. Manual testing recommended:
1. Start the application locally
2. Verify all API endpoints return expected data
3. Check frontend displays and updates correctly
4. Test with API failures to ensure fallback data works