# GymBro – Computer Vision Based Exercise Form Analysis System

GymBro is a computer vision based exercise form analysis and feedback system for strength training. The system currently focuses on bicep curls, where it uses pose estimation to track joint positions, evaluate movement patterns, and classify repetitions as correct or incorrect form. The system is also optimised for Google Colab to simplify setup and execution, while remaining fully compatible with local Python environments.

It detects common issues such as elbow drift, incomplete range of motion, and poor movement control, while also identifying properly executed repetitions. The architecture is exercise-agnostic, allowing the same pipeline to be extended to other exercises by defining new movement rules or integrating learned classifiers.

---

## Key Capabilities

- Joint-level pose tracking using computer vision
- Per-repetition form evaluation for bicep curls
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
- Human-readable feedback generation with direct and concise why, risks, and fix analysis
- Exercise-agnostic pipeline design for future extension
- Generation of Labled Videos, CSV with all Data, and full JSON Report
- **Optimised for Google Colab, with full support for local execution.**

---

## System Highlights

- Environment-aware execution (Google Colab optimised with local fallback)
- Reproducible pipeline using deterministic seeding and stratified splits
- Primary subject tracking to ignore background bystanders
- Automated per-clip quality assurance and sanity checks
- Label-free, rule-driven exercise form evaluation
- Optional learned classifiers for extensibility
- Idempotent output handling for safe and stable re-runs

---

## How the System Works

1. **Video Input**
   - Exercise videos are loaded from structured directories
   - Each clip contains a primary subject performing bicep curls

2. **Pose Estimation**
   - Pose keypoints are extracted using MediaPipe Pose and YOLOv8
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

---

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
├── form_classifier_model.pkl
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
  Optional trained model for extended form evaluation

---

## Setup

### Option A – Google Colab (Recommended)

GymBro is optimized and primarily tested on Google Colab.  
This is the simplest and most stable way to run the system.

#### Steps

1. Open the notebook in Colab:

   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://drive.google.com/file/d/1HdUp-Pomo1WEeRMnDpObzptFvtBRKTip/view?usp=sharing)

2. Mount Google Drive when prompted.

3. If training, place your training videos inside **test_videos** folder

4. Run the notebook cells **top to bottom**.

No local installation is required. All dependencies are installed automatically.

### Option B – Local Python Environment

Local execution is supported for users who prefer running the notebook on their own machine.
This requires manual dependency installation and environment management.

---

### Requirements

- Python 3.9+
- pip
- Jupyter Notebook or JupyterLab
- FFmpeg available in system PATH

---

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

Create missing folders if required.

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

````

> When running in Google Colab, dependency installation and environment handling are managed automatically inside the notebook.

---

## Usage

1. Open the notebook:

```bash
jupyter notebook exercise_form.ipynb
````

2. Run the cells in sequence to:
   - Extract pose keypoints
   - Analyse repetitions
   - Evaluate form quality
   - Generate feedback

3. Outputs are written safely and can be regenerated without conflicts.

---

## Tech Stack

- Python
- OpenCV
- MediaPipe Pose
- YOLOv8
- XGBoost
- scikit-learn
- NumPy
- Pandas
- joblib
- Google Colab
- Jupyter Notebook

---

## Notes

- Designed for educational and research purposes
- Accuracy depends on camera angle, lighting, and subject visibility
- Easily extensible to new exercises through rule definitions

---

## License

MIT License  
Copyright (c) 2026 Chanitha Abeygunawardena
