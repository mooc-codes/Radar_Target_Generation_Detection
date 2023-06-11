### FMCW Waveform Design

- The sweep bandwidth was calculated based on the required range resolution.
- The sweep time (Tchirp) was calculated based on the maximum range required for the radar.
- The slope of the chirp was computed using sweep bandwidth and sweep time.

### Simulation Loop

- Initial target position is 110 m and relative velocity is -20 m/s
- The loop was successfully implemented that simulates target movement, transmit and receive signal and the corresponding beat signal.

- The transmit, recieve and beat signal for a single chrip with 1024 samples.

![image](https://github.com/mooc-codes/Radar_Target_Generation_Detection/assets/75176247/15458efa-870e-4faf-8186-2096b9375bb4)


### Range FFT

- Running the 1D range FFT on the Mix signal gives us the expected range within tolerance 0f +- 10 m

![image](https://github.com/mooc-codes/Radar_Target_Generation_Detection/assets/75176247/824f0b4c-4ee8-4a2d-a2ee-65c61b4594d4)


### Range - Doppler FFT

- Running the 2D FFT , gives the the range and velocity estimation.

![image](https://github.com/mooc-codes/Radar_Target_Generation_Detection/assets/75176247/dccbd726-b0e6-4b02-b051-280eccaf041a)

### 2D CFAR

- Running cfar on the output of the 2D FFT eliminates noise and gives the target signal with expected velocity.

![image](https://github.com/mooc-codes/Radar_Target_Generation_Detection/assets/75176247/3a004d30-096d-45cd-bd8f-9ca95024877f)


- The number of guard cells were selected as 2, so that the signal does not leak into the training cells but also does not isolate the signal too much.
- The number of training cells were selected as 7 in each direction since the relative levels of noise are low in the simulated signal and hence does not need a wider band to estimate the noise level.
- The offset was selected as 5 since the target signal strengh is much higher compared to the noise levels in our example.
- The signal levels at the edges were suppressed by using a zero initialized matrix to store the result of CFAR.

- Steps for CFAR
  - For each Cell Under Test (CUT) , find the neighbouring average noise level (Watts).
  - Compute a threshold which is the sum of the average noise level + offset (Watts).
  - Find the value of the cell corresponding to CUT in the output of 2D FFT in Watts.
  - Compare to threshold , if the value is greater than threshold , we set the corresponding cell in CFAR signal matrix to 1 else 0.
 
