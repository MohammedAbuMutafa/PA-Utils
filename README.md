# PA-Utils

A collection of utility scripts and tools for the Precision Agriculture project, providing image processing, computer vision, and machine learning utilities for agricultural data analysis and model training.

## Overview

PA-Utils contains essential utility functions and scripts that support the broader Precision Agriculture ecosystem. These tools handle image preprocessing, dataset preparation, threshold optimization, and various computer vision approaches for agricultural image analysis.

## Features

### Image Processing Utilities
- **HSV Threshold Optimization**: Interactive threshold tuning for color-based segmentation
- **Image Classification**: Computer vision approaches for crop health analysis
- **Dataset Preparation**: Tools for preparing training data for machine learning models
- **Label Generation**: Automated label creation for YOLO and other object detection models

### Computer Vision Tools
- **OpenCV Integration**: Comprehensive OpenCV-based image processing
- **Color Space Conversion**: HSV, RGB, and other color space transformations
- **Image Augmentation**: Data augmentation techniques for training datasets
- **Threshold Analysis**: Automated threshold detection and optimization

### Machine Learning Support
- **YOLO Dataset Preparation**: Convert CSV labels to YOLO format
- **Model Testing**: Accuracy testing and validation utilities
- **Data Visualization**: Tools for visualizing processing results
- **Performance Metrics**: Evaluation tools for model performance assessment

## Project Structure

```
PA-Utils/
├── apply_on_images.py          # Main image processing application
├── create_yolo_labels.py       # YOLO dataset label conversion
├── createOriginalLabels.py     # Original label generation
├── draw_threshold.py           # Threshold visualization
├── ImageProcessingApproach.py  # Core image processing classifier
├── test_cv_approach.py         # Computer vision testing utilities
├── threshhold.py              # HSV threshold optimization tool
├── containers/                 # Docker container configurations
│   └── docker-compose.yml      # Container orchestration
└── README.md                  # This file
```

## Tools and Scripts

### 1. HSV Threshold Optimizer (`threshhold.py`)
Interactive tool for optimizing HSV color thresholds for crop segmentation.

**Features:**
- Real-time threshold adjustment with trackbars
- Camera or image file input support
- HSV color space optimization
- Visual feedback for threshold effects

**Usage:**
```bash
# Use with camera
python threshhold.py

# Use with image file
python threshhold.py path/to/image.jpg
```

### 2. YOLO Label Creator (`create_yolo_labels.py`)
Converts CSV label format to YOLO annotation format for object detection training.

**Features:**
- CSV to YOLO format conversion
- Automatic image dimension detection
- Class mapping for stressed/healthy crops
- Batch processing for multiple images

**Usage:**
```bash
python create_yolo_labels.py
```

### 3. Image Processing Classifier (`ImageProcessingApproach.py`)
Core computer vision classifier for crop health analysis.

**Features:**
- NDVI-based classification
- Threshold-based segmentation
- Healthy/unhealthy plant detection
- Numpy array processing

**Usage:**
```python
from ImageProcessingApproach import ImageProcessingClassifier

classifier = ImageProcessingClassifier(threshold_low=0.35, threshold_high=0.6, numpy_dir="path/to/numpy")
result = classifier.process_image("image.npy")
```

### 4. Accuracy Testing (`test_cv_approach.py`)
Comprehensive testing framework for computer vision approaches.

**Features:**
- Model accuracy evaluation
- Confusion matrix generation
- Performance metrics calculation
- Batch testing capabilities

**Usage:**
```bash
python test_cv_approach.py
```

### 5. Threshold Visualization (`draw_threshold.py`)
Visualization tool for threshold-based segmentation results.

**Features:**
- Threshold overlay visualization
- Before/after comparison
- Export capabilities for analysis
- Custom threshold adjustment

### 6. Main Processing Application (`apply_on_images.py`)
Main application for processing agricultural images with various approaches.

**Features:**
- Multiple processing approaches
- Batch image processing
- Results visualization
- Performance tracking

## Installation and Setup

### Prerequisites
- Python 3.8 or higher
- OpenCV (cv2)
- NumPy
- Pandas
- PIL/Pillow
- Matplotlib

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/MohammedAbuMutafa/PA-Utils.git
   cd PA-Utils
   ```

2. **Create virtual environment**:
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install opencv-python numpy pandas pillow matplotlib
   ```

### Docker Setup

1. **Build and run with Docker Compose**:
   ```bash
   docker-compose up -d
   ```

2. **Access the container**:
   ```bash
   docker exec -it pa-utils-container bash
   ```

## Usage Examples

### HSV Threshold Optimization

```python
# Interactive threshold tuning
python threshhold.py input_image.jpg

# Adjust trackbars in the GUI to find optimal thresholds
# Values will be displayed in the console
```

