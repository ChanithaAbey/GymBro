# GymBro – Computer Vision Based Exercise Form Analysis System [![Google Drive](https://img.shields.io/badge/Google%20Drive-black?logo=google-drive)](https://drive.google.com/drive/folders/1Z8DeWwoG_3IULlUwUt1n7W0ZKEesfmnf?usp=sharing)

GymBro is a computer vision based exercise form analysis and feedback system for strength training. The system currently focuses on bicep curls, where it uses pose estimation to track joint positions, evaluate movement patterns, and classify repetitions as correct or incorrect form with a full diagnosis. The system is also optimised for Google Colab to simplify setup and execution, while remaining fully compatible with local Python environments.

It detects common issues such as elbow drift, incomplete range of motion, and poor movement control, while also identifying properly executed repetitions. The architecture is exercise-agnostic, allowing the same pipeline to be extended to other exercises by defining new movement rules or integrating learned classifiers.

The final verdict combines a learned classifier with rule-based biomechanical checks, allowing high-confidence form issues to override the model when necessary.

## Key Capabilities

- Joint-level pose tracking using computer vision
- Repetition-aware motion analysis with exercise form evaluation for bicep curls
- Detection of common form issues:
  - Trunk swing
  - Elbows forward
  - Elbows flare
  - Shoulder shrug
  - Wrist extended
  - Wrist flexed
  - Half reps at the top
  - Half reps at the bottom
  - Leg drive
  - Torso twist
  - Neck forward
  - Asymmetric arm movement
  - Out-of-sync arms
  - Speed bouncing
  - Constant lean

- Detection of positive execution cues:
  - Stable torso with minimal swing
  - Upright posture with no backward lean
  - Elbows staying close to the torso
  - Elbows not drifting forward during the curl
  - Full range of motion, reaching both bottom and top positions
  - No leg drive, with hips remaining stable
  - Square torso without rotational twisting
  - Symmetrical movement between left and right arms
  - Arms moving in sync with good timing
  - Controlled tempo with no bouncing or rebounding
- Human-readable feedback generation with concise explanations of:
  - Why an issue occurred
  - Potential risks
  - How to fix it
- Exercise-agnostic pipeline design for future extension
- Generation of Labled Videos, CSV with all Data, and full JSON Report
- **Optimised for Google Colab, with full support for local execution.**

## System Highlights

- Environment-aware execution (Google Colab optimised with local fallback)
- Reproducible pipeline using deterministic seeding and stratified splits
- Primary subject tracking to ignore background bystanders
- Automated per-clip quality assurance and sanity checks
- Label-free, rule-driven exercise form evaluation
- Optional learned classifiers for extensibility
- Idempotent output handling for safe and stable re-runs

## How the System Works

1. **Video Input**
   - Exercise videos are loaded from structured directories
   - Each clip contains a primary subject performing bicep curls

2. **Pose Estimation**
   - Pose keypoints are extracted using YOLOv8
   - Only the primary subject is tracked to avoid interference

3. **Movement Analysis**
   - Joint trajectories and angles are computed over time
   - Repetitions are segmented from continuous motion

4. **Form Evaluation**
   - Rule-based biomechanical checks are applied per repetition
   - Common mistakes are detected and flagged

5. **Feedback Generation**
   - Each repetition is marked as correct or incorrect
   - Specific feedback is generated based on detected issues

6. **Optional Learning-Based Models**
   - Keypoint features can be used to train ML models for extension
   - Trained models are saved for reuse and inference

## Folder Structure

```
.
├── artifacts/
├── exercise_videos/
├── exercise_labeled_videos/
├── exercise_keypoints/
├── test_videos/
│
├── exercise_form.ipynb
```

### Folder Overview

- `exercise_videos/`  
  Raw exercise videos used for analysis and training

- `exercise_labeled_videos/`  
  Videos labelled for evaluation and validation

- `exercise_keypoints/`  
  Extracted pose keypoints stored for reuse

- `test_videos/`  
  Unseen videos for inference and testing

- `artifacts/`  
  Intermediate outputs, logs, and cached results

- `exercise_form.ipynb`  
  Main notebook containing the full analysis pipeline

- `form_classifier_model.pkl`  
  Model generated during training

## Setup

### Option A – Google Colab (Recommended)

GymBro is optimized and primarily tested on Google Colab.  
This is the simplest and most stable way to run the system.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://drive.google.com/file/d/1HdUp-Pomo1WEeRMnDpObzptFvtBRKTip/view?usp=sharing)

#### Steps

