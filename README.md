# Solving an SDG Problem with Data (Choose Your SDG)

## Overview
Select a Sustainable Development Goal (SDG) that resonates with you and develop a data-driven solution to address a specific problem within that SDG. Design a database, perform data analysis, and use Microsoft Excel as the user interface.

## Objectives
- Choose an SDG and identify a specific problem to address.
- Design and implement a relational database relevant to your chosen problem.
- Write SQL queries to retrieve and analyze data.
- Use Microsoft Excel for data visualization and analysis.

## Requirements

### Part 1: SDG Selection and Problem Definition
- **SDG Selection:** Choose an SDG (e.g., SDG 3: Good Health, SDG 7: Affordable and Clean Energy).
- **Problem Definition:** Define a specific problem within your chosen SDG that can be addressed using data.

  SDG Selection: SDG 11 - Sustainable Cities and Communities
Problem Definition:

Problem: Managing Urban Air Quality

Urban air quality is a critical issue in many cities worldwide. Poor air quality can lead to significant health problems, including respiratory and cardiovascular diseases, and negatively impacts the overall quality of life. Data can be used to monitor air pollution levels, identify sources of pollution, and develop strategies to improve air quality.

Specific Problem:
Problem: Tracking and Reducing Air Pollution Levels

Description:

Cities often face challenges with air pollution due to factors such as traffic congestion, industrial activities, and construction. To address this problem, data collection and analysis are essential to monitor pollution levels, identify pollution hotspots, and assess the effectiveness of air quality improvement measures.

Data Required:

Air Quality Data:

Pollutant Levels: Concentrations of pollutants like PM2.5, PM10, NO2, SO2, and CO.
Measurement Locations: Geographic locations of air quality monitoring stations.
Traffic Data:

Vehicle Counts: Number of vehicles in different city areas.
Traffic Patterns: Traffic flow data, congestion levels.
Industrial Data:

Emissions: Data on emissions from industrial facilities.
Facility Locations: Geographic locations of major industrial sources.
Weather Data:

Meteorological Conditions: Temperature, humidity, wind speed, and direction.
Potential Use of Data:
Monitoring Air Quality Trends: Analyzing historical data to understand trends and identify patterns in air pollution over time.

Identifying Pollution Hotspots: Using geographic data to pinpoint areas with consistently high pollution levels.

Evaluating Impact of Interventions: Assessing the effectiveness of policies or measures aimed at reducing pollution, such as traffic restrictions or industrial regulations.

Public Awareness and Health Advisories: Providing actionable information to the public and authorities about air quality and health risks.

### Part 2: Database Design
- **ERD:** Design an ERD for your project, including entities relevant to your SDG problem.
+-----------------------+        +------------------------+
| AirQualityMonitoring  |        | AirQualityMeasurement  |
| Station               |        |                        |
+-----------------------+        +------------------------+
| station_id (PK)       |<-------| measurement_id (PK)    |
| location_name         |        | station_id (FK)        |
| latitude              |        | measurement_date       |
| longitude             |        | pm2_5                  |
| installation_date     |        | pm10                   |
+-----------------------+        | no2                    |
                                 | so2                    |
                                 | co                     |
                                 | temperature            |
                                 | humidity               |
                                 | wind_speed             |
                                 | wind_direction         |
                                 +------------------------+

+------------------+         +--------------------+
| AirQualityAlert  |         | TrafficData        |
|                  |         |                    |
+------------------+         +--------------------+
| alert_id (PK)    |         | traffic_id (PK)    |
| station_id (FK)  |         | location_name      |
| alert_date       |         | date               |
| alert_type       |         | vehicle_count      |
| description      |         | congestion_level   |
+------------------+         +--------------------+

+-------------------+       +-----------------------+
| IndustrialFacility|       | CityArea              |
|                   |       |                       |
+-------------------+       +-----------------------+
| facility_id (PK)  |       | area_id (PK)          |
| facility_name     |       | area_name             |
| location_name     |       | latitude              |
| emission_level    |       | longitude             |
+-------------------+       +-----------------------+

  
- 
- **Schema:** Write SQL statements to create the database schema based on your ERD.

-- Create the AirQualityMonitoringStation table
CREATE TABLE AirQualityMonitoringStation (
    station_id INT PRIMARY KEY AUTO_INCREMENT,
    location_name VARCHAR(255) NOT NULL,
    latitude DECIMAL(9, 6) NOT NULL,
    longitude DECIMAL(9, 6) NOT NULL,
    installation_date DATE NOT NULL
);

