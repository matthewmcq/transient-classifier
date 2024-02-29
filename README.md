## Preprocessing:

- Audio files are loaded in with librosa.load() (librosa is the most popular audio/signal processing python library).
- We then time stretch the signal by a factor of 3/4 to allow for better transient detection of quick subsequent drums.
- We use the Constant-Q Transform to define an onset envelope for the audio signal and  plug that into librosa.onset_detect to detect transients with greater accuracy than just using the unaltered signal amplitudes.
- Once the transients have been located, we record the onset frames and then convert their frame number to the corresponding time in seconds at which the transient occurs. 
- We then use the last frame in onset_frames to denote offet_frames (i.e. the end of the transient), where the space between onset and offset frames would yield the time between transient events (what we care about for this research).

## Adding Feature Representations to each Transient:

- For each transient, we compute the wavelet transform and the Mel-frequency cepstral coefficients. This allows us to group transients based on their spectral composition.
- We then normalize the wavelet coefficients with zero padding, as some transients encode more frequency information than others.
Finally, we concatenate the MFCC and wavelet coefficients to create an aggregated feature representation for each transient.

## KMeans Clustering:

- Once we have the spectral features, we use the scikit-learn StandardScalar() object to allow for KMeans clustering to work with the two different features (essentially, this normalizes the features so that MFCC and wavelet coefficients are “worth” the same amount during clustering).
- We then apply Principal Component Analysis to aid in KMeans clustering.
- From here, we compute the KMeans clusters from min_cluster to max_cluster number of unique labels/clusters. This allows for better generalization between audio files and eliminates the need to manually count the number of unique sounds, which is generally not feasible or efficient.
- We then compare the silhouette scores of each clustering to identify which number of KMeans clusters most accurately corresponds to the number of distinct transients in the audio signal.
- Finally, we graph the cluster distributions and replot the waveform with each transient colored according to its KMeans cluster.
For this example, the yellow transients correspond to the woodpecker drums

## Postprocessing:

- We then locate the cluster that corresponds to the drum (with two clusters this would just be the second label to occur) and record all onset_times corresponding to that label along with the time between onsets
- Finally we determine the centroid of each cluster and write that to an mp3 for qualitative assessment of the classifiers accuracy for what a cluster is