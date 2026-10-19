# Vendora — Dwell-Time & Bottleneck Mapper

**Smarter Retail, Stronger Business**

A privacy-conscious retail video analytics prototype that transforms CCTV person-tracking data into zone-level dwell-time estimates, foot-traffic heatmaps, occupancy statistics, and preliminary bottleneck indicators.

## Overview

Retailers need to understand how customers move through stores, which areas attract attention, and where congestion or low activity may occur. Vendora explores how existing CCTV tracking data and manually annotated zones can help generate these insights.

This prototype processes person-tracking CSV data and polygon-zone JSON annotations to calculate observed dwell time, visualize movement density, and identify areas that may require further review.

## Features

- **Zone-based dwell-time analysis:** Estimates observed time spent inside annotated retail zones.
- **Visit segmentation:** Groups tracked observations into visit segments.
- **Foot-traffic heatmaps:** Visualizes the spatial distribution of tracked foot positions.
- **Occupancy analytics:** Calculates average and peak observed occupancy by camera view and zone.
- **Movement indicators:** Estimates movement in pixels per second from consecutive tracking observations.
- **Preliminary bottleneck flags:** Identifies high-occupancy zones for manual review.
- **Low-activity flags:** Identifies zones that may warrant investigation.
- **Privacy-conscious analytics:** Focuses on temporary tracking IDs, spatial positions, and aggregated statistics rather than real-world identities.

## System Workflow

1. Load existing person-tracking results.
2. Load manually annotated polygon zones from a JSON file.
3. Determine whether each tracked foot position lies inside an annotated zone.
4. Segment observed zone visits and estimate dwell time.
5. Generate per-view foot-traffic heatmaps.
6. Calculate occupancy and movement indicators.
7. Produce preliminary zone flags and export analytics to CSV.

**Note:** This notebook consumes existing tracking results. It does not currently run the person detector and tracker end to end.

## Prototype Dataset

The demonstration uses a 60-second video composed of six consecutive camera views.

| Parameter | Value |
|---|---|
| Video resolution | 1280 × 720 |
| Frame rate | 24 FPS |
| Total duration | 60 seconds |
| Camera views | 6 |
| Duration per view | 10 seconds |
| Annotated zones | 10 |
| Tracking detections loaded | 11,141 |
| Zone-membership detections | 9,245 |
| Recorded visit segments | 247 |

Track identities are handled separately by view. Cross-camera identity matching is not claimed.

## Sample Findings

The short demonstration produced the following preliminary observations:

- `zone_F` had the highest mean observed occupancy in the annotated zones.
- `zone_E` and `zone_G` were also flagged as high-occupancy candidates.
- `zone_D` and `zone_H` were flagged as low-activity candidates.

These are exploratory results from a short sample. They are not proof of persistent congestion or permanently unused retail areas and require video review.

## Generated Outputs

| File | Description |
|---|---|
| `zone_dwell_summary.csv` | Per-zone visit counts and observed dwell-time statistics |
| `zone_visits.csv` | Individual visit segments |
| `zone_membership.csv` | Tracked observations inside annotated polygons |
| `six_view_foot_traffic_heatmaps.png` | Foot-position density heatmaps for all six views |
| `heatmap_summary.csv` | Heatmap statistics by view |
| `view_occupancy_movement_summary.csv` | View-level occupancy and movement indicators |
| `zone_occupancy_summary.csv` | Zone-level presence and occupancy statistics |
| `preliminary_zone_flags.csv` | Preliminary bottleneck and low-activity flags |

## Technology Stack

- Python
- Pandas and NumPy
- OpenCV
- Matplotlib
- Google Colab
- CSV tracking data and JSON polygon annotations

The existing tracking data was generated using a person-detection and tracking pipeline based on YOLO and BoT-SORT, with manual track corrections. Tracking is not perfect, particularly around occlusions and identity switches.

## Running the Notebook

1. Open the `.ipynb` notebook in Google Colab.
2. Upload or mount the required tracking CSV, polygon-zone JSON, and other input files.
3. Update file paths if your files are stored in different locations.
4. Run the notebook cells in sequence.
5. Review the generated CSV files and heatmap.

Install the required Python packages if they are not already available:

```bash
pip install -r requirements.txt
```

## Limitations

- Dwell time is based on observed tracking detections and can be affected by missed detections, identity switches, and track fragmentation.
- Visit segmentation is an estimate and may not correspond exactly to a continuous real-world visit.
- Heatmaps represent detection frequency at foot positions, not unique customers or calibrated physical density.
- Movement speed is measured in pixels per second, not metres per second.
- Bottleneck and low-activity classifications use preliminary rules and have not been validated against manually labelled ground truth.
- Ten seconds per view is insufficient to establish persistent traffic patterns.
- A complete real-time CCTV pipeline, validated cross-camera journeys, calibrated speed, and production-ready congestion detection remain future work.

## Privacy Considerations

The prototype emphasizes temporary track IDs, spatial information, and aggregated zone-level metrics. Real-world identity recognition is not part of the demonstrated analytics.

Only upload CCTV footage that you are authorized to share. Do not publish identifiable or sensitive surveillance footage in a public repository.

## Future Improvements

- Validate dwell-time estimates against manually reviewed video.
- Improve track continuity and visit segmentation during occlusion.
- Evaluate bottleneck rules using longer recordings and labelled ground truth.
- Add a store-local dashboard and real-time inference pipeline.
- Incorporate camera calibration and normalized occupancy metrics.
- Evaluate performance on longer videos and additional retail layouts.

## Project Status

**Research and prototype stage.** Zone-level analytics, heatmaps, occupancy summaries, and preliminary rule-based flags have been generated from existing tracking data. Further validation is required before deployment in a real retail environment.
