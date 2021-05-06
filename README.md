# Pulse Rate Estimation on Wrist Wearable Device

**Introduction:**

Heart rate estimation from wearable device during activities has always been challenging. The sensor typically used to detect pulse rate is called PGG (Photoplethysmogram). During activities such as walking and running, the signals can be interfered. The purpose of this project is to develop an algorithm to estimate heart rates during running from the PGG signals.

##### **Step 1:** Let’s look at some signal data from some of the subjects

- The first first rows are raw pgg signals
- The second rows are pgg signals after Fourier transform
- The third rows are accelerometer (x, y and z axis) signals after Fourier transform

![subject 1](https://peihua-erika.com/wp-content/uploads/2021/05/output_3_0.png)

![subject 2](https://peihua-erika.com/wp-content/uploads/2021/05/output_3_1.png)

![subject 3](https://peihua-erika.com/wp-content/uploads/2021/05/output_3_2.png)



![subject 4](https://peihua-erika.com/wp-content/uploads/2021/05/output_3_3.png)

In this experiment, the subjects wore a wrist device which had pgg sensor and accelerometer inside. From the diagrams above, we can see that some of the dominant frequencies in pgg signals may be motion artifacts.

##### **Step 2:** Remove Motion Artifacts

Heart rate estimation is given every two seconds, using a window length of 8 seconds. This is what each window of the signal data goes through –

1. Bandpass filter: First, we want to remove frequencies that are unlikely to be the true heart rate. You are unlikely to see heart rate to be above 240bpm or below 40bpm. So I use a bandpass filter to remove frequencies that are below 40/60.o Hz and above 240/60.0 Hz
2. Fourier transform: use the np.fft module to apply Fourier transform on the signals (pgg and 3-axis accelerometer)
3. Compare to the accelerometer frequencies: Pick the strongest PGG frequencies that are not present in the accelerometer frequencies to exclude possible motion artifacts.
4. Calculate estimation confidence: I set confidence frequency window to +/- 3Hz from the estimated heart rate frequency. And then calculate the ratio of frequency magnitudes that fall within that window. The higher the ratio, the higher the confidence about the estimation.
5. Calculate errors: Record the difference between the estimated heart rate and the ground truth which was measured by simultaneously recorded ECG.

##### **Step 3:** Algorithm Performance

The overall algorithm performance is evaluated by the mean absolute error values (MAE) at 90% availability. First, find the 10th percentile confidence of all estimates. And then calculate the errors of the pulse rate estimates where the confidence values are above the 10th percentile. We then get the mean absolute error of the best 90% pulse rate estimates. This algorithm reached 5.10 MAE on this dataset!

(Learn more from the source code by clicking the github icon above)

Reference:

The dataset used in this project is the Troika dataset: *Z. Zhang, Z. Pi, B. Liu, TROIKA: A general framework for heart rate monitoring using wrist-type photoplethysmographic signals during intensive physical exercise, IEEE Transactions on Biomedical Engineering, vol. 62, no. 2, pp. 522-531, February 2015, DOI: 10.1109/TBME.2014.2359372*
