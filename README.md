🍎 Digital Image Processing for Fruit Surface Defect Detection and Quality Classification
📌 Project Overview

This project is a real-time Fruit Quality Inspection System developed using Python, OpenCV, and digital image processing techniques. It detects visible surface defects in fruits using a live webcam feed or synthetic demo images and classifies fruit quality into different categories.

The system analyzes the fruit surface to identify potential defects such as dark spots, bruises, discoloration, and green or blue-green mold-like regions. Based on the estimated damaged surface percentage and the number of detected defect regions, the fruit is classified as Fresh, Minor Defects, or Rotten.

The application provides a visual inspection interface with defect highlighting, fruit segmentation, quality status, and damaged surface percentage.

🎯 Objectives
Detect fruit surfaces from camera images.
Identify potential surface defects using image processing.
Estimate the percentage of damaged surface area.
Classify fruits based on predefined quality thresholds.
Support multiple fruit profiles with different detection settings.
Provide real-time visual feedback for fruit quality inspection.
Demonstrate fruit inspection using built-in synthetic sample images.
✨ Features
Real-Time Camera Inspection: Captures fruit images through a webcam.
Demo Mode: Displays synthetic fresh and defective fruit samples without requiring a camera.
Fruit Segmentation: Separates the fruit from the background using color differences in the LAB color space.
Defect Detection: Identifies dark spots, abnormal color changes, and potential mold-like regions.
Fruit Profile Detection: Automatically estimates the fruit type using HSV color ranges and shape information.
Quality Classification: Categorizes fruit as Fresh, Minor Defects, or Rotten.
Defect Visualization: Highlights detected defects in red and displays bounding boxes around defect regions.
Background Calibration: Allows users to calibrate the inspection area for improved background segmentation.
Sensitivity Adjustment: Lets users adjust the sensitivity of defect detection.
Visual Results: Displays the detected fruit, quality status, defect count, and damaged surface percentage.
Snapshot Export: Saves the inspection image and defect mask for later analysis.
🍊 Supported Fruit Profiles

The system includes predefined color profiles for the following fruits:

Fruit Profile	Detection Method
Red Apple / Tomato	Red hue ranges
Pomegranate	Red hue ranges with dark-skin settings
Strawberry	Red hue ranges
Orange	Orange hue ranges
Mango	Yellow-green hue ranges
Banana	Yellow hue ranges and elongated shape
Lemon	Yellow hue ranges
Guava	Green hue ranges
Green Apple	Green hue ranges
Plum / Grape	Purple hue ranges with dark-skin settings
Generic Fruit	Fallback profile

Note: Fruit profiles are selected using color and shape heuristics. The system does not use a trained deep-learning model, and automatic fruit identification may not always be accurate.

🛠️ Technologies Used
Python — Main programming language
OpenCV — Image processing, camera capture, segmentation, and visualization
NumPy — Numerical operations and image array processing
Argparse — Command-line argument handling
Dataclasses — Fruit profile configuration
Collections — History smoothing and profile voting
📂 Project Structure
fruit-defect-detection/
│
├── fruit_inspector.py       # Main application
├── README.md                # Project documentation
├── fruit_snapshot.png       # Saved inspection image (generated)
└── defect_mask.png          # Saved defect mask (generated)


The main Python file can be named fruit_inspector.py, or you can use another filename and adjust the commands below accordingly.

⚙️ Installation
1. Install Python

Install Python 3.9 or later. Python 3.10 or 3.11 is a suitable choice for this project.

2. Clone the Repository
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY


Replace the repository URL with your actual GitHub repository URL.

3. Install Dependencies

Install the required libraries using pip:

pip install opencv-python numpy


No separate dataset is required to run the built-in demo samples.

▶️ How to Run the Project
Run with a Webcam

Connect a webcam and execute:

python fruit_inspector.py


The application opens the camera feed with a centered inspection box. Place a fruit inside the box and hold it steady while the system analyzes the surface.

Run in Demo Mode

To run the application using synthetic fruit images:

python fruit_inspector.py --demo


Demo mode includes predefined fresh and defective examples of apples, oranges, bananas, tomatoes, guavas, mangoes, and plums.

Press n to switch to the next sample.

Select a Camera

If your computer has multiple cameras, specify the camera index:

python fruit_inspector.py --cam 1


The default camera index is 0. If the camera cannot be opened, the application automatically switches to demo mode.

🎮 Keyboard Controls
Key	Function
q	Quit the application
n	Display the next demo sample
d	Toggle demo mode
f	Cycle through manual fruit profiles and automatic detection
b	Calibrate the background using an empty inspection box
c	Clear background calibration
+ or =	Increase defect detection sensitivity
- or _	Decrease defect detection sensitivity
s	Save the current inspection image and defect mask
Sensitivity Adjustment
Increasing sensitivity makes the detection thresholds more responsive, potentially detecting more defects.
Decreasing sensitivity makes the thresholds less responsive, potentially reducing false detections.

