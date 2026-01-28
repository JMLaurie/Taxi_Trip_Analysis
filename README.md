# NYC Yellow Taxi Trip Data Analysis

## Overview
This project provides an end-to-end analysis of NYC Yellow Taxi trip data using the CRISP-DM (Cross-Industry Standard Process for Data Mining) methodology. The analysis focuses on uncovering patterns in taxi operations, identifying high-demand locations, and providing actionable business insights for taxi companies to optimize their services and maximize revenue.

## Author
Jake Laurie

## Project Objectives
1. **Data Cleaning and Preprocessing**: Prepare the taxi trip dataset for comprehensive analysis
2. **MapReduce Simulation**: Simulate distributed data aggregation tasks locally to analyze trip patterns
3. **Clustering Analysis using K-Means**: Identify patterns and segments in taxi trip behavior
4. **Visualization and Business Insights**: Present actionable recommendations for operational improvements

## Dataset
- **Source**: NYC Yellow Taxi trip data (taxi_tripdata.csv)
- **Size**: 83,691 trip records
- **Features**: 20 variables including:
  - Pickup/dropoff datetime and locations
  - Trip distance and duration
  - Fare amounts and payment details
  - Passenger count and trip type
  - Extra charges and surcharges

## Methodology (CRISP-DM Framework)

### 1. Data Understanding
Initial exploration revealed:
- 83,691 total trip records
- Mixed data types (float64, int64, object)
- Missing values in several columns (VendorID, passenger_count, payment_type)
- Empty column (ehail_fee) that was dropped
- Dataset covers trip attributes including distance, duration, fare, and location

### 2. Data Preparation & Cleaning
Data preprocessing steps:
- Dropped irrelevant/empty columns (ehail_fee)
- Converted datetime fields to proper datetime format
- Filtered out trips with non-positive distances or fares
- Removed rows with missing values in critical columns
- Created new feature: `trip_duration` (in minutes)
- Created `cost_per_mile` metric for efficiency analysis
- Final cleaned dataset: 48,170 valid records

### 3. MapReduce Simulation Analysis

#### Analysis 1: Trip Counts by Pickup Location
**Top Pickup Locations:**
- Location 74: 8,676 trips
- Location 75: 7,582 trips
- Location 41: 4,686 trips

**Business Insight**: Locations 74 and 75 are significantly busier than others, representing prime opportunities for driver deployment.

#### Analysis 2: Average Trip Duration by Location
**Longest Average Trip Durations:**
- Location 132: 105.5 minutes
- Location 6: 72.1 minutes
- Location 118: 70.9 minutes

**Shortest Average Trip Duration:**
- Location 156: 50.5 minutes

**Business Insight**: Understanding average trip durations helps drivers choose locations based on their preference for longer or shorter rides, enabling better revenue prediction.

#### Analysis 3: Payment Type Distribution
- **Credit Card**: 27,961 transactions (58%)
- **Cash**: 20,036 transactions (42%)
- **No Charge**: 152 transactions
- **Dispute**: 20 transactions
- **Unknown**: 1 transaction

**Business Insight**: Credit card is the dominant payment method. Ensuring card readers are functional is critical. Minimizing "no charge" and "dispute" transactions could increase revenue.

### 4. K-Means Clustering Analysis

#### Cluster 1: Trip Distance vs Total Amount
- **Number of Clusters**: 5
- **Silhouette Score**: 0.582 (indicating good cluster separation)
- **Key Findings**:
  - Majority of trips fall between 0-100,000 miles with $0-$100 fares
  - Average trips cluster around 50,000 miles and $50
  - Identified outliers:
    - Very short trip (~0 miles) costing $400+
    - Extremely long trip (250,000+ miles) costing less than $100
  - Lime green cluster shows short trips with unexpectedly high prices

**Business Insight**: Investigate pricing anomalies, particularly short trips with high fares and long trips with low fares.

#### Cluster 2: Trip Duration vs Cost Per Mile
- **Number of Clusters**: 4
- **Key Findings**:
  - Two clusters (blue/purple) maintain consistent cost per mile (~$0) across trip durations (0-1400 minutes)
  - Two clusters (orange/yellow) show increasing cost per mile ($1,000-$8,000) with short trip durations (0-100 minutes)
  - Unexpected pattern: High trip duration does NOT correlate with high cost per mile for most trips

