# Heart Rate Extraction from ECG Data

A competitive project implementing heart rate extraction from electrocardiogram (ECG) signals using digital signal processing techniques.

## Project Overview

This project implements an automated heart rate (HR) extraction algorithm from ECG signals, competed in the "Analytics for Health Wearables" course held on University Of Turku. The solution leverages the Pan-Tompkins pipeline, a well-established algorithm in the field of cardiac signal processing, with custom optimizations for improved accuracy.

## Team

| Name | Email | Affiliation |
|------|-------|------|
| Aarni Kivelä | aarni.i.kivela@utu.fi | University of Turku |
| Jaakko Nurminen | jaakko.j.nurminen@utu.fi | University of Turku |
| Oliver Nielikäinen | oliver.e.nielikainen@utu.fi | University of Turku |

## Objectives

1. Extract accurate heart rate values from raw ECG signals
2. Implement a robust signal processing pipeline that handles noisy physiological data
3. Minimize Mean Absolute Error (MAE) against ground truth HR labels
4. Identify and filter out cardiac signal artifacts and outliers

## Methodology

The implementation follows the Pan-Tompkins algorithm with the following signal processing pipeline:

### 1. **Preprocessing**
   - **Bandpass Filtering**: Applied a 4th-order Butterworth bandpass filter (0.5—15 Hz) to isolate QRS complex frequencies
   - **Derivative Filtering**: Used a 5-point stencil derivative filter to emphasize QRS slopes

### 2. **Feature Extraction**
   - **Squaring**: Squared the derivative to amplify QRS components
   - **Moving Average**: Applied a 30-sample moving average filter for smoothing

### 3. **Peak Detection**
   - Detected peaks in the filtered signal using dynamic thresholds based on signal statistics
   - Applied distance-based constraints to avoid detecting multiple peaks per heartbeat

### 4. **Peak Refinement**
   - Mapped detected peaks back to the original ECG signal to find local maxima (±150 ms window)

### 5. **Heart Rate Estimation**
   - Calculated inter-beat intervals (RR intervals) from detected peaks
   - Applied range filtering (0.33—1.5 seconds) to reject unrealistic intervals
   - Used statistical outlier rejection: filtered intervals beyond ±2 standard deviations from the median

## Results

**Final Performance**: Mean Absolute Error (MAE) = **5.44 bpm**

### Key Insight

Significant improvement was achieved through statistical outlier rejection in the interval filtering phase. This approach filters out signal artifacts by treating intervals outside the 95% confidence interval as outliers. This single optimization substantially reduced the MAE compared to basic peak counting methods.

## Requirements

- Python 3.7+
- NumPy
- SciPy
- Matplotlib
- Seaborn

## Installation

```bash
# Clone the repository
git clone <repository-url>
cd Competition

# Install dependencies
pip install numpy scipy matplotlib seaborn
```

## Usage

Run the Jupyter notebook to execute the complete analysis:

```bash
jupyter notebook competition_template.ipynb
```

The notebook is organized into three main sections:
1. **Data Loading**: Loads ECG signals and ground truth HR labels
2. **Algorithm Implementation**: Implements the heart rate extraction pipeline
3. **Evaluation**: Computes MAE and visualizes results

## Data Format

The project uses ECG signals stored in a pickle file (`data/ecg_data_with_hr_labels.pkl`) with the following structure:

```python
{
    "signals": [array of ECG signals (200 samples × variable length)],
    "hr_values": [list of ground truth HR values (200 values)]
}
```

**Sampling Rate**: 200 Hz

## Key Technical Details

- **Nyquist Frequency**: 100 Hz
- **Bandpass Frequency Range**: 0.5—15 Hz (captures QRS complex)
- **Moving Average Window**: 30 samples
- **Peak Detection Distance**: 60 samples (0.3 seconds)

## Visualizations

The notebook includes visualizations for:
- Raw ECG signal inspection
- Frequency domain analysis
- Detected peaks overlaid on ECG signals
- Comparison of estimated vs. ground truth HR values

## References

- Pan, J., & Tompkins, W. J. (1985). A real-time QRS detection algorithm. IEEE Transactions on Biomedical Engineering, BME-32(3), 230-236.
- [Pan-Tompkins Algorithm Overview](https://medium.com/@cosmicwanderer/pan-tompkins-algorithm-for-detecting-qrs-waves-29c5f2927906)
- [Statistical Confidence Intervals](https://www.bmj.com/about-bmj/resources-readers/publications/statistics-square-one/4-statements-probability-and-confiden)

## Lessons Learned

The project demonstrates the importance of statistical methods in physiological signal processing:
- **Outlier rejection** using standard deviation-based thresholding significantly improved robustness
- **Parameter tuning** (window sizes, frequency ranges) is critical for ECG processing
- **Domain knowledge** of cardiac physiology informed algorithm design decisions

## License

This project was completed as part of the University of Turku "Analytics for Health Wearables" course.
