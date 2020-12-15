[![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/course/sensor-fusion-engineer-nanodegree--nd313)

# Udacity Nanodegree: Sensor Fusion

## Project 04: Radar target generation and detection

This project aims to develop a software stack that will achieve the following objectives.

```
* Range detection 
* Velocity detection
* Detection of Angle of Approach
* Constant false alarm rate (CFAR) noise suppression
```

Project pipeline :  
<img src="./docs/project_layout.png" width="779" height="414" />


### Dependencies for Running Locally
* Matlab
    - Automated Driving Toolbox
    - Signal Processing Toolbox
    - DSP System Toolbox

### Project Rubric

#### Task.1 FMCW Waveform Design
 
* FMCW Radar characteristics

   > We already have the Radar requirement specifications which are as follows
```
Frequency of operation = 77GHz
Max Range              = 200m
Range Resolution       = 1 m
Max Velocity           = 100 m/s
Speed of light         = 3*10^8 m/s
Sweep time factor      = 5.5;
``` 
   > We know that the Bandwidth of the chirp is dependent only on the Range resolution. And its mainly because the signal in frequency domain can only be distinctly identified if we have enough frequency difference and time of observation. Similarly the chirp time is only dependent on the Sweep time factor. We need to have sweep time factor more than two in order to avoid the interference of signals. To get the slope of chirp we just need to get the ratio of prior quantities.

* Bandwidth   = speed of light / (2 * Range Resolution)        = 150 *10^6 Hz
* Chirp time  = (sweep time factor*2*Max Range)/speed of light = 7.33 *10^-6 sec
* slope       = Bandwidth / chirp time                         = 2.0455 * 10^13

#### Task.2 Range FFT (1st FFT)
Implement the Range FFT on the Beat or Mixed Signal and plot the result.
   > The FMCW radar uses the linear increase in frequency to detect the range. To make it bit more simple we know that the received signal is nothing but replica of transmitted signal with some delay. As you can see the following block diagram the mixer have access to both transmitted and received signal and to get the beat frequency it substract the received signal. Every object will have its corresponding beat frquency based on its range. To get the beat frequency associated with the signal we need to convert the signal from time domain to frequency domain and in frequency domain each peak will corrsponds to object.
<img src="./docs/fmcw_radar.png" width="779" height="414" />


#### Task.3 Doppler FFT (2nd FFT)
Implement the Doppler FFT based on the Range FFT readings.
   > We know that if the source and the observer have a non-zero relative velocity the frequency observed will have some shift and generally it is used for determining the velocity. However in case of the FMCW Radar the we calculate the beat frequency which is predominatly dominated by the range of object. To get the velocity estimation from the FMCW radar we focus on the phase of the received signal. With the FMCW radar we send multiple chirps in one sweep. For every chirp the range fft will look identical and if we calculate the change of phase difference over each chirp we can get the velocity.  

#### Task.4 2D CFAR
Implement the 2D CFAR process on the output of 2D FFT operation, i.e the Range Doppler Map.

   > To supress the Noise and to avoid the false positives we implemented the ```CA-CFAR(Cell Averaging Constant False Alarm Rate)``` technique. In this technique we have a 2d mask which contains the guard cell(which avoids the signal supression) and training cells. We slide this mask over entire image and replace the value at a particular cell by avarage of training cells. This technique works cause we assume noise is spatially and temporally same. The offset is a very important parameters added to the threshold estimate to avoids the false positives. However large offset factor might lead to supression of valid signal. The cells at the boundary of image would remain non-thresholded in the previous step. Hence, these cells are suppressed to 0 after implementing 2D CFAR.
<img src="./docs/ca-cfar.png" width="779" height="414" />

### References

[FMCW leature series](https://www.youtube.com/playlist?list=PLJAlx-5DOdeMNjpg4sRO6cty3gL_PZeCE)