### YOLO Dataset Preparation

```python
# Convert CSV labels to YOLO format
python create_yolo_labels.py

# Ensure CSV file has columns: filename, class, x1, y1, x2, y2
# Output will be in YOLO format: class x_center y_center width height
```

### Image Classification

```python
from ImageProcessingApproach import ImageProcessingClassifier

# Initialize classifier with thresholds
classifier = ImageProcessingClassifier(
    threshold_low=0.35,
    threshold_high=0.6,
    numpy_dir="path/to/numpy/data"
)

# Process an image
result = classifier.process_image("image.npy")
print(f"Healthy: {result['healthy_percent']}%, Unhealthy: {result['unhealthy_percent']}%")
```

### Accuracy Testing

```python
from test_cv_approach import AccuracyTester

# Initialize tester with ground truth labels
tester = AccuracyTester(
    labels_path="path/to/labels.csv",
    imageClassifier=classifier,
    processed_folder="path/to/processed",
    rgb_folder="path/to/rgb"
)

# Test on specific image
tester.process_image("Image_048.jpg")
```

## Configuration

### Threshold Parameters
```python
# HSV Threshold ranges for crop segmentation
HSV_LOW = [35, 50, 50]    # Lower bound for healthy crops
HSV_HIGH = [85, 255, 255] # Upper bound for healthy crops

# NDVI Thresholds
NDVI_LOW = 0.35   # Lower threshold for vegetation
NDVI_HIGH = 0.6   # Upper threshold for healthy vegetation
```

### File Paths
```python
# Update paths in scripts as needed
BASE_DIR = "path/to/your/data"
RGB_DIR = "path/to/rgb/images"
LABELS_DIR = "path/to/labels.csv"
OUTPUT_DIR = "path/to/output"
```

## Integration

### With PA Components
- **PA-MultispectralProcessing**: Utilizes classification results and thresholds
- **PA-Backend**: Provides processed data and metrics
- **PA-Web**: Visualizes processing results and thresholds
- **PA-Components**: Shares utility functions and classifiers

### External Tools
- **YOLO**: Dataset preparation for object detection training
- **OpenCV**: Core computer vision operations
- **NumPy**: Numerical computations and array operations
- **Pandas**: Data manipulation and CSV processing

## Advanced Features

### Custom Threshold Optimization
```python
# Automated threshold optimization
def optimize_thresholds(image_path, target_accuracy=0.85):
    best_thresholds = None
    best_accuracy = 0
    
    for low in range(20, 50, 5):
        for high in range(60, 90, 5):
            classifier = ImageProcessingClassifier(low/100, high/100)
            accuracy = classifier.evaluate_accuracy(image_path)
            
            if accuracy > best_accuracy:
                best_accuracy = accuracy
                best_thresholds = (low/100, high/100)
    
    return best_thresholds, best_accuracy
```

### Batch Processing
```python
# Process multiple images
import os
from apply_on_images import process_image_batch

input_dir = "path/to/input/images"
output_dir = "path/to/output/results"

process_image_batch(input_dir, output_dir, classifier)
```

## Performance Optimization

### Memory Management
- Use numpy arrays for efficient image processing
- Implement batch processing for large datasets
- Clear unused variables and arrays

### Processing Speed
- Utilize OpenCV's optimized operations
- Implement parallel processing for batch operations
- Cache frequently used classifiers and models

## Troubleshooting

### Common Issues

1. **OpenCV Installation Issues**:
   ```bash
   pip uninstall opencv-python
   pip install opencv-python-headless
   ```

2. **Memory Issues with Large Images**:
   - Process images in smaller batches
   - Use image resizing before processing
   - Implement memory-efficient numpy operations

3. **Threshold Optimization Problems**:
   - Ensure proper lighting conditions in source images
   - Check color space conversions
   - Verify HSV value ranges (0-179 for Hue, 0-255 for Saturation/Value)

### Debug Mode
Enable debug logging by setting environment variable:
```bash
export DEBUG_MODE=TRUE
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Add your utility functions
4. Include tests and documentation
5. Submit a pull request

### Adding New Utilities

1. **Create new script**:
   ```python
   # new_utility.py
   import cv2
   import numpy as np
   
   def new_utility_function(input_data):
       # Implementation
       return result
   ```

2. **Add to main application**:
   ```python
   # Import and integrate in apply_on_images.py
   from new_utility import new_utility_function
   ```

3. **Update documentation**:
   - Add usage examples
   - Document parameters and return values
   - Include integration notes

## License

This project is part of the Precision Agriculture ecosystem and is developed for research and educational purposes.

## Support

For issues and questions:
- Create an issue in the repository
- Check the utility function documentation
- Review the integration examples

---

*Part of the Precision Agriculture ecosystem - Supporting smart farming through utility tools*
