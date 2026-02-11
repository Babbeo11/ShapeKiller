# ShapeKiller (AI State Tree Research)

> **A stealth AI prototype developed to explore the capabilities of Unreal Engine 5's State Trees and EQS in a dynamic "Prop Hunt" environment.**

## 🎓 Context & Goal
Developed as part of a 4-person student team project. While the core game loop involves shapeshifting mechanics (Hitman meets Prop Hunt), my specific role was **AI Programmer**.

**My Technical Objective:**
My goal was **R&D (Research & Development)**: moving away from standard Behavior Trees to master the experimental **UE5 State Tree** system and **Environment Query System (EQS)**, evaluating their efficiency for stealth gameplay.

---

## 🎥 AI Behavior Demo

| Patrol & Detection (Gameplay) | AI Perception Debug (Developer View) |
| :---: | :---: |
| ![Gameplay](https://github.com/user-attachments/assets/4fc35039-69ec-4bd8-bfcf-7ed9062f668e) | ![Debug](https://github.com/user-attachments/assets/3a608a15-0ad4-4015-b31a-fd157a1ce906) |
| *Smooth state transitions.* | *Real-time vision & hearing logic.* |

*(Note: The shapeshifting mechanic shown is used to test the AI's ability to distinguish props from players.)*

---

## 🧠 Key Technical Features

### 1. Hierarchical State Tree Architecture
The AI logic is not a single monolith but a modular system using **Linked Assets** (Sub-Trees). The Main Tree handles high-level state transitions, while specific behaviors are encapsulated in their own assets.
<br><br>
<img width="1385" height="801" alt="MainStateTree" src="https://github.com/user-attachments/assets/3c1cb1a8-d4bf-4735-ab4e-885b146d99e0" />
*Fig 1. The Main State Tree structure. Note the clean transition logic from "Routine" to "Investigating" and "Chase", delegating execution to Linked State Trees (e.g., EnterPatrolSplineTree).*

**Core States:**
* **Routine:** Handles Idle and Patrol behaviors.
* **Investigation:** Triggered by noise events (using AI Perception).
* **Chase:** Dynamic combat state that switches between *Shoot* and *Follow* based on distance.
* **Seeking:** Activated when the target breaks line-of-sight during a chase.

### 2. Predictive Seeking (EQS)
To make the AI feel relentless, I implemented a **Predictive Pursuit** system using EQS when Line of Sight is broken.
* **Last Known Location:** The AI first rushes to the exact point where the player was last seen.
* **Forward Prediction:** Upon arrival, instead of searching randomly, it runs an **EQS Cone Query** aligned with its forward vector.
* **Trajectory Simulation:** It selects a point ahead to simulate predicting the target's momentum, assuming the player kept running in their escape direction.

### 3. Designer-Friendly Patrol System
I created a flexible patrol component that allows Level Designers to set up paths without touching code.
* **Dual Mode:** The AI can follow a specifically placed **Spline** or a list of **Waypoints (Actors)**.
*Exposed Parameters:** Designers can easily toggle between modes, adjust wait durations, and loop settings directly from the Details Panel.

<br>
<table width="100%" border="0" cellspacing="0" cellpadding="0" style="border-collapse: collapse; border: none;">
  <tr>
    <td width="55%" valign="center" style="border: none; padding-right: 10px;">
      <img src="https://github.com/user-attachments/assets/e41cb592-6182-4b02-934a-4f5701567266" width="100%" style="display: block;" />
    </td>
    <td width="45%" valign="center" style="border: none;">
      <img src="https://github.com/user-attachments/assets/47c8afc8-0fe3-4e29-822b-f5c1eb4a10a2" width="100%" style="display: block;" />
    </td>
  </tr>
  <tr>
    <td align="center" style="border: none; padding-top: 10px;"><i><b>Fig 2a. In-Editor Visualization:</b> The patrol path and idle points as seen by the level designer.</i></td>
    <td align="center" style="border: none; padding-top: 10px;"><i><b>Fig 2b. Data-Driven Settings:</b> Exposed parameters for state switching and wait durations.</i></td>
  </tr>
</table>
<br>

### 4. Data-Driven Detection System
The detection logic is built on **Gameplay Tags** and **Data Assets** to keep the rules scalable.
* **Zone Logic:** Collision volumes apply tags like `State.Zone.Public` or `State.Zone.Private`.
* **Context Awareness:** The suspicion meter calculation is dynamic:
    * *Public Zone:* Suspicion rises only if the player performs illegal actions (Running, Jumping, Transforming).
    * *Private Zone:* Suspicion rises immediately upon visual contact.
    * *Distance Scaling:* Detection speed is modulated by the distance between the AI and the target.

---
*Tools used: Unreal Engine 5, State Trees, EQS, AI Perception System, Blueprints.*
