# GIS-MCDA-5131
MCDA tool for wet weather management
https://mckeont.github.io/GIS-MCDA-5131/  <- Simple Version

https://mckeont.github.io/mcda_v1/ <- original Version



# Methodology

The GIS Multi-Criteria Decision Analysis (MCDA) tool employs a weighted, threshold-based approach to evaluate and rank spatial features based on multiple criteria derived from various datasets. This methodology provides a structured yet flexible framework for multi-criteria decision-making in spatial analysis. Below is a detailed description of the steps involved in the scoring process.

## 1. Data Input and Selection

### Datasets and Fields:

- **User-Provided Data:** Users upload multiple GeoJSON datasets. *(Next version to upload shapefiles and other data formats)*, each representing a specific criterion relevant to the analysis (e.g., land slope, green space availability, water quality metrics).
- **Field Selection:** For each dataset, users select a particular field (attribute) that they wish to summarize and analyze (e.g., `"slope_angle"` in a land slope dataset).

### Weights:

- **Importance Assignment:** Users assign a weight to each selected dataset to reflect its relative importance in the overall analysis. The weight is a numerical value, typically ranging from 0 to 100.

## 2. Threshold-Based Scoring

### Scoring Logic:

- The tool applies a threshold-based scoring system to evaluate the selected field within each dataset. The scoring assigns numerical values based on how each data value compares to calculated thresholds.

### Threshold Calculation:

- **Range-Based Thresholds:** Thresholds are calculated using the minimum and maximum values of the selected field in each dataset, effectively dividing the data range into four equal intervals. Specifically:
  - **Lower Threshold:** 

    ```markdown
    Lower Threshold = Min + 0.25 × (Max − Min)
    ```

  - **Middle Threshold:** 

    ```markdown
    Middle Threshold = Min + 0.5 × (Max − Min)
    ```

  - **Upper Threshold:** 

    ```markdown
    Upper Threshold = Min + 0.75 × (Max − Min)
    ```

- **Note on Thresholds:** These thresholds divide the data into quartiles based on the range, not on actual percentiles of the data distribution. This approach assumes a uniform distribution of data values.

### Scoring Assignments:

- **Score Definitions:**
  - `++` (2 points): If the field value is greater than or equal to the upper threshold.
  - `+` (1 point): If the field value is between the middle and upper thresholds.
  - `-` (-1 point): If the field value is between the lower and middle thresholds.
  - `--` (-2 points): If the field value is less than or equal to the lower threshold.
  
- **Application:** Each feature within the dataset is evaluated against these thresholds, and a score is assigned accordingly.

## 3. Weighted Score Calculation

### Intersection and Proportion:

- **Spatial Intersection:** For each boundary feature (e.g., a watershed or administrative area), the tool calculates the spatial intersection with features from each dataset.
- **Proportion of Overlap:** The area of overlap between the boundary feature and each dataset feature is computed to determine the proportion of influence `(proportion = intersection area / feature area)`.

### Weighted Score Computation:

- **Feature Score Calculation:** The score assigned to each intersecting feature is multiplied by its user-assigned weight and the proportion of overlap.

    ```markdown
    Weighted Score = Score × Weight × Proportion
    ```

- **Dataset Score Summation:** The weighted scores for all features within a dataset are summed to obtain a total score for that dataset within the boundary feature.
- **Normalization:** If multiple features from the same dataset intersect with the boundary feature, the total score is normalized by the total weight assigned to that dataset.

## 4. Cumulative Score Calculation

### Aggregation Across Datasets:

- **Total Score and Weight:** For each boundary feature, the total scores from all datasets are aggregated, and the total weights are summed.
- **Cumulative Score Computation:** The cumulative score is calculated by normalizing the aggregated score using the following formula:

    ```markdown
    Raw Cumulative Score = Total Score / Total Weight
    ```

### Normalization to Scale:

- **Scaling to 1-100:** The raw cumulative score is adjusted to fit within a 1-100 scale for consistency and ease of interpretation.

    ```markdown
    Cumulative Score = min(100, max(1, (Raw Cumulative Score + 2) × 25))
    ```

  - **Explanation:** The raw cumulative score, which can range from -2 to 2 due to the scoring system, is shifted and scaled to fit within the desired range.

## 5. Infrastructure Type Suggestion

Based on the cumulative score, the tool suggests the most suitable type of infrastructure or strategy for each boundary feature:

- **1-15:** Decentralized Surface Green Infrastructure
- **16-30:** Decentralized Rooftop Green Infrastructure
- **31-45:** Decentralized Surface Gray Infrastructure
- **46-60:** Decentralized Subsurface Gray Infrastructure
- **61-75:** Centralized Surface Green Infrastructure
- **76-100:** Centralized Subsurface Gray Infrastructure

## 6. Map Visualization

### Color-Coded Representation:

- **Color Scale:** The cumulative scores are visualized on an interactive map using a gradient color scale, typically ranging from green (low scores) to red (high scores).
- **Boundary Feature Styling:** Each boundary feature is color-coded based on its cumulative score, providing a quick visual representation of areas that meet certain criteria.

### Interactive Elements:

- **Pop-ups and Legends:** Users can interact with the map to view detailed information about each boundary feature, including its cumulative score and suggested infrastructure type.
- **Dynamic Updates:** The map and associated data tables update dynamically as users adjust weights, thresholds, or datasets.

## 7. Flexibility and Customization

### User Input and Control:

- **Dataset Selection:** Users have the flexibility to select which datasets to include in the analysis, allowing for tailored evaluations based on specific interests or data availability.
- **Weight Adjustment:** Adjusting the weights assigned to each dataset enables users to emphasize or de-emphasize certain criteria in the final scoring.
- **Field Selection:** Choosing different fields within datasets can significantly impact the analysis, providing a means to explore various aspects of the data.

### Dynamic Analysis:

- **Adaptability:** The threshold-based scoring system adapts to different data ranges and distributions, maintaining relevance across various scenarios.
- **Real-Time Feedback:** Immediate visual and numerical feedback allows users to iteratively refine their analysis parameters for optimal results.

- **Range-Based vs. Percentile-Based Thresholds:**
  - The current method uses range-based thresholds, dividing the data range into quartiles without accounting for the actual data distribution.
  - This approach assumes a uniform distribution of data values, which may not hold true for skewed datasets or those with outliers.



## Conclusion

The GIS MCDA tool's methodology integrates multiple spatial datasets to evaluate and rank boundary features based on user-defined criteria and weights. By employing a weighted, threshold-based scoring system, the tool provides a comprehensive analysis that is both structured and adaptable to various scenarios. The methodology emphasizes user control and customization, allowing for tailored analyses that meet specific needs.

This methodology facilitates informed decision-making in spatial planning, infrastructure development, and environmental management by providing clear, quantifiable insights into the suitability of different areas based on multiple criteria.
