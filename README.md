# ECG Signal Denoising and Quality Enhancement Using Digital FIR Filtering and a 1D CNN Refiner

# 1. Introduction

An Electrocardiogram (ECG) records the electrical activity of the heart. However, ECG signals are often contaminated by different types of noise, which can affect their quality and interpretation.


This project proposes a hybrid ECG denoising technique using Finite Impulse Response (FIR) filters followed by a One-Dimensional Convolutional Neural Network (1D CNN). Initially, FIR filters are applied to remove unwanted frequency components. The filtered signal is then passed through a CNN to remove residual noise and improve the quality of the ECG signal while preserving important features such as P waves, QRS complexes and T waves.

# 2. Objectives


- Obtain clean ECG signals from a standard ECG dataset.
- Introduce different types of noise into clean ECG signals.
- Design FIR filters using different window functions and filter orders.
- Remove baseline wander and unwanted frequency components using filtering.
- Apply a 1D CNN to the FIR-filtered signal for further noise removal.
- Evaluate the final output using SNR, RMSE and correlation.
- Examine the preservation of R-peaks and QRS complexes.

# 3. Dataset

The project uses the MIT-BIH Arrhythmia Database . Record 100, channel 0, is used as the reference ECG signal. A 10-second segment is selected for testing.

| Parameter | Value |
|---|---|
| Dataset | MIT-BIH Arrhythmia Database |
| Record Number | 100 |
| Channel | 0 |
| Sampling Frequency | 360 Hz |
| Sampling Interval | 2.778 ms |
| Test Duration | 10 seconds |
| Number of Samples | 3600 |
| Normalized Amplitude | -1 to +1 |

## 4. Noise Types and Parameters

Different types of noise are introduced into the clean ECG signal to generate a noisy ECG signal for testing the denoising system.

| Noise Type | Frequency / Range | Amplitude |
|---|---|---|
| Baseline Wander 1 | 0.25 Hz | 0.15 |
| Baseline Wander 2 | 0.35 Hz | 0.04 |
| Power-Line Interference | 50 Hz | 0.08 |
| EMG Noise | 40–100 Hz | 0.08 RMS |
| AWGN | Target SNR: 10 dB | Calculated based on signal power |

## 5. FIR Filter Design

In the first stage, FIR filters are designed and applied to the noisy ECG signal to remove unwanted frequency components.

A 0.5 Hz Butterworth high-pass filter is used for baseline wander removal, followed by a 40 Hz windowed-sinc FIR low-pass filter to attenuate high-frequency noise and power-line interference.

Different window functions and filter orders are tested to study their effects on denoising performance.

The window functions considered are Hamming, Hann and Blackman. FIR orders of 20, 40, 60, 80 and 100 are evaluated. The main configuration uses an order-100 Hamming window FIR filter.

## 6. CNN-Based Denoising

After FIR filtering, the filtered ECG signal is passed to a One-Dimensional Convolutional Neural Network (1D CNN).

The CNN is trained using pairs of FIR-filtered noisy ECG signals and their corresponding clean ECG signals. It learns the characteristics of clean ECG waveforms and estimates the remaining noise.

The network uses six dilated Conv1D layers with residual learning. The CNN predicts a correction that is added to the FIR-filtered input to produce the final denoised ECG signal.

## 7. Proposed Methodology

The complete process follows these steps:

1. Obtain clean ECG signals from the MIT-BIH Arrhythmia Database.
2. Add different types of noise to generate noisy ECG signals.
3. Apply a Butterworth high-pass filter for baseline removal.
4. Apply a windowed-sinc FIR low-pass filter for initial noise removal.
5. Use the FIR-filtered signals as inputs to the CNN.
6. Train the CNN using corresponding clean ECG signals as target outputs.
7. Apply the trained CNN to the FIR-filtered test signal.
8. Evaluate the final denoised signal using performance metrics.

## The overall processing flow is:

Clean ECG Signal + Noise  
↓  
Noisy ECG Signal  
↓  
Baseline Removal  
↓  
FIR Low-Pass Filtering  
↓  
FIR-Filtered ECG Signal  
↓  
1D CNN Refinement  
↓  
Final Denoised ECG Signal  
↓  
Performance Evaluation

## 8. Performance Evaluation
   The system is evaluated using the following metrics:

- Signal-to-Noise Ratio (SNR): Measures the signal strength relative to noise. Higher SNR indicates better denoising.
- Mean Squared Error (MSE): Measures the average squared difference between the clean and denoised signals.
- Root Mean Squared Error (RMSE): Measures the reconstruction error. Lower RMSE is better.
- Correlation Coefficient: Measures similarity between the clean reference ECG and denoised ECG.
- R-peak Detection: Evaluates whether important ECG peaks are preserved.

## 9. Results


| Signal | SNR (dB) | RMSE | Correlation | TP | FP | FN | F1 Score |
|---|---:|---:|---:|---:|---:|---:|---:|
| Noisy ECG | -1.694 | 0.1616 | 0.6578 | 8 | 15 | 5 | 0.444 |
| FIR-Filtered ECG | 9.141 | 0.0464 | 0.9395 | 13 | 0 | 0 | 1.000 |
| FIR + CNN | 14.078 | 0.0263 | 0.9806 | 13 | 0 | 0 | 1.000 |
## 10. Tools and Technologies



## 11. Conclusion

This project develops a hybrid ECG denoising approach in which FIR filters perform initial noise removal and a 1D CNN further processes the filtered signal to remove residual noise.

The FIR stage attenuates unwanted frequency components, while the CNN learns ECG waveform characteristics and improves the reconstruction of the clean signal.

The experimental results show that the combined FIR and CNN approach improves SNR, reduces RMSE and increases correlation while preserving important ECG features.

## 12. Future Work

Future work can include testing on multiple ECG records, evaluating real-world noise, improving CNN generalization and exploring real-time implementation.
