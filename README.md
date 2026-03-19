# MRAC---Adaptive-Control-Algorithm
# 🌡️ Model Reference Adaptive Control (MRAC) for a Thermal System

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue.svg?logo=linkedin&style=flat-square)](https://www.linkedin.com/in/ahmed-ouf-2014a)
[![MATLAB](https://img.shields.io/badge/MATLAB-Simulink-orange.svg?style=flat-square&logo=mathworks)](#)
[![Control Systems](https://img.shields.io/badge/Control_Systems-Adaptive_Control-success.svg?style=flat-square)](#)

**Developed by:** [Ahmed Ouf](https://www.linkedin.com/in/ahmed-ouf-2014a) | Standard DSP and Control Algorithm Engineer

---

## 👨‍💻 Professional Profile
I am a **Mechatronics Engineer** with over 2 years of hands-on industry experience in **Model-Based Design (MBD)**, **Digital Signal Processing (DSP)**, and **Advanced Control Systems**. I have successfully guided algorithm development through the entire testing cycle (MIL, SIL, PIL, HIL) for medical devices (ventilators) and standard-compliant power quality monitors.

Currently, I am pursuing an M.Sc. in Automatic Control, Cybernetics, and Robotics at Gdańsk University of Technology. My technical stack includes MATLAB/Simulink, Stateflow, Embedded Coder, C/C++, and C#.


---

## 🚀 Project Overview
This repository contains a complete simulation project demonstrating the design, mathematical derivation, and Simulink implementation of a **Normalized Model Reference Adaptive Controller (MRAC)** based on the **MIT Rule**. 

The algorithm is designed to control a first-order thermal plant (temperature control of a body), ensuring the plant output perfectly tracks an ideal reference model while robustly handling ambient temperature disturbances, actuator saturation, and measurement noise.

### ✨ Key Features & Technical Skills Demonstrated
* **Control Systems:** Model Reference Adaptive Control (MRAC), Disturbance Rejection, Anti-Windup mechanisms.
* **Mathematical Modeling:** First-order differential equations, Laplace transforms, perfect matching approximation, and Gradient Descent optimization.
* **Software Tools:** MATLAB, Simulink, System Modeling (MBD approach).
* **Technical Writing:** Comprehensive mathematical documentation using LaTeX (Source code included).

---

## 🧮 Mathematical Foundation & Control Strategy

### 1. System Dynamics
The system comprises a plant (thermal body) and a reference model representing the desired ideal dynamics. To handle ambient temperature disturbances ($T_a$), the control law is augmented with a feedforward term ($\theta_3$).
*   **Plant Model:** $\dot{T} = -a_p T + b_p u + a_p T_a$
*   **Reference Model:** $\dot{T}_m = -a_m T_m + b_m T_r$

### 2. Augmented Control Law
We utilize three adaptive parameters ($\theta_1, \theta_2, \theta_3$) to control tracking, state feedback, and disturbance rejection respectively:
$$u(t) = \theta_1 T_r(t) - \theta_2 T(t) - \theta_3 T_a(t)$$

### 3. Normalized Adaptation (MIT Rule)
To ensure absolute stability regardless of signal magnitudes, the parameter update laws are normalized by the squared norm of the sensitivity vector ($\phi$):
$$\dot{\theta}_1 = - \frac{\gamma \cdot e \cdot \phi_1}{\alpha + \phi_1^2 + \phi_2^2 + \phi_3^2}$$
$$\dot{\theta}_2 = + \frac{\gamma \cdot e \cdot \phi_2}{\alpha + \phi_1^2 + \phi_2^2 + \phi_3^2}$$
$$\dot{\theta}_3 = + \frac{\gamma \cdot e \cdot \phi_3}{\alpha + \phi_1^2 + \phi_2^2 + \phi_3^2}$$

### 4. Anti-Windup Mechanism
To handle the physical limits of the heater/cooler ($u_{max} = 2.5 kW, u_{min} = -1.2 kW$), a custom anti-windup logic is implemented. When $u(t)$ saturates, the adaptation derivatives are forced to zero ($\dot{\theta} = 0$), preventing integral windup of the adaptive parameters.

---

## 🧪 Simulation & Test Cases

The MRAC controller was rigorously validated in Simulink across 6 specific test cases:

1.  **Case A (Parameter Tuning):** Verified that the adaptive parameters ($\theta_1, \theta_2, \theta_3$) converge accurately to their theoretical optimums leading to zero steady-state tracking error.
2.  **Case B (Unconstrained Control):** Demonstrated perfect tracking of a $20^\circ C \to 30^\circ C$ step response.
3.  **Case C (Disturbance Rejection):** Evaluated controller behavior when the ambient temperature suddenly drops to $15^\circ C$. The controller effectively adjusted power to maintain the setpoint.
4.  **Case D & E (Actuator Constraints & Anti-Windup):** Tested the system's stability when the control signal hits the physical boundaries ($900W$ and $-600W$). The custom Anti-Windup block successfully paused adaptation during saturation, preventing system instability.
5.  **Case F (Robustness to Noise):** Injected Gaussian measurement noise ($\sigma = 0.5^\circ C$) into the feedback loop. The controller maintained robust stability without diverging.

*(Detailed plots, Simulink block diagrams, and analyses are available in the enclosed LaTeX report).*

---

## ⚙️ How to Run the Simulation

1. Clone this repository to your local machine:
   bash
   git clone [https://github.com/AhmedOuf19/MRAC---Adaptive-Control-Algorithm.git](https://github.com/AhmedOuf19/MRAC---Adaptive-Control-Algorithm.git)
2. Open MATLAB and navigate to the cloned directory.

3. Open the Simulink model Simulink_Model/MRAC_Thermal_System.slx.

4. The system parameters will load automatically into the workspace via the model's InitFcn callback.

5. Run the simulation and open the Scopes to view the temperature tracking, control effort, and parameter convergence.
