# SFND_Radar_Target_Generation_And_Detection

[//]: # (Image References)
[image1]: ./media/2D_CACFAR_BlockDiagram.png "3D CA-CFAR"
[image2]: ./media/CFAR_2D_Output.png "CFAR Output"
[image3]: ./media/CFAR_2D_Output_2DView.png "CFAR Output - 2D View"
[image4]: ./media/FFT.png "Fast Fourier Transform"
[image5]: ./media/ProjectBlockDiagram.png "Project layout"
[image6]: ./media/RangeDopplerMap.png "Range Doppler Map"
[image7]: ./media/RangeSpectrum.png "Range Spectrum"
[image8]: ./media/SignalPropagationModel_MovingTarget.png "Signal Progagation Model"


## Project Flow Chart
![alt text][image5]

---
## 1. Radar Specifications 
* Frequency of operation = 77GHz
* Max Range = 200 m
* Range Resolution = 1 m
* Max Velocity = 100 m/s
* Light Speed = 3e8 m/s

## 2. User Defined Range and Velocity of the simulated target

Taget is initially 45m away and moving inbound (-) toward radar sensor
* Target Initial Range = 45 m
* Target Velocity = -30 m/s

## 3. FMCW Waveform Generation
The FMCW waveform design:
* Carrier Frequency = 77 GHz
* Sweep Bandwidth (B) = speed_of_light / (2 * Range_Resolution_of_Radar) = 150 MHz
* Chirp Time (Tchirp) =  (sweep_time_factor(should be at least 5 to 6) * 2 * Max_Range_of_Radar) / speed_of_light = 7.333 μs
* Slope of the FMCW chirp = B / Tchirp = 2.045e+13
* The number of chirps in one sequence (Nd) = 128
* The number of samples on each chirp (Nr) = 1024 

## 4. Signal Generation and Moving Target simulation
![alt text][image8]

## 5. Range Measurement
Applying the Fast Fourier Transform on the sampled beat signal to convert the signal from time domain to frequency domain to know the range between the target and the radar and the frequency shift.
![alt text][image4]

* Range Spectrum (1D-FFT) Result
![alt text][image7]

## 6. Range Doppler Response
Applying the 2 dimensional FFT on the beat signal to get Range Doppler Spectrum, also called as Range Doppler Map.

* Range Doppler Map Result
![alt text][image6]

## 7. CA-CFAR implementation
Applying the 2 dimensional CA-CFAR on the RDM for target detection.

### CA-CFAR algorithm:
1. Determine the number of Training cells/Guard cells on range/doppler dimension.
2. Slide the cell under test across the complete matrix. Make sure the Cell Under Test (**CUT**) has margin for Training and Guard cells from the edges.
3. For every iteration over range/doppler dimension, compute the average of the summed values for all of the training cells used. That's the estimated noise level
5. Further add the offset to it to determine the threshold.
6. Next, compare the signal under CUT against this threshold.
7. If the CUT level > threshold assign the filtered signal a value of 1.


#### Sliding Window Design:
![alt text][image1]

* CA-CFAR Result - 3D View
![alt text][image2]

* CA-CFAR Result - 2D View
![alt text][image3]