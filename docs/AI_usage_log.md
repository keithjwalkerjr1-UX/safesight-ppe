# AI Usage Log

## Entry 1 — September 22, 2026
**Tool:** ChatGPT

**What I asked:**  
I used ChatGPT to help clarify the project tier requirements and talk through how my PPE compliance idea could fit the course guidelines.

**What AI suggested:**  
ChatGPT explained the difference between Tier 1 and Tier 2 and gave examples of how a reporting component could work with object detection.

**What I learned:**  
I learned that fine-tuning a single YOLO model still falls under Tier 1, while combining detection with another application component can qualify as Tier 2.

**How I applied it:**  
I decided to keep SafeSight PPE as a Tier 2 project by pairing PPE detection with a simple compliance-review summary.

---

## Entry 2 — September 22, 2026
**Tool:** ChatGPT

**What I asked:**  
I asked ChatGPT to help compare several publicly available PPE datasets I was considering.

**What AI suggested:**  
ChatGPT summarized the main differences in dataset size, labels, and project fit.

**What I learned:**  
I learned that dataset size is not the only factor to consider. The available labels and how closely they match the project goal are also important.

**How I applied it:**  
After reviewing the options, I chose the Construction Site Safety dataset because its labels matched the PPE conditions I wanted SafeSight to evaluate.

---

### Entry 3 — Baseline YOLO Testing
**Date:** October 9, 2026

I used AI to help explain the output from the pretrained YOLO11 model and how the confidence threshold affects detections. I tested the same construction image at different confidence levels and observed that raising the threshold from 0.25 to 0.90 removed the two Person detections because their confidence scores were about 0.88.

**What I learned:** A higher confidence threshold can reduce weak detections, but it can also remove valid objects.

---

### Entry 4 — Dataset Configuration and Class Mapping
**Date:** October 9, 2026

I used AI to help review the dataset folder structure and understand how the YOLO class IDs matched the PPE class names. I decided to keep all 10 original classes in the training configuration instead of removing the classes that SafeSight does not directly report.

**What I learned:** YOLO class numbers must stay aligned with the original annotations. Changing the class order without changing the labels would cause the model to interpret objects incorrectly.

---

### Entry 5 — Model Training and Evaluation
**Date:** October 9, 2026

I used AI to help interpret the training and test metrics after fine-tuning YOLO11n for 25 epochs. The held-out test set produced a mAP50 of 0.692, which was slightly below my original 0.70 target, while inference speed was about 6.95 ms per image.

**What I learned:** Validation performance and final test performance can be different. I also learned that individual class results are important because the NO-Hardhat class performed worse than several of the visible PPE classes.

---

### Entry 6 — Compliance Review Logic
**Date:** October 9, 2026

I used AI to help think through how SafeSight should handle conflicting and incomplete detections. During testing, I found examples where the model detected both Safety Vest and NO-Safety Vest or detected fewer hard hats than workers. I chose to make these cases return REVIEW REQUIRED instead of automatically treating the image as compliant.

**What I learned:** Object detection output should not automatically be treated as a final safety decision. Application logic can identify uncertainty and send questionable results for human review.

---

### Entry 7 — False Positive Investigation
**Date:** October 9, 2026

I noticed that the model incorrectly identified a bright green plant as a Person and produced PPE-related detections around it. I used AI to help test person-relative filtering and person-by-person image crops. The experiments did not consistently improve the results, so I decided not to include those approaches in the final SafeSight workflow.

**What I learned:** Errors in an early detection can affect later application logic. I also learned that testing an idea and deciding not to use it is still part of the engineering process.
