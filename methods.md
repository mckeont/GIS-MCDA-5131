# Methodology

The GIS Multi-Criteria Decision Analysis (MCDA) tool employs a weighted, threshold-based approach to evaluate and rank spatial features based on multiple criteria derived from various datasets. This methodology provides a structured yet flexible framework for spatial decision-making. The following sections detail each step of the scoring process.

## 1. Data Input and Selection

### Datasets and Fields

- **User-Provided Data**: Users upload multiple GeoJSON datasets, each representing a specific criterion relevant to the analysis (e.g., land slope, green space availability, water quality metrics).
- **Field Selection**: For each dataset, users select a specific field (attribute) that they wish to analyze (e.g., `slope_angle` in a land slope dataset).

### Weights

- **Importance Assignment**: Users assign a weight to each selected dataset to reflect its relative importance in the overall analysis. The weight is a numerical value, typically ranging from 0 to 100.

## 2. Threshold-Based Scoring

### Scoring Logic

The tool applies a threshold-based scoring system to evaluate the selected field within each dataset. Scores are assigned based on how each data value compares to calculated thresholds.

### Threshold Calculation

**Range-Based Thresholds**: Thresholds are calculated using the minimum and maximum values of the selected field in each dataset, effectively dividing the data range into four equal intervals. Specifically:

- **Lower Threshold**:

  $$
  \text{Lower Threshold} = \text{Min} + 0.25 \times (\text{Max} - \text{Min})
  $$

- **Middle Threshold**:

  $$
  \text{Middle Threshold} = \text{Min} + 0.5 \times (\text{Max} - \text{Min})
  $$

- **Upper Threshold**:

  $$
  \text{Upper Threshold} = \text{Min} + 0.75 \times (\text{Max} - \text{Min})
  $$

**Note**: These thresholds divide the data into quartiles based on the range, not on actual percentiles of the data distribution. This approach assumes a uniform distribution of data values.

### Scoring Assignments

**Score Definitions**:

- **++ (2 points)**: If the field value is greater than or equal to the upper threshold.
- **+ (1 point)**: If the field value is between the middle and upper thresholds.
- **- (-1 point)**: If the field value is between the lower and middle thresholds.
- **-- (-2 points)**: If the field value is less than or equal to the lower threshold.

**Application**: Each feature within the dataset is evaluated against these thresholds, and a score is assigned accordingly.

## 3. Weighted Score Calculation

### Intersection and Proportion

- **Spatial Intersection**: For each boundary feature (e.g., a watershed or administrative area), the tool calculates the spatial intersection with features from each dataset.
- **Proportion of Overlap**: The area of overlap between the boundary feature and each dataset feature is computed to determine the proportion of influence:

  $$
  \text{Proportion} = \frac{\text{Intersection Area}}{\text{Feature Area}}
  $$

### Weighted Score Computation

- **Feature Score Calculation**: The score assigned to each intersecting feature is multiplied by its user-assigned weight and the proportion of overlap:

  $$
  \text{Weighted Score} = \text{Score} \times \text{Weight} \times \text{Proportion}
  $$

- **Dataset Score Summation**: The weighted scores for all features within a dataset are summed to obtain a total score for that dataset within the boundary feature.

- **Normalization**: If multiple features from the same dataset intersect with the boundary feature, the total score is normalized by the total weight assigned to that dataset.

## 4. Cumulative Score Calculation

### Aggregation Across Datasets

- **Total Score and Weight**: For each boundary feature, the total scores from all datasets are aggregated, and the total weights are summed.

- **Raw Cumulative Score**: Calculated by normalizing the aggregated score:

  $$
  \text{Raw Cumulative Score} = \frac{\text{Total Score}}{\text{Total Weight}}
  $$

### Normalization to Scale

- **Scaling to 1-100**: The raw cumulative score is adjusted to fit within a 1-100 scale for consistency and ease of interpretation:

  $$
  \text{Cumulative Score} = \min\left(100,\, \max\left(1,\, (\text{Raw Cumulative Score} + 2) \times 25\right)\right)
  $$

- **Explanation**: The raw cumulative score, which ranges from -2 to 2 due to the scoring system, is shifted and scaled to fit within the desired range.

## 5. Infrastructure Type Suggestion

Based on the cumulative score, the tool suggests the most suitable type of infrastructure or strategy for each boundary feature:

- **1-15**: Decentralized Surface Green Infrastructure
- **16-30**: Decentralized Rooftop Green Infrastructure
- **31-45**: Decentralized Surface Gray Infrastructure
- **46-60**: Decentralized Subsurface Gray Infrastructure
- **61-75**: Centralized Surface Green Infrastructure
- **76-100**: Centralized Subsurface Gray Infrastructure

## 6. Map Visualization

### Color-Coded Representation

- **Color Scale**: The cumulative scores are visualized on an interactive map using a gradient color scale, typically ranging from green (low scores) to red (high scores).
- **Boundary Feature Styling**: Each boundary feature is color-coded based on its cumulative score, providing a quick visual representation of areas that meet certain criteria.

### Interactive Elements

- **Pop-ups and Legends**: Users can interact with the map to view detailed information about each boundary feature, including its cumulative score and suggested infrastructure type.
- **Dynamic Updates**: The map and associated data tables update dynamically as users adjust weights, thresholds, or datasets.

## 7. Flexibility and Customization

### User Input and Control

- **Dataset Selection**: Users have the flexibility to select which datasets to include in the analysis, allowing for tailored evaluations based on specific interests or data availability.
- **Weight Adjustment**: Adjusting the weights assigned to each dataset enables users to emphasize or de-emphasize certain criteria in the final scoring.
- **Field Selection**: Choosing different fields within datasets can significantly impact the analysis, providing a means to explore various aspects of the data.

### Dynamic Analysis

- **Adaptability**: The threshold-based scoring system adapts to different data ranges and distributions, maintaining relevance across various scenarios.
- **Real-Time Feedback**: Immediate visual and numerical feedback allows users to iteratively refine their analysis parameters for optimal results.