**Business Insight**: Pricing inconsistencies suggest potential issues with fare calculation or data quality. Further investigation needed to understand why longer trips don't proportionally increase cost per mile.

## Key Findings & Recommendations

### Operational Recommendations
1. **Driver Deployment Strategy**:
   - Prioritize stationing drivers at high-demand locations (74 and 75)
   - Consider promotions at lower-demand location (41) to increase business
   - Use average trip duration data to help drivers choose locations based on their preferred trip length

2. **Revenue Optimization**:
   - Focus on maintaining credit card processing infrastructure (58% of transactions)
   - Investigate and minimize "no charge" (152 cases) and "dispute" (20 cases) transactions
   - Address pricing anomalies identified in clustering analysis

3. **Data Quality & Pricing**:
   - Investigate outlier trips with unusual distance-to-cost ratios
   - Review fare calculation algorithms for consistency
   - Ensure accurate recording of trip metrics

### Strategic Insights
- High-demand locations represent significant revenue opportunities
- Payment method preferences should guide infrastructure investment
- Clustering reveals distinct customer segments with different trip patterns
- Pricing inconsistencies suggest opportunities for optimization

## Technologies Used
- **Python**: Core programming language
- **pandas**: Data manipulation and analysis
- **numpy**: Numerical computing
- **matplotlib**: Data visualization
- **scikit-learn**: Machine learning (K-Means clustering, StandardScaler, silhouette_score)
- **datetime**: Time-based feature engineering

## Files
- `taxi_trip_analysis.ipynb`: Complete Jupyter notebook with all analysis and visualizations
- `taxi_tripdata.csv`: Source dataset (not included in repository)
- `README.md`: This file

## Project Structure
```
Taxi_Trip_Analysis/
│
├── taxi_trip_analysis.ipynb    # Main analysis notebook
├── taxi_tripdata.csv            # Data file (not tracked)
└── README.md                    # Project documentation
```

## Deployment Strategy

### Implementation Plan
1. **Platform Selection**: 
   - Leverage Amazon EMR for scalable processing of MapReduce and K-Means clustering
   - Use AWS scheduling tools to automate data processing and analysis

2. **Dashboard Development**:
   - Create interactive dashboard highlighting:
     - High-demand pickup locations
     - Average trip durations by location
     - Payment method distributions
     - Flagged anomalies for review

3. **Alert System**:
   - Implement automated alerts for unusual trip characteristics
   - Flag pricing anomalies for manual review
   - Monitor payment system health

4. **Continuous Improvement**:
   - Regular monitoring of key metrics
   - Iterative enhancements based on new data patterns
   - Feedback loop from drivers and operations team

### Expected Outcomes
- Improved driver placement efficiency
- Enhanced customer experiences through reduced wait times
- Increased revenue through optimized operations
- Data-driven decision making for strategic planning

## Visualizations
The analysis includes several key visualizations:
1. **Trip Distance vs Total Amount Clustering**: Shows 5 distinct customer segments
2. **Trip Duration vs Cost Per Mile Clustering**: Reveals 4 pricing pattern clusters
3. Distribution charts for pickup locations, trip durations, and payment types

## Future Enhancements
- Incorporate time-based analysis (peak hours, day of week patterns)
- Add weather data correlation with trip demand
- Implement predictive models for demand forecasting
- Analyze driver rating impacts on trip patterns
- Create route optimization recommendations
- Develop dynamic pricing suggestions based on demand

## Business Impact
This analysis enables taxi companies to:
- **Maximize Revenue**: Through optimal driver placement at high-demand locations
- **Improve Efficiency**: By understanding trip duration patterns and pricing optimization
- **Enhance Customer Service**: Through reduced wait times and better service availability
- **Data-Driven Operations**: Using continuous monitoring and iterative improvements

## Results Summary
The CRISP-DM methodology successfully identified critical operational insights including high-demand locations (74 & 75), payment preferences (58% credit card), and distinct customer segments through clustering analysis. The MapReduce simulation demonstrated scalability for larger datasets, while K-Means clustering revealed pricing anomalies requiring further investigation. These findings provide actionable recommendations for driver deployment, revenue optimization, and operational improvements.
