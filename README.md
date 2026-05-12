# Project Overview

This project implements a multi-stage machine learning pipeline to analyze energy consumption patterns across 147 individual buildings and 4 aggregated communities. By clustering buildings based on their energy, I developed tailored Random Forest models to predict energy demand and identify mechanical anomalies for predictive maintenance.

**Datasource** - https://springernature.figshare.com/articles/dataset/HEEW_a_Hierarchical_Dataset_on_Multiple_Energy_Consumption_PV_Generation_Emissions_and_Weather_Information/28425647?file=57947407

# Data Cleaning & Preprocessing

To ensure high-quality model training, the raw data underwent a rigorous cleaning phase:

    1) Missing Value Management: Handled NaNs in sensor data using the dataset's built-in baseline cleaning scheme.

    2) Outlier Removal: Identified and removed non-physical negative energy values and extreme sensor spikes that would otherwise bias the predictive models.

    3) Feature Extraction: Transformed timestamps into cyclical temporal features (Hour, Day of Week, Month) to capture the rhythmic nature of building occupancy and climate-driven cycles.

# The Data Pipeline

    1) Data Sanitization: Removed non-physical values (negative consumption) and sensor spikes to ensure model integrity.

    2) Feature Engineering: Extracted temporal features (Hour, Day of Week, Month) to capture human-driven and seasonal cycles.

    3) Unsupervised Clustering: Grouped 148 sensors into 3 distinct behavioral profiles using K-Means clustering.

    4) Cluster-Level Modeling: Trained 3 independent Random Forest Regressors to establish a "Normal Operation" baseline for each group.

# Observation

- ## Cluster 0 (69 Meters):
  - **Performance:** 14.26% Error Rate.
  - **Observation:** This group is highly rhythmic, following standard 9-to-5 human occupancy patterns.
  - **Anomalies (897 detected):** These are largely "human" anomalies—instances where lighting or equipment was likely left on during weekends or late nights.
  - **Recommended action:** Operational waste reduction.

- ## Cluster 1 (12 Meters):
  - **Performance:** 65.38% Error Rate.
  - **Observation:** This is the most problematic group. The high error rate suggests these 12 meters could possibly be faulty and unreliable.
  - **Anomalies (587 detected):** This is a high density of errors for only 12 sensors. This indicates faulty sensors, broken timers, or manual overrides that are bypassing the building’s automated control systems.
  - **Recommended action:** Inspecting the sensors.

- ## Cluster 2 (66 Meters including CN03):
  - **Performance:** 18.20% Error Rate.
  - **Observation:** This cluster represents high-thermal-mass systems. They are more stable than Cluster 1 but less predictable than Cluster 0 because they are Weather Dependent.
  - **Anomalies (659 detected):** Since this includes the community-level CN03 meter, these anomalies are the highest priority. The "jittery" nature of the actual data compared to the smooth model prediction indicates mechanical hunting (equipment struggling to find a steady state).
  - **Recommended action:** Mechanical Audit for cycling issues.\*
