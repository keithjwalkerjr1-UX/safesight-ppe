# SafeSight PPE Dataset

## Primary Dataset
**Construction Site Safety**

## Source
Roboflow Universe

## Dataset Size
- Total images: 2,801
- Training images: 2,605
- Validation images: 114
- Test images: 82

## Project Labels
SafeSight PPE will focus on the following classes:

- Person
- Hardhat
- NO-Hardhat
- Safety Vest
- NO-Safety Vest

The source dataset contains additional construction-related classes, but the project will focus on these five because they directly support PPE compliance review.

## Dataset Purpose
The dataset will be used to fine-tune and evaluate the YOLO11 object detection model. SafeSight PPE will use the resulting detections to identify workers and flag possible missing hard hats or safety vests for human review.

## Data Format
The dataset contains labeled object-detection images with bounding-box annotations and is available in YOLO-compatible formats.

## Dataset Link
[Construction Site Safety Dataset](https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow)

## License
CC BY 4.0

## Repository Note
The full image dataset will not be stored directly in this GitHub repository. This folder documents the dataset source and how it will be used in the project.
