# transient-classifier
Uses Constant Q Transform assisted transient detection along with KMeans clustering with silhouette score to identify different types of transients in an audio file based off Mel Spectrogram analysis and Wavelet features


## Control Flow:

1. Audio file is read in via librosa (defauled to mono) and the CQT is applied to better isolate transients.
2. Detect tranients with librosa.onset_detect -- play around with the args to better tailor the transient detection to the specific audio
3. Extract the Mel-frequency cepstral coefficients (MFCC) and Wavelet features from each transient
4. Interatively apply KMeans clustering based on the MFCC and Wavelet features using sillhouette scores to identify the best number of clusters.
5. Alternatively, use t-SNE for alternate visualization.