1. Open the notebook in Colab:

2. If training, place your training videos inside **test_videos** folder

3. Upload test video to test_videos

4. Select t4 GPU runtime type.

5. Run the notebook cells **top to bottom**.

6. Mount Google Drive when prompted.

7. Enter video name when prompted.

No local installation is required. All dependencies are installed automatically.

### Option B – Local Python Environment

Local execution is supported for users who prefer running the notebook on their own machine.
This requires manual dependency installation and environment management.

### Requirements

- Python 3.9+
- pip
- Jupyter Notebook or JupyterLab
- FFmpeg available in system PATH
- FFmpeg is required for reliable video encoding when writing labeled output videos.

### 1. Clone the repository

```bash
git clone https://github.com/ChanithaAbey/GymBro.git
cd GymBro
```

---

### 2. Create a virtual environment (recommended)

```bash
python -m venv venv
```

Activate it:

**Windows**

```bash
venv\Scripts\activate
```

**macOS / Linux**

```bash
source venv/bin/activate
```

---

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

### 4. Verify FFmpeg

```bash
ffmpeg -version
```

If FFmpeg is not installed:

- Windows: install from https://ffmpeg.org or via Chocolatey
- macOS: `brew install ffmpeg`
- Linux: `sudo apt install ffmpeg`

---

### 5. Folder structure

Ensure the project directory contains:

```
GymBro/
├── exercise_form.ipynb
├── exercise_videos/
├── test_videos/
├── artifacts/
├── exercise_keypoints/
├── exercise_labeled_videos/
```

---

### 6. Run the notebook

```bash
jupyter notebook exercise_form.ipynb
```

Run all cells from top to bottom.

---

### Notes

- GPU acceleration is optional but not required for local execution.
- Performance depends on CPU speed and video resolution.
- Camera angle and lighting of training videos significantly affect pose detection quality.
- When running in Google Colab, dependency installation and environment handling are managed automatically inside the notebook.

## Usage

1. Open the notebook:

```bash
jupyter notebook exercise_form.ipynb
```

2. Run the cells in sequence to:
   - Extract pose keypoints
   - Analyse repetitions
   - Evaluate form quality
   - Generate feedback

3. Outputs are written safely and can be regenerated without conflicts.

## Tech Stack

- **Python** – Core language for the entire pipeline
- **YOLOv8 Pose** – Real-time pose estimation and joint keypoint extraction
- **OpenCV** – Video decoding, frame processing, and labeled output video generation
- **NumPy** – Numerical operations on joint trajectories and motion signals
- **Pandas** – Structured handling of per-video and per-metric data
- **scikit-learn** – Feature processing and traditional ML utilities
- **XGBoost** – Optional learned classifier for form quality prediction
- **joblib** – Model serialization and reuse
- **Google Colab** – Primary execution environment with GPU support
- **Jupyter Notebook** – Interactive development, analysis, and experimentation

## Notes

- Designed for educational and research purposes
- Accuracy depends on camera angle, lighting, and subject visibility
- Easily extensible to new exercises through rule definitions

## Example - Console Output

```
Enter File Name: TestBad.mov
[ok] wrote TestBad_keypoints.csv and output_TestBad.mov

Form Diagnosis
Bad form  —  classifier=Bad, strong_issues=1

Positive Points:
 • Stable torso — minimal swing.
 • Elbows don’t drift forward.
 • Full range — reaches both bottom and top.
 • Square torso — no twist.
 • Good timing — arms move together.
 • Controlled tempo — no bounce.

Negative Points:
 • asymmetric  (confidence 0.96)
    Why This Happened:    One arm stronger or drifting more than the other.
    Possible Side-Effects:  Imbalances and joint stress on the dominant side.
    How to Fix:    Lighten the load and match the weaker side’s range.
 • elbows flare  (confidence 0.66)
    Why This Happened:    Letting upper arms drift out to the sides.
    Possible Side-Effects:  Shoulder internal rotation stress and poor biceps line of pull.
    How to Fix:    Squeeze a towel in your armpits or stand closer to a wall.
 • shoulder shrug  (confidence 0.60)
    Why This Happened:    Compensating with traps as the weight rises.
    Possible Side-Effects:  Neck tightness and upper trap overuse.
    How to Fix:    Keep shoulders down and back. Think pockets heavy.

[ok] saved detailed report -> /content/drive/MyDrive/GymBro/artifacts/TestBad_report.json
```

## License

MIT License | Copyright (c) 2026 Chanitha Disas Abeygunawardena