-- Create the AirQualityMeasurement table
CREATE TABLE AirQualityMeasurement (
    measurement_id INT PRIMARY KEY AUTO_INCREMENT,
    station_id INT,
    measurement_date DATE NOT NULL,
    pm2_5 DECIMAL(5, 2),
    pm10 DECIMAL(5, 2),
    no2 DECIMAL(5, 2),
    so2 DECIMAL(5, 2),
    co DECIMAL(5, 2),
    temperature DECIMAL(5, 2),
    humidity DECIMAL(5, 2),
    wind_speed DECIMAL(5, 2),
    wind_direction DECIMAL(5, 2),
    FOREIGN KEY (station_id) REFERENCES AirQualityMonitoringStation(station_id)
);

-- Create the TrafficData table
CREATE TABLE TrafficData (
    traffic_id INT PRIMARY KEY AUTO_INCREMENT,
    location_name VARCHAR(255) NOT NULL,
    date DATE NOT NULL,
    vehicle_count INT,
    congestion_level VARCHAR(50)
);

-- Create the IndustrialFacility table
CREATE TABLE IndustrialFacility (
    facility_id INT PRIMARY KEY AUTO_INCREMENT,
    facility_name VARCHAR(255) NOT NULL,
    location_name VARCHAR(255) NOT NULL,
    emission_level DECIMAL(10, 2)
);

-- Create the CityArea table
CREATE TABLE CityArea (
    area_id INT PRIMARY KEY AUTO_INCREMENT,
    area_name VARCHAR(255) NOT NULL,
    latitude DECIMAL(9, 6),
    longitude DECIMAL(9, 6)
);

-- Create the AirQualityAlert table
CREATE TABLE AirQualityAlert (
    alert_id INT PRIMARY KEY AUTO_INCREMENT,
    station_id INT,
    alert_date DATE NOT NULL,
    alert_type VARCHAR(255),
    description TEXT,
    FOREIGN KEY (station_id) REFERENCES AirQualityMonitoringStation(station_id)
);

-- Add foreign key to link TrafficData to CityArea
ALTER TABLE TrafficData
ADD COLUMN area_id INT,
ADD FOREIGN KEY (area_id) REFERENCES CityArea(area_id);

-- Add foreign key to link IndustrialFacility to CityArea
ALTER TABLE IndustrialFacility
ADD COLUMN area_id INT,
ADD FOREIGN KEY (area_id) REFERENCES CityArea(area_id);

  
- **Sample Data:** Populate the database with relevant sample data.

  -- Insert sample data into AirQualityMonitoringStation
INSERT INTO AirQualityMonitoringStation (location_name, latitude, longitude, installation_date) VALUES
('Downtown Station', 40.712776, -74.005974, '2023-01-15'),
('Uptown Station', 40.776676, -73.971321, '2023-02-10'),
('Eastside Station', 40.730610, -73.935242, '2023-03-05');

-- Insert sample data into AirQualityMeasurement
INSERT INTO AirQualityMeasurement (station_id, measurement_date, pm2_5, pm10, no2, so2, co, temperature, humidity, wind_speed, wind_direction) VALUES
(1, '2024-08-01', 35.2, 45.0, 22.5, 8.0, 1.2, 27.5, 60.0, 3.5, 180),
(1, '2024-08-02', 32.0, 42.0, 25.0, 7.5, 1.5, 26.0, 55.0, 4.0, 190),
(2, '2024-08-01', 50.0, 60.0, 30.0, 10.0, 1.8, 29.0, 70.0, 2.5, 160),
(2, '2024-08-02', 48.0, 58.0, 32.0, 9.5, 1.9, 28.0, 68.0, 2.8, 170),
(3, '2024-08-01', 40.0, 50.0, 25.0, 9.0, 1.6, 26.5, 65.0, 3.0, 200);

-- Insert sample data into TrafficData
INSERT INTO TrafficData (location_name, date, vehicle_count, congestion_level, area_id) VALUES
('Downtown', '2024-08-01', 1200, 'High', 1),
('Uptown', '2024-08-01', 800, 'Medium', 2),
('Eastside', '2024-08-01', 600, 'Low', 3),
('Downtown', '2024-08-02', 1300, 'High', 1),
('Uptown', '2024-08-02', 850, 'Medium', 2);

-- Insert sample data into IndustrialFacility
INSERT INTO IndustrialFacility (facility_name, location_name, emission_level, area_id) VALUES
('Factory A', 'Downtown', 200.0, 1),
('Factory B', 'Uptown', 150.0, 2),
('Factory C', 'Eastside', 100.0, 3);

-- Insert sample data into CityArea
INSERT INTO CityArea (area_name, latitude, longitude) VALUES
('Downtown', 40.712776, -74.005974),
('Uptown', 40.776676, -73.971321),
('Eastside', 40.730610, -73.935242);

