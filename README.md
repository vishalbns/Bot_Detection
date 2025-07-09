# 🧠 Human vs Bot Classifier

This project simulates pointer event sessions and builds a machine learning pipeline to classify whether the session comes from a human or a bot.

---

## 🧪 Overview

- **Sessions**: 100 total (50 human, 50 bot), each a list of `(x, y, timestamp)` events and a metadata field (`time_of_day`).
- **Features**: 5 handcrafted motion and timing features.
- **Model**: Random Forest Classifier.
- **Output**: ROC AUC = 1.0, perfect precision/recall on test set.

---

## 🛠️ What the Code Does

1. **Synthetic Data Generation**:
    - **Human sessions**: Simulated using noisy, jittery pointer movements with irregular timing.
    - **Bot sessions**: Linear paths with consistent step size and timing — overly smooth and deterministic.

2. **Feature Extraction**:
    For each session, we extract:
    - `velocity_mean`: Average velocity across pointer events.
    - `velocity_std`: Standard deviation of velocity — captures motion jitter.
    - `inter_event_time_std`: Variability in time between pointer movements.
    - `direction_entropy`: How random the movement directions are (higher for humans).
    - `total_path_length`: Total distance covered by the pointer.

3. **Model Training**:
    - Data is split into training and test sets.
    - Random Forest classifier is trained and evaluated.
    - Metrics reported: ROC AUC, accuracy, precision, recall.
    - Confusion matrix and ROC curve are saved to the `results/` folder.

---

## 📊 Sample Metrics

| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| Bot   | 1.00      | 1.00   | 1.00     | 8       |
| Human | 1.00      | 1.00   | 1.00     | 12      |

- **Accuracy**: 1.00  
- **ROC AUC**: 1.00  
- Confusion matrix and ROC plot saved to: `results/confusion_matrix.png`, `results/roc_curve.png`

---

## 🚀 Next‑Step Considerations

### 1. Scaling to Real-World Data

To handle production data:

- **Device Signals**:
  - Use richer input like touch pressure, accelerometer, and gyroscope.
  - Integrate multi-modal sensor data for more nuanced features.

- **Multi-Session Context**:
  - Model user behavior over time, not just single sessions.
  - Include clickstream patterns, idle durations, and temporal trends.

- **Cross-Device Alignment**:
  - Normalize across different screen sizes, input methods, and refresh rates.

---

### 2. Adversarial Scenario + Defense

**Threat**:  
A bot mimics human sessions using real data replays or GAN-generated pointer paths.

**Defense Strategies**:
- Use **adversarial training** with near-human bot simulations.
- Track **micromovements** and **inconsistencies** in jitter or pauses.
- Combine pointer features with passive identifiers like IP entropy or device fingerprinting.

---

### 3. Low-Latency Deployment

- **Model optimization**:
  - Use model compression, ONNX export, or convert to JS-compatible formats (e.g., TensorFlow.js).
- **Client-side inference**:
  - Extract features and run inference in-browser (WebAssembly) to avoid server lag.
- **Real-time stream processing**:
  - Maintain rolling pointer windows; classify continuously under 50ms per session.

---
