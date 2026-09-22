# SafeSight PPE

## Team Members
- Keith Walker - W210940946@student.hccs.edu

## Project Tier
**Tier 2: Advanced**

SafeSight PPE fits Tier 2 because it combines YOLO-based PPE detection with application logic that converts model detections into a compliance review and possible violation report.

## Problem Statement
Construction safety teams need a consistent way to review whether workers appear to be wearing required personal protective equipment such as hard hats and safety vests. Manually reviewing worksite images can become time-consuming and inconsistent, especially when multiple workers are present in the same scene.

## Solution Overview
SafeSight PPE is a computer vision application that analyzes construction-site images and detects workers along with key PPE conditions. The system will use those detections to produce an annotated image and a short review summary that flags possible missing hard hats or safety vests for human review.

**Input → Model → Output**

Worksite Image → YOLO PPE Detection → Compliance Logic → Annotated Image + Review Summary

## Technical Approach
- **CV Technique:** Object Detection + Compliance Reporting
- **Model Architecture:** CNN-based object detector
- **Model:** YOLO11
- **How I will use it:** Start with pretrained weights and fine-tune the model using transfer learning on the Construction Site Safety dataset
- **Framework:** Ultralytics / PyTorch
- **Why this approach:** YOLO can detect multiple workers and PPE items in a single image while providing bounding boxes and confidence scores. The compliance logic will convert those raw detections into a result that is easier for a safety manager to review.

## Dataset
- **Dataset:** Construction Site Safety
- **Source:** Roboflow Universe
- **Size:** 2,801 images
- **Training:** 2,605 images
- **Validation:** 114 images
- **Test:** 82 images
- **Project Labels:** Person, Hardhat, NO-Hardhat, Safety Vest, NO-Safety Vest
- **Task:** Object Detection
- **License:** CC BY 4.0
- **Link:** (https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow)

## Success Metrics
- **Primary:** Measure mAP50 with a target of at least **0.70** on unseen test images.
- **Secondary:** Measure inference speed with a target of **under 1 second per image**.

## Milestone Plan

| Phase | Goal | Milestone | Schedule |
|---|---|---|---|
| Blueprint | Finalize the project scope, dataset, metrics, and technical plan | Proposal submitted | Week 10 |
| First Working Demo | Run a pretrained YOLO model end-to-end on sample worksite images | Input, prediction, and output all work | Week 11 |
| Make It Yours | Fine-tune YOLO on the PPE dataset and add compliance logic | SafeSight works on the selected problem | Weeks 12-13 |
| Improve and Measure | Test unseen images, review failure cases, and measure performance | mAP50 and inference speed recorded | Week 14 |
| Package and Present | Complete demo, documentation, slides, and presentation | Final project submitted | Week 15 |

## Resources
- **Compute:** Google Colab
- **Backup Compute:** Kaggle Notebooks or HCC Heidi
- **Frameworks:** Ultralytics and PyTorch
- **Cost:** $0 using free-tier and open-source resources

## Risks and Mitigation

| Risk | Probability | Plan B |
|---|---|---|
| The model does not reliably identify missing PPE | Medium | Narrow the first version to hard-hat compliance or use the baseline model while focusing on application logic and evaluation |
| Colab training or GPU availability becomes a limitation | Medium | Use a smaller model or fewer training epochs, save checkpoints, or move training to Kaggle or HCC Heidi |

## Demo Video
A demo video link will be added for the Final submission.

## AI Usage Log
See [docs/AI_usage_log.md](docs/AI_usage_log.md)

## Current Status
- [x] Repository created
- [x] Project idea and tier selected
- [x] Dataset selected
- [ ] Proposal submitted
- [ ] First working demo
- [ ] System works on project data
- [ ] Metrics measured
- [ ] Final submitted
