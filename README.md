# SafeSight PPE

## Project Overview

**SafeSight PPE** is an AI-assisted computer vision project that reviews construction-site images for possible personal protective equipment (PPE) compliance issues.

The system uses a fine-tuned YOLO11 object-detection model to identify workers, hard hats, safety vests, and possible missing PPE. The detections are then passed through simple compliance-review logic that produces an annotated image and a human-readable review summary.

SafeSight is designed as a **review aid**, not as a replacement for a human safety professional.

---

## Team Member

- Keith Walker - W210940946@student.hccs.edu

---

## Project Tier

**Tier 2: Advanced**

SafeSight PPE combines two components:

1. YOLO-based PPE object detection
2. Application logic that converts raw detections into a compliance-review result

The second component adds decision logic beyond basic object detection by checking for explicit missing-PPE detections, incomplete PPE coverage, and conflicting results.

---

## Problem Statement

Construction safety teams may need to review large numbers of worksite images to determine whether workers appear to be wearing required PPE such as hard hats and safety vests.

Manual review can become time-consuming and inconsistent, especially when multiple workers appear in the same image.

SafeSight PPE explores whether computer vision can assist with this process by identifying possible PPE concerns and directing questionable images to human review.

---

## System Workflow

**Worksite Image → YOLO PPE Detection → Compliance Logic → Annotated Image + Review Summary**

The final application:

- Accepts a construction-site image
- Runs a fine-tuned YOLO11n model
- Detects PPE-related classes
- Displays bounding boxes and confidence scores
- Counts relevant detections
- Checks for missing, incomplete, or conflicting PPE evidence
- Returns either:
  - `REVIEW REQUIRED`
  - `NO PPE VIOLATIONS DETECTED`

---

## PPE Classes Used by SafeSight

The full dataset contains 10 object classes:

- Hardhat
- Mask
- NO-Hardhat
- NO-Mask
- NO-Safety Vest
- Person
- Safety Cone
- Safety Vest
- machinery
- vehicle

The SafeSight compliance report focuses on five classes:

- Person
- Hardhat
- NO-Hardhat
- Safety Vest
- NO-Safety Vest

All 10 original classes were retained during training so the YOLO annotation class IDs remained aligned with the dataset.

---

## Dataset

- **Dataset:** Construction Site Safety
- **Source:** Roboflow / Kaggle
- **Task:** Object Detection
- **Total Images:** 2,801
- **Training Images:** 2,605
- **Validation Images:** 114
- **Test Images:** 82
- **Image Size:** 640 × 640
- **License:** CC BY 4.0

Dataset link:

https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow

---

## Technical Approach

- **Computer Vision Task:** Object Detection
- **Model:** YOLO11n
- **Framework:** Ultralytics / PyTorch
- **Training Method:** Transfer Learning
- **Training Epochs:** 25
- **Image Size:** 640
- **Batch Size:** 16
- **Compute:** Google Colab with NVIDIA Tesla T4 GPU

I started with pretrained YOLO11n weights and fine-tuned the model using the Construction Site Safety dataset.

A baseline test was also performed before fine-tuning. The pretrained COCO model could detect workers as `Person`, but it could not identify PPE-specific classes such as Hardhat or Safety Vest.

---

## Final Test Results

The final model was evaluated on the held-out 82-image test set.

| Metric | Original Target | Final Result | Outcome |
|---|---:|---:|---|
| mAP50 | ≥ 0.70 | **0.692** | Missed narrowly |
| Inference Speed | < 1 second/image | **6.95 ms/image** | Met |
| Precision | Not specified | **0.797** | Measured |
| Recall | Not specified | **0.636** | Measured |
| mAP50-95 | Not specified | **0.385** | Measured |

The mAP50 result missed the original 0.70 target by 0.008, while inference speed was significantly faster than the original one-second target.

---

## Selected Class Performance

Some PPE classes performed better than others on the held-out test set.

| Class | Test mAP50 |
|---|---:|
| Hardhat | 0.870 |
| Person | 0.748 |
| Safety Vest | 0.804 |
| NO-Safety Vest | 0.712 |
| NO-Hardhat | 0.536 |

One important finding was that `NO-Hardhat` was less reliable than the positive `Hardhat` class. This influenced the final compliance logic.

---

## Compliance Review Logic

SafeSight does not treat every model prediction as a final safety decision.

An image is marked `REVIEW REQUIRED` when:

- `NO-Hardhat` is explicitly detected
- `NO-Safety Vest` is explicitly detected
- Fewer hard hats are detected than workers
- Fewer safety vests are detected than workers
- Detection totals suggest duplicate or conflicting PPE predictions

If none of these conditions are present, the application returns:

`NO PPE VIOLATIONS DETECTED`

This wording is intentional. A negative model result does not guarantee that a worksite is safe.

---

## Testing and Failure Cases

Several real construction images were used to test the completed workflow.

### Compliant PPE Example

A test image visually showed three workers wearing hard hats and safety vests.

The model detected:

- 3 Persons
- 4 Hardhats
- 3 Safety Vests

Because the number of detected hard hats exceeded the number of workers, SafeSight treated the result as a possible duplicate/conflict and returned `REVIEW REQUIRED`.

### Missing PPE Example

A worker who appeared to be missing both a hard hat and safety vest was tested.

The model correctly detected `NO-Safety Vest`, but it did not explicitly detect `NO-Hardhat`.

Because the worker count exceeded the number of detected hard hats, the additional coverage logic still caused SafeSight to return `REVIEW REQUIRED`.

### False Positive Example

During testing, the model incorrectly classified a bright green plant as a `Person` and produced PPE-related predictions in the same area.

I tested person-relative PPE filtering and person-by-person image cropping as possible improvements. These approaches did not consistently solve the issue and sometimes introduced additional conflicting detections.

I decided not to include those experimental methods in the final workflow.

This showed that an error in an early object detection can affect later application logic.

---

## Limitations

SafeSight still has several limitations:

- Missing-PPE classes are less reliable than some visible PPE classes.
- False-positive Person detections can affect later compliance logic.
- Bounding boxes may overlap or produce duplicate predictions.
- The current system uses image-level counts rather than permanently matching each PPE item to a specific worker.
- Results should be reviewed by a person before making a safety decision.

Future improvements could include stronger person-to-PPE association, additional training data, class balancing, and more targeted testing of difficult examples.

---

## Repository Structure

```text
safesight-ppe/
├── README.md
├── requirements.txt
├── data/
│   └── README.md
├── docs/
│   ├── AI_usage_log.md
│   └── proposal.pdf
├── notebooks/
│   └── SafeSight_PPE_POC.ipynb
└── results/
