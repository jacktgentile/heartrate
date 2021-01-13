# Heart Rate Detection

Accurately detect a person's heart rate using only an input video of the person's face.

## Background

Each time your heart beats, blood rushes to your face. This increased amount of blood causes your skin to become slightly more red. The change is imperceptible to the naked eye, but computers can detect, analyze, and emphasize this change. The frequency of this intensity-shift cycle is the same as your heart rate at any given time. 

There are a number of parameters that affect the accuracy and precision of heart rate detection using this method: region of interest, movement and facial tracking, ambient noise, skin pigmentation, etc..

## How It Works

### 1. Read the input data

Specify the filepath of a video file to be processed. Read the file data into a numpy array of shape `(f, h, w, c)`, where 
```
f = number of frames
h = height of a single frame
w = width of a single frame
c = number of channels (3)
```

### 2. Select the region of interest (ROI)

Choose a center point and radius for a box ROI using the first frame as a reference. The forehead yields the best results. The skin is thinnest at that part of the face, so the intensity shift is most dramatic there.

### 3. Calculate average intensity of ROI at each frame

For each frame and each channel, calculate the average pixel intensity of the ROI. Store results in an array of shape `(f, c)`, with `f`, `c` as defined above.

### 4. Normalize intensity data

Use the sliding window method to calculate a partial mean and standard deviation for each full window of the intensity array. Normalize i-th intensity using the window centered at i.

### 5. Use FFT to calculate frequency and BPM

FFT expresses a signal as the weighted sum of frequencies. The frequency with the largest weight corresponds to the dominant frequency: heart rate. To calculate BPM, multiply the frequency (should be in range \[0, 0.5)) with the heighest corresponding weight by 60 * (frame rate in fps).