The sensitivity value is limited to the range 0.5 to 2.0.

🔬 Image Processing Workflow

The system follows these primary processing stages:

Image Acquisition: Capture an image from the webcam or generate a demo sample.
Region of Interest (ROI): Extract the centered 300 × 300 pixel inspection area.
Fruit Segmentation: Use LAB color differences, thresholding, and morphological operations to separate the fruit from the background.
White Balance Correction: Adjust LAB color channels when the background is sufficiently neutral.
Fruit Profile Selection: Estimate the fruit profile using HSV hue ranges, brightness, and shape characteristics, or use a manually selected profile.
Valid Surface Extraction: Exclude the outer edge of the fruit and bright glare regions from defect analysis.
Defect Detection: Combine local darkness, absolute darkness, color deviations, and potential mold-like color detection.
Noise Removal: Apply morphological operations and remove small defect regions and some stem-like regions.
Defect Measurement: Calculate the detected defect pixel percentage and count defect regions.
Quality Classification: Smooth measurements across multiple frames and assign a quality category.
Visualization and Export: Display the results and optionally save the inspection image and defect mask.
📊 Quality Classification

The application uses predefined defect-percentage thresholds for each fruit profile.

Classification	Description
ANALYZING	Collecting enough consecutive measurements
FRESH	Detected damage is below the warning threshold
MINOR	Detected damage reaches the warning threshold but not the rejection threshold
ROTTEN	Detected damage reaches the rejection threshold, or the combined defect conditions trigger rejection
EMPTY	No suitable fruit region or valid surface analysis is detected
TOO_BIG	The detected fruit occupies more than 85% of the inspection box

The system uses the median of up to eight recent defect-percentage measurements and defect counts to reduce fluctuations between frames. It waits for at least four measurements before assigning a normal quality category.

Important: The reported damaged surface percentage is calculated relative to the valid analysis region, not necessarily the entire physical fruit surface. The results are heuristic estimates and should not be treated as scientifically validated grading or food-safety determinations.

🖼️ Output and Visualization

The application opens two windows:

1. Fruit Quality Inspector

Displays the camera or demo image, fruit boundary, detected defect regions, quality status, selected fruit profile, defect count, and sensitivity setting.

The inspection box border indicates the current state:

🟢 Green — Fresh
🟡 Yellow — Minor defects
🔴 Red — Rotten
🔵 Cyan — Analyzing
🟠 Orange — Fruit too large
⚪ White — Empty inspection area
2. Defect Mask

Displays the segmented fruit and detected defect regions. White pixels represent detected damage, while gray pixels represent the segmented fruit.

Saving Results

Press s to save the current results:

fruit_snapshot.png — Annotated inspection image
defect_mask.png — Defect visualization mask

Files are saved in the current working directory and may be overwritten when saved again.

💡 Recommended Inspection Conditions

For more consistent results:

Use a plain, light-colored background.
Ensure the fruit fits inside the inspection box.
Provide even lighting from the front or sides.
Avoid strong reflections and direct glare.
Keep the camera steady during inspection.
Calibrate the background with the inspection box empty when needed.
Use the appropriate manual fruit profile if automatic selection is incorrect.

The program displays a lighting warning when the measured brightness along the top edge of the inspection box is too low or too high.

⚠️ Limitations
The system uses traditional image-processing heuristics rather than a trained machine-learning or deep-learning model.
Fruit profile selection is based primarily on color and shape, so visually similar fruits may be confused.
Lighting, camera quality, background color, shadows, and reflections can affect results.
Natural markings, stems, and variations in fruit skin may be misidentified as defects.
Mold detection is based on color ranges and does not confirm the presence of actual mold.
The synthetic demo samples are intended for testing the interface and processing pipeline, not for evaluating real-world accuracy.
Classification thresholds are predefined and may require calibration for different fruits and inspection conditions.
🚀 Future Enhancements
Integrate a trained CNN or YOLO model for more robust fruit and defect detection.
Add dataset-based model training and validation.
Support image upload in addition to webcam input.
Generate detailed inspection reports with timestamps.
Add a graphical interface using Streamlit, Flask, or Tkinter.
Store inspection results in a database.
Evaluate precision, recall, F1-score, and classification accuracy using labeled test images.
Extend the system to fruit grading based on size, color, and defect severity.
👨‍💻 Author

Your Name

GitHub: YOUR-USERNAME

📄 License

This project is available for educational and research purposes. Add a LICENSE file to your repository if you want to specify the terms under which others may use, modify, and distribute the code.