-- Insert sample data into AirQualityAlert
INSERT INTO AirQualityAlert (station_id, alert_date, alert_type, description) VALUES
(1, '2024-08-01', 'High PM2.5', 'PM2.5 levels exceed safety limits in Downtown.'),
(2, '2024-08-01', 'High NO2', 'NO2 levels are high in Uptown.'),
(3, '2024-08-01', 'Normal', 'Air quality is within safe limits in Eastside.');


### Part 3: SQL Programming
- **Data Retrieval:** Write SQL queries to retrieve relevant data based on your problem definition.
1. Retrieve Air Quality Measurements for a Specific Station
Query:
Get the air quality measurements from the "Downtown Station" for a specific date.
SELECT a.station_id, a.location_name, m.measurement_date, m.pm2_5, m.pm10, m.no2, m.so2, m.co, m.temperature, m.humidity, m.wind_speed, m.wind_direction
FROM AirQualityMonitoringStation a
JOIN AirQualityMeasurement m ON a.station_id = m.station_id
WHERE a.location_name = 'Downtown Station' AND m.measurement_date = '2024-08-01';

2. Get Air Quality Measurements for All Stations on a Specific Date
Query:
Retrieve air quality measurements for all stations on '2024-08-01'.
SELECT a.location_name, m.measurement_date, m.pm2_5, m.pm10, m.no2, m.so2, m.co, m.temperature, m.humidity, m.wind_speed, m.wind_direction
FROM AirQualityMeasurement m
JOIN AirQualityMonitoringStation a ON m.station_id = a.station_id
WHERE m.measurement_date = '2024-08-01';
3. Count of Patients Diagnosed with Each Chronic Disease
Query:
Count the number of patients diagnosed with each chronic disease.
SELECT ds.disease_name, COUNT(d.disease_id) AS num_patients
FROM Diagnoses d
JOIN Diseases ds ON d.disease_id = ds.disease_id
GROUP BY ds.disease_name;

4.Get Traffic Data for a Specific City Area
Query:
Retrieve traffic data for the "Downtown" area on '2024-08-01'.

SELECT t.date, t.vehicle_count, t.congestion_level
FROM TrafficData t
JOIN CityArea c ON t.area_id = c.area_id
WHERE c.area_name = 'Downtown' AND t.date = '2024-08-01';

5. List of Industrial Facilities and Their Emissions in a Specific City Area
Query:
Get the list of industrial facilities and their emission levels in the "Uptown" area.
SELECT i.facility_name, i.emission_level
FROM IndustrialFacility i
JOIN CityArea c ON i.area_id = c.area_id
WHERE c.area_name = 'Uptown';

6. Retrieve Air Quality Alerts for a Specific Station
Query:
Get the air quality alerts for the "Downtown Station".

SELECT a.alert_date, a.alert_type, a.description
FROM AirQualityAlert a
JOIN AirQualityMonitoringStation s ON a.station_id = s.station_id
WHERE s.location_name = 'Downtown Station';

7. Average Pollutant Levels for a Specific City Area Over a Period
Query:
Calculate the average levels of PM2.5 and PM10 in the "Eastside" area over August 2024.
SELECT AVG(m.pm2_5) AS avg_pm2_5, AVG(m.pm10) AS avg_pm10
FROM AirQualityMeasurement m
JOIN AirQualityMonitoringStation a ON m.station_id = a.station_id
JOIN CityArea c ON a.location_name = c.area_name
WHERE c.area_name = 'Eastside' AND m.measurement_date BETWEEN '2024-08-01' AND '2024-08-31';

 8. Identify High Congestion Areas
Query:
Find city areas with high congestion levels from traffic data.
 Identify High Congestion Areas
Query:
Find city areas with high congestion levels from traffic data.



9. Compare Emission Levels and Air Quality Measurements
Query:
Compare emission levels of industrial facilities with air quality measurements in the "Downtown" area.
SELECT i.facility_name, i.emission_level, m.pm2_5, m.pm10
FROM IndustrialFacility i
JOIN CityArea c ON i.area_id = c.area_id
JOIN AirQualityMonitoringStation a ON c.area_name = a.location_name
JOIN AirQualityMeasurement m ON a.station_id = m.station_id
WHERE c.area_name = 'Downtown' AND m.measurement_date = '2024-08-01';




- **Data Analysis:** Write SQL queries to analyze data and generate insights related to your SDG problem.
1. Average Pollutant Levels by City Area
Query:
Calculate the average levels of PM2.5 and PM10 for each city area.

sql
SELECT c.area_name,
       AVG(m.pm2_5) AS avg_pm2_5,
       AVG(m.pm10) AS avg_pm10
