# Thermal AI – Thermal Anomaly Detection

## Project Overview

This project implements an automated thermal anomaly detection pipeline designed to inspect industrial thermal imaging data. Operating on pixel-level temperature matrices, the system identifies high-temperature hotspots, segments distinct thermal regions using connected component labelling, and conducts comparative Delta-T analysis against corresponding reference regions to evaluate thermal variations and classify severity.

Key capabilities:
- **Matrix-Level Thermal Processing:** Analyzes full 2D temperature frames to assess overall and regional thermal characteristics.
- **Threshold-Based Hotspot Segmentation:** Identifies localized thermal anomalies that are masked in bulk average temperature measurements.
- **Delta-T Comparative Severity Classification:** Evaluates temperature differentials ($\Delta T$) against standard industrial condition monitoring benchmarks (Normal, Watch, Warning).
- **Automated Reporting & Visualization:** Generates visual hotspot maps and structured metric summaries.

---

## 1. Methodology

1. **Data Loading:** Loaded the 120 × 160 temperature matrix (19,200 pixels) from the supplied CSV using Pandas and NumPy.
2. **Asset Analysis:** Extracted the two asset regions and calculated their mean temperatures.
3. **Thresholding:** Applied a temperature threshold of 75°C to identify the two specified hotspots
4. **Hotspot Detection:** Used SciPy connected component labelling with 8-connectivity to group neighbouring hot pixels into distinct regions.
5. **Hotspot Characterisation:** Calculated peak temperature, mean temperature, pixel count, peak location and bounding box for each region.
6. **Delta-T Analysis:** Compared each hotspot peak with the corresponding pixel on the other asset using a 70-column offset and classified the temperature difference.

**Classification criteria:**

| Delta-T    | Classification |
| Below 10°C | Normal         |
| 10–25°C    | Watch          |
| Above 25°C | Warning        |

## 2. Threshold Sensitivity Analysis

Different thresholds were tested to evaluate their effect on hotspot detection.

| Threshold | Hot pixels | Regions | Region sizes |
| 73°C      | 178        | 2       | 120, 58      |
| 74°C      | 159        | 2       | 114, 45      |
| 75°C      | 146        | 2       | 103, 43      |
| 76°C      | 129        | 2       | 92, 37       |
| 80°C      | 97         | 2       | 76, 21       |

**Selected threshold: 75°C.** I tested several threshold to see which one best fitted the given dataset. Although both the hotspots remained identifiable with all the 5 threshold values I tested, I chose 75°C because it detected a sizeable amount of regions. 

## 3. Experimental Results

**Overall asset temperature comparison**

| Parameter        | Asset A | Asset B |
| Mean temperature | 69.36°C | 69.17°C |

Difference between asset mean temperatures: **0.19°C**.

**Hotspot detection and Delta-T results**

| Parameter | Hotspot 1 | Hotspot 2 |
| Asset     | A         | B         |
| Peak temperature | 117.49°C | 94.41°C |
| Mean temperature | 89.75°C | 81.96°C |
| Pixel count | 103 | 43 |
| Peak location (row, column) | (62, 45) | (95, 120) |
| Corresponding temperature | 68.18°C | 69.74°C |
| Delta-T | 49.31°C | 24.67°C |
| Classification | **Warning** | **Watch** |

**Key finding:** Despite nearly identical overall asset mean temperatures, the analysis identified two localised hotspots, demonstrating the importance of pixel-level thermal analysis.

The labelled hotspot visualisation is saved as `thermal_hotspots.png`.

## 4. AI Assistance Disclosure

AI assistance was used to understand the assessment requirements, explore the detection methodology, debug code and review result interpretation. The implementation was executed interactively, and the reported numerical results were verified against the supplied dataset.
