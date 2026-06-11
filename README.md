# PINN-Fourier-Lorenz

## Physics-Informed Neural Networks for Chaotic Dynamical System Reconstruction

This repository presents a research project conducted during my Master’s degree in Mathematical Engineering and Data Science at the University of Haute-Alsace (UHA). The project focuses on the application of **Physics-Informed Neural Networks (PINNs)** to solve and reconstruct chaotic dynamical systems governed by ordinary differential equations.

The Lorenz system was chosen as the benchmark problem because of its highly nonlinear and chaotic behavior.

The main objective of this work is to compare a classical PINN architecture with an enhanced PINN using **Fourier Features** in order to improve the learning of high-frequency chaotic dynamics.

---

## 📌 Project Overview

Two neural network architectures are investigated:

### 🔹 Standard PINN

A classical multilayer perceptron (MLP) receiving normalized time as input.

### 🔹 PINN with Fourier Features

The same MLP architecture enriched with sinusoidal Fourier embeddings using multiple frequencies:

[
f_k \in {1,2,4,8,16,32}
]

This encoding helps reduce the spectral bias of neural networks with `tanh` activation functions and significantly improves the approximation of chaotic trajectories.

---

## ⚙️ Lorenz Dynamical System

The Lorenz system is defined by:

[
\dot{x} = \sigma(y-x)
]

[
\dot{y} = x(\rho-z)-y
]

[
\dot{z} = xy-\beta z
]

with:

* (\sigma = 10)
* (\rho = 28)
* (\beta = 8/3)

These parameters generate the famous chaotic butterfly attractor.

---

## 🎯 Objectives

This project aims to:

* Evaluate the performance of PINNs on a chaotic nonlinear system.
* Compare a standard PINN with a Fourier-enhanced PINN.
* Study both direct and inverse problems.
* Investigate partial observability when only one variable is measured.

---

## 🔬 Problems Studied

### ✅ Direct Problem

Predict the full dynamical state:

[
(x(t), y(t), z(t))
]

using reference trajectories generated with an RK45 numerical solver.

---

### ✅ Inverse Problems

Reconstruct the complete dynamics while observing only one variable:

* (x(t))
* or (y(t))
* or (z(t))

This simulates realistic scenarios with a single physical sensor.

---

## 🧠 Model Architecture

### Neural Network

* 4 hidden layers
* 256 neurons per layer
* `tanh` activation
* linear output layer

### Training Data

* 500 RK45 reference points
* 1000 collocation points for ODE residuals

### Loss Function

The total loss combines:

* supervised data loss
* physics-based residual loss

[
\mathcal{L}
===========

\lambda_{data}\mathcal{L}*{data}
+
\lambda*{ODE}\mathcal{L}_{ODE}
]

Residuals are normalized to balance the contribution of each variable.

---

## 🚀 Optimization Strategy

Training is performed in three stages:

1. Supervised pretraining
2. Progressive introduction of physics constraints
3. Final refinement using L-BFGS optimization

This curriculum strategy improves convergence and training stability.

---

## 📊 Main Results

| Problem              | Model            | Global L2 Error |
| -------------------- | ---------------- | --------------- |
| Direct Problem       | Standard PINN    | 0.105           |
| Direct Problem       | **PINN-Fourier** | **0.061**       |
| Inverse – Z observed | Standard PINN    | 0.235           |
| Inverse – Z observed | PINN-Fourier     | 0.206           |
| Inverse – Y observed | Standard PINN    | 0.251           |
| Inverse – Y observed | **PINN-Fourier** | **0.197**       |
| Inverse – X observed | Standard PINN    | 0.277           |
| Inverse – X observed | PINN-Fourier     | 0.215           |

---

## 📈 Key Findings

* Fourier Features consistently improve prediction accuracy.
* PINN-Fourier better reconstructs chaotic attractors.
* The variable (y) is the most informative for reconstruction.
* Partial observability remains a major limitation in inverse problems.

---

## 🌍 Applications

### 🌦️ Weather and Climate Modeling

* Atmospheric state reconstruction
* Data assimilation
* Extreme event forecasting

### 🏭 Industrial Monitoring

* Predictive maintenance
* Chaotic system monitoring
* Sensor reduction strategies

### ⚡ Scientific Computing

* Physics-guided machine learning
* Efficient differential equation solving
* Hybrid AI-physics simulation methods

---

## 🛠️ Technologies

* Python
* PyTorch
* NumPy
* SciPy
* Matplotlib

---

## 👨‍🎓 Author

Kossi NOUMAGNO
Master’s Student in Mathematical Engineering and Data Science
University of Haute-Alsace (UHA)

Supervised by Professor Cornel Marius Murea.

---

## 📄 License

This project is released under the MIT License.