FROM AirQualityMeasurement m
JOIN AirQualityMonitoringStation a ON m.station_id = a.station_id
JOIN CityArea c ON a.location_name = c.area_name
GROUP BY c.area_name;

2. Pollution Trends Over Time
Query:
Analyze the trend of PM2.5 levels over the past month in the "Downtown" area.

sql

SELECT m.measurement_date,
       AVG(m.pm2_5) AS avg_pm2_5
FROM AirQualityMeasurement m
JOIN AirQualityMonitoringStation a ON m.station_id = a.station_id
JOIN CityArea c ON a.location_name = c.area_name
WHERE c.area_name = 'Downtown' AND m.measurement_date BETWEEN '2024-07-01' AND '2024-07-31'
GROUP BY m.measurement_date
ORDER BY m.measurement_date;

3. High Pollution Alerts by Station
Query:
Count the number of high pollution alerts (e.g., High PM2.5, High NO2) issued for each air quality monitoring station.

sql

SELECT a.location_name,
       COUNT(aa.alert_id) AS high_alerts_count
FROM AirQualityAlert aa
JOIN AirQualityMonitoringStation a ON aa.station_id = a.station_id
WHERE aa.alert_type LIKE '%High%'
GROUP BY a.location_name;


4. Correlation Between Traffic Congestion and Pollution Levels
Query:
Compare average PM2.5 levels with traffic congestion levels for the "Uptown" area.

sql

SELECT t.congestion_level,
       AVG(m.pm2_5) AS avg_pm2_5
FROM TrafficData t
JOIN CityArea c ON t.area_id = c.area_id
JOIN AirQualityMonitoringStation a ON c.area_name = a.location_name
JOIN AirQualityMeasurement m ON a.station_id = m.station_id
WHERE c.area_name = 'Uptown' AND m.measurement_date BETWEEN '2024-08-01' AND '2024-08-31'
GROUP BY t.congestion_level;

5. Emission Levels from Industrial Facilities and Corresponding Air Quality
Query:
Compare the average emission levels of industrial facilities with average PM2.5 levels in the "Eastside" area.


SELECT i.facility_name,
       i.emission_level,
       AVG(m.pm2_5) AS avg_pm2_5
FROM IndustrialFacility i
JOIN CityArea c ON i.area_id = c.area_id
JOIN AirQualityMonitoringStation a ON c.area_name = a.location_name
JOIN AirQualityMeasurement m ON a.station_id = m.station_id
WHERE c.area_name = 'Eastside' AND m.measurement_date BETWEEN '2024-08-01' AND '2024-08-31'
GROUP BY i.facility_name, i.emission_level;

6. Impact of Weather on Air Quality
Query:
Analyze how weather conditions (e.g., temperature, wind speed) affect PM2.5 levels.

sql

SELECT AVG(m.pm2_5) AS avg_pm2_5,
       AVG(m.temperature) AS avg_temperature,
       AVG(m.wind_speed) AS avg_wind_speed
FROM AirQualityMeasurement m
WHERE m.measurement_date BETWEEN '2024-08-01' AND '2024-08-31'
GROUP BY m.measurement_date
ORDER BY avg_pm2_5 DESC;

7. Monthly Average Emission Levels from Industrial Facilities
Query:
Calculate the monthly average emission levels from industrial facilities.

sql

SELECT MONTH(m.measurement_date) AS month,
       AVG(i.emission_level) AS avg_emission_level
FROM IndustrialFacility i
JOIN CityArea c ON i.area_id = c.area_id
JOIN AirQualityMonitoringStation a ON c.area_name = a.location_name
JOIN AirQualityMeasurement m ON a.station_id = m.station_id
GROUP BY MONTH(m.measurement_date);

### Part 4: Data Analysis Using Excel
- **Import Data:** Import data from your database into Excel.
- **Analysis:** Analyze the data using pivot tables, charts, and other Excel tools.
- **Dashboard:** Create an interactive Excel dashboard to visualize key insights.

### Part 5: Integration and Testing
- **Integration:** Document the process of importing data into Excel and ensuring consistency.
- **Testing:** Test the integration and functionality of your Excel dashboard.

### Part 6: Presentation
- **Pitch Deck:** Develop a 10-slide PowerPoint presentation as taught in the entrepreneurship module covering:
  - Project overview and SDG alignment.
  - Problem definition and significance.
  - Database design and schema.
  - Data analysis insights.
  - Excel dashboard demonstration.
- **Delivery:** Present your pitch deck, demonstrating how your project addresses the SDG problem.

## Deliverables (upload onto this repo)
- SDG problem definition document
- ERD
- SQL scripts
- Excel workbook with data analysis and dashboard
- Integration documentation
- Pitch deck presentation (Provide the link e.g Canva or Gamma in your documentation)

