# Air Quality Armenia Data Preparation

This repository contains scripts and notebooks for processing and analyzing air quality data for Armenia. The data is sourced from various sensors and stations across the country, and the project focuses on preparing the data for analysis and visualization.

## Project Overview

The goal of this project is to:
- Load air quality data from CSV files into a SQLite database.
- Perform data processing and aggregation to analyze trends in PM2.5, PM10, temperature, pressure, and humidity.
- Generate daily and hourly aggregated datasets for further analysis.

## Data Description

The data is organized into the following files and directories:

### Main CSV Files
- **`sensors.csv`**: Contains the list of sensors and their stations.
  - Columns: `id`, `station_id`, `city_slug`, `district_slug`, `address_en`, `title`, `provider`, `external_id`, `sensor_type`, `latitude`, `longitude`, `altitude`, `first_measurement_time`, `last_measurement_time`, `is_suspicious`
- **`sensor_avg_daily.csv`**: Contains daily average values for each sensor.
  - Columns: `sensor_id`, `timestamp`, `avg_pm2.5`, `avg_pm10`, `avg_temperature`, `avg_pressure`, `avg_humidity`
- **`station_avg_daily.csv`**: Contains daily average values for each station.
  - Columns: `station_id`, `timestamp`, `avg_pm2.5`, `avg_pm10`, `avg_temperature`, `avg_pressure`, `avg_humidity`

### Hourly Data
- **`sensor_avg_hourly/sensor_avg_hourly_{year}.csv`**: Hourly average values for sensors, organized by year.
- **`station_avg_hourly/station_avg_hourly_{year}.csv`**: Hourly average values for stations, organized by year.

### Raw Measurements
- **`measurements/measurements_{year}_{month}.csv`**: Raw sensor measurements, organized by year and month.
  - Columns: `sensor_id`, `timestamp`, `time_start`, `time_end`, `pm2.5`, `pm2.5_corrected`, `pm10`, `temperature`, `pressure`, `humidity`

## Setup Instructions

### Prerequisites
- Python 3.x
- Required Python packages: `pandas`, `sqlite3`

### Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/air-quality-armenia.git
   cd air-quality-armenia
   ```
2. Install the required Python packages:
   ```bash
   pip install pandas
   ```

### Database Setup
1. Run the Jupyter notebook `air_quality_data_preparation.ipynb` to load the CSV files into a SQLite database:
   ```bash
   jupyter notebook air_quality_data_preparation.ipynb
   ```
2. The database file `air_quality_armenia.db` will be created in the root directory.

## Usage

### Example Queries
1. **Daily PM2.5 Averages by City (2024)**:
   ```sql
   SELECT 
       strftime('%Y-%m-%d', timestamp) AS day,
       city_slug AS city,
       AVG("pm2.5") AS avg_pm2_5
   FROM sensor_avg_hourly_2024
   JOIN sensors ON sensor_avg_hourly_2024.sensor_id = sensors.id
   WHERE is_suspicious = 0
   GROUP BY day, city
   ORDER BY day, city;
   ```

2. **Daily PM2.5 Averages for Armenia (2024)**:
   ```sql
   SELECT 
       strftime('%Y-%m-%d', timestamp) AS day,
       AVG("pm2.5") AS avg_pm2_5
   FROM sensor_avg_hourly_2024
   JOIN sensors ON sensor_avg_hourly_2024.sensor_id = sensors.id
   WHERE is_suspicious = 0
   GROUP BY day
   ORDER BY day;
   ```

### Saving Results
Aggregated results are saved in the `data_agg` directory. For example:
