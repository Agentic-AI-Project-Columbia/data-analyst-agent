# Airbnb Data Schema - New York

## Dataset Overview
This schema describes the Airbnb datasets for New York, located in `Data/New York/`.

## Files

### 1. listings.csv
**Purpose**: Contains detailed information about each Airbnb listing.

**Key Columns**:
- `id`: Unique listing identifier
- `name`: Listing title
- `description`: Full property description
- `neighborhood_cleansed`: Neighborhood name
- `latitude`, `longitude`: Geographic coordinates
- `property_type`: Type of property (e.g., "Entire rental unit")
- `room_type`: Room category ("Entire home/apt", "Private room", etc.)
- `accommodates`: Maximum number of guests
- `bedrooms`: Number of bedrooms
- `beds`: Number of beds
- `price`: Nightly price (USD)
- `minimum_nights`, `maximum_nights`: Stay requirements
- `availability_30/60/90/365`: Available nights in next X days
- `number_of_reviews`: Total review count
- `review_scores_rating`: Overall rating (1-5)
- `host_id`: Host identifier
- `host_since`: When host started on Airbnb
- `instant_bookable`: Whether instant booking is available

**Analysis Notes**:
- Price needs cleaning (remove $, convert to numeric)
- Neighborhoods use ward names (e.g., "SIXTH WARD")
- Geographic coordinates available for spatial analysis

### 2. calendar.csv.gz
**Purpose**: Daily availability and pricing for each listing.

**Key Columns**:
- `listing_id`: References listings.id
- `date`: Calendar date
- `available`: "t" (available) or "f" (booked/blocked)
- `price`: Daily price (may vary from listing base price)

**Analysis Notes**:
- Compressed file (.gz format)
- Unavailability is a proxy for occupancy (not perfect)
- Price variations can indicate seasonal demand
- Requires decompression before analysis

### 3. reviews.csv
**Purpose**: Individual guest reviews for listings.

**Key Columns**:
- `listing_id`: References listings.id
- `id`: Unique review identifier
- `date`: Review date
- `reviewer_id`: Guest identifier
- `comments`: Review text (sentiment analysis)

**Analysis Notes**:
- Text data for sentiment analysis
- Time series of guest feedback
- Can track review frequency over time

### 4. neighbourhoods.csv
**Purpose**: Neighborhood reference data.

**Key Columns**:
- `neighbourhood_group`: Administrative area (often empty)
- `neighbourhood`: Neighborhood name (matches listings.neighborhood_cleansed)

**Analysis Notes**:
- Albany uses ward-based neighborhoods
- No hierarchical grouping in this dataset

### 5. neighbourhoods.geojson
**Purpose**: Geographic boundaries for neighborhoods.

**Format**: GeoJSON with polygon boundaries
**Use**: Spatial visualization and mapping

## Data Quality Considerations

### Missing Data
- Some listings may have missing review scores
- Host information can be incomplete
- Calendar data may have gaps

### Data Cleaning Needed
- Price fields: Remove "$" and convert to float
- Date fields: Convert to datetime objects
- Boolean fields: Convert "t"/"f" to boolean
- Geographic coordinates: Validate ranges

### Analysis Limitations
- Calendar unavailability ≠ actual occupancy
- Prices in listings.csv may not reflect dynamic pricing
- Neighborhood boundaries may not perfectly align with coordinates

## Recommended Analysis Approach

1. **Load and Clean**: Start with listings.csv for basic analysis
2. **Join Data**: Use listing_id to connect calendar and reviews
3. **Spatial Analysis**: Use coordinates with neighbourhoods.geojson
4. **Time Series**: Leverage calendar and reviews date fields
5. **Aggregation**: Group by neighborhood for market insights

## Sample Queries Supported

- Average price by neighborhood
- Occupancy rates by month (calendar proxy)
- Review sentiment trends
- Geographic distribution of listings
- Seasonal price variations
