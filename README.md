# Pill Identification System

| **Tablet** | **Capsule** | **Box** |
| :---: | :---: | :---: |
| ![Tablet](Images/Standard/Tablet.jpg) | ![Capsule](Images/Standard/Capsule.jpg) | ![Box](Images/Standard/Box.jpg) |
| **Blister** | **Ampoule** | **Vial** |
| ![Blister](Images/Standard/Blister.jpg) | ![Ampoule](Images/Standard/Ampoule.jpg) | ![Vial](Images/Standard/Vial.jpg) |

> **Portfolio Showcase**: This repository contains documentation for a project I developed during my employment. Therefore, source code is not available. This `README` serves as a high-level overview of the project, its key features, and my development role.

## Project Narrative & My Role

As the core developer, I was tasked with building a real-time Pill Identification system from the ground up. The project's initial scope of identifying **tablets** and **capsules** was quickly expanded, challenging me to create a more ambitious system capable of recognizing a diverse range of items including **medicine boxes, blisters, ampoules, and vials**. I was responsible for the **entire technical execution**, from research and data collection to model development and backend engineering.

My key challenges and responsibilities included:

*   **Custom Dataset Curation:** No public dataset existed for this specific task. I single-handedly addressed this by **photographing, processing, and labeling a comprehensive dataset** from medicines provided by the company, forming the foundation of the AI's accuracy.
*   **AI Engine Development:** I developed the **core computer vision engine using YOLO (Ultralytics)** to detect and classify all specified medicine forms, from simple pills to complex packaging.
*   **Integrating Diverse Object Recognition:** The mid-project expansion from pills to packaging was a major technical hurdle. This required me to **re-architect the computer vision pipeline**, moving it beyond simple shape and color analysis to a more complex system capable of identifying fundamentally different features, such as **printed text on boxes, the transparency of ampoules, and the unique structure of blister packs**.
*   **Full-Stack System Architecture:** I designed the **end-to-end system workflow** and built a robust backend with **Django**. This included creating a functional frontend interface used for crucial debugging and visual validation of the AI engine's performance.
*   **Mobile API Integration:** I engineered and provided a real-time **WebSocket API** to the mobile development team, enabling them to integrate the AI's identification results seamlessly into the production user-facing application.

This project was a fantastic opportunity to take a complex idea from a concept to a high-fidelity prototype, which was successfully **demonstrated at two major public technology events**.

## Tech Stack

*   **AI & Computer Vision:** Python, YOLO (Ultralytics), OpenCV
*   **Backend:** Django
*   **Frontend:** HTML, CSS, JavaScript, Tailwind CSS
*   **Databases & Caching:** SQLite, Redis

## Key Features

The system processes images to identify various forms of medicine in real-time, providing the following capabilities:

*   **Multi-Form Identification:** Identifies medicine in various forms including tablets, capsules, boxes, blisters, ampoules, and vials.
*   **Flexible Image Analysis:** The system is designed to process images at any orientation. For optimal accuracy with tablets and capsules, the items must not be stacked or overlapping.
*   **Robust Tablet Recognition:** Accurately identifies tablets even in challenging scenarios, such as when a package contains a non-uniform mix of pills (different sizes, colors, or shapes) or includes broken tablets.
*   **Fine-Grained Recognition:** Distinguishes between medicines with very similar visual appearances. For example, it can successfully differentiate between two types of ampoules in transparent bottles that both contain transparent liquid and feature similar green labels—a task that is challenging for the human eye.
*   **Standardized Identification:** Utilizes the **Thai Medicines Terminology (TMT)** number as a unique identifier, specifically focusing on the **Trade Product Unit (TPU)** category. This links every identification to a national standard, with data referenced from the official [TMT portal](https://tmt.this.or.th/).

## Showcase of Identification Capabilities

This section provides visual examples of the system's identification performance.

> **Note on Output:** The following results are shown as simplified JSON to represent the raw data extracted by the backend identification engine. This is not the final user-facing output.

---

### Standard Identification Examples

The system can successfully identify a wide range of medicine types from a single image.

| Input Image | System Output (JSON) |
| :--- | :--- |
| ![Tablet](Images/Standard/Tablet.jpg) | ```json { "form": "tablet", "tmt": "117222", "name": "BESTATIN 10", "shape": "oval", "color": "pink", "scoring": "single", "width": 14, "height": 9, "error": null }``` |
| ![Capsule](Images/Standard/Capsule.jpg) | ```json { "form": "capsule", "tmt": "786858", "name": "MAGORAL", "shape": "oblong", "color": "yellow-green", "scoring": "none", "width": 20, "height": 8, "error": null }``` |
| ![Box](Images/Standard/Box.jpg) | ```json { "form": "box", "tmt": "592601", "name": "BROVOL PIP" }``` |
| ![Blister](Images/Standard/Blister.jpg) | ```json { "form": "blister", "tmt": "1134347", "name": "BERLONTIN 100" }``` |
| ![Ampoule](Images/Standard/Ampoule.jpg) | ```json { "form": "ampoule", "tmt": "536245", "name": "ACETIN" }``` |
| ![Vial](Images/Standard/Vial.jpg) | ```json { "form": "vial", "tmt": "1264908", "name": "ZYCORT" }``` |

---

### Advanced & Challenging Scenarios

The following examples demonstrate the system's robustness in more complex situations.

#### 1. Identification of Broken Tablets

The system can correctly identify pills even when they are damaged or incomplete.

| Input Image | System Output (JSON) |
| :--- | :--- |
| ![Broken](Images/Challenge/Broken.jpg) | ```json { "form": "tablet", "tmt": "129523", "name": "PONSTAN 500", "shape": "oval", "color": "yellow", "scoring": "none", "width": 20, "height": 10, "error": "Tablets 'Broken' are found" }``` |

#### 2. Differentiating by Size (Same Color and Shape)

The system can detect anomalies when a package contains pills that should be uniform but have different sizes.

| Input Image | System Output (JSON) |
| :--- | :--- |
| ![Size](Images/Challenge/Size.jpg) | ```json { "form": "tablet", "tmt": "235234", "name": "ALLOPURINOL", "shape": "round", "color": "white", "scoring": "single", "width": 11, "height": 11, "error": "'Width' and 'Height' different are found" }``` |

#### 3. Differentiating by Color (Same Size and Shape)

Similarly, the system can distinguish between tablets based on color.

| Input Image | System Output (JSON) |
| :--- | :--- |
| ![Size](Images/Challenge/Color.jpg) | ```json { "form": "tablet", "tmt": "907740", "name": "GPO-HCTZ", "shape": "round", "color": "yellow", "scoring": "+", "width": 8, "height": 8, "error": "'Color' different is found" }``` |

#### 4. Distinguishing Between Highly Similar Items

This showcases the fine-grained recognition capability, where the system tells apart items that look nearly identical.

| Input Image | System Output (JSON) |
| :--- | :--- |
| ![Fenac Ampoule](Images/Challenge/Fenac.jpg) | ```json { "form": "ampoule", "tmt": "120291", "name": "FENAC" }``` |
| ![Dexton Ampoule](Images/Challenge/Dexton.jpg) | ```json { "form": "ampoule", "tmt": "556126", "name": "DEXTON" }``` |
| ![Sara 120 Box](Images/Challenge/Sara120.jpg) | ```json { "form": "box", "tmt": "952263", "name": "SARA (STRAWBERRY) 120" }``` |
| ![Sara 250 Box](Images/Challenge/Sara250.jpg) | ```json { "form": "box", "tmt": "957539", "name": "SARA (STRAWBERRY) 250" }``` |