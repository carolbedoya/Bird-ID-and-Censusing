# Bird-ID-and-Censusing

Hola!

This folder contains the algorithms needed to replicate the results of our manuscript entitled "Acoustic censusing and individual identification of birds in the wild" 
by CL Bedoya and LE Molles (https://www.biorxiv.org/content/10.1101/2021.10.29.466450v1). A dataset with Kiwi (roroa) calls related to these codes is available at: https://doi.org/10.6084/m9.figshare.16850542.v1

 Every single piece of code mentioned in this ReadMe is also internally documented.
**********************************************************************************************************************************************************************
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
DATA PRE-PROCESSING

1) 'Divide_training_validation_GH.m' randomly divides the data (i.e., segmented calls) into training and validation datasets. We used 70% (training) and 30% (validation) 
in our manuscript, but you can change the parameters and use any other proportion. 

2) 'Data_Augmentation_GH.m' augments the data with background noise. We have provided a dataset with 6410 noise recordings from six sites to perform the data augmentation (see link above). 
The algorithm mixes each call with a randomly selected noise recording from each of these sites. The animal call and noise signal must have the same sampling frequency and length.

In our example dataset, we have also provided our acoustic data already divided into training (augmented) and validation; in case you want to skip the previous two steps.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
NOISE REDUCTION

Noise reduction is an important topic in Bioacoustics and Ecoacoustics; therefore, we have provided an isolated example of our noise reduction approach, in case you want to use it for a different application. 
 
3) 'medianequ.m' is a function containing the dynamics processor (i.e., median equalizer) disclosed in the manuscript. It performs the bulk of the noise-reduction in the spectrogram. 
Note that the bilinear interpolation mentioned in the manuscript is made after this function has been applied (see Noise_Reduction_Example_GH.m). 

4) 'Noise_Reduction_Example_GH.m' is an example of how to use the median equalizer to filter bioacoustics signals. We have also provided ‘Call_Example.wav’ (a 1-min vocalization of male Great Spotted Kiwi) 
as an example signal to demonstrate our method. This code also replicates Figure 1A from the manuscript.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------
LAMDA

5) 'LAMDA_General.m' is an implementation of LAMDA (Learning Algorithm for Multivariate Data Analysis) that allows, supervised, online, and unsupervised learning. 
This code is ready to use as a standalone clustering/classification algorithm in any other application. Two LAMDA versions are implemented: (i) the one presented in our 
manuscript (full reinforced with the 3pi aggregator) and the most common implementation with a hybrid aggregator (min-max).

------------------------------------------------------------------------------------------------------------------------------------------------------------------------
CONVOLUTIONAL NEURAL NETWORK (CNN)

6) 'KiwiNet_GH.m' trains a CNN (KiwiNet) to recognise segmented vocalisations of different individuals. The results of this algorithm are the input for CDCN_GH.m. 
This code can also be used as a stand-alone individual recognizer (i.e., classic individual identification - no unknown individuals or censusing -). A set of acoustic recordings (individualized calls) has 
been provided (see link at the beginning of the ReadMe) so you can use it as an example.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------
CONVOLUTIONAL DEEP CLUSTERING NETWORK (CDCN)

7) 'CDCN_GH.m' performs the joint training of the CNN with LAMDA. This produces an acoustic feature set that accurately characterizes vocal individuality and is useful for clustering tasks (e.g., censusing). 
This code (CDCN_GH.m) uses as input ‘CNN_and_Data.mat’, which is the output from KiwiNet_GH.m. The output of CDCN_GH.m ('KiwiNet_trained_CDCN.mat') is the CNN trained, which can be used in 
either Known_and_Unknown_ID_GH.m or Censusing_GH.m depending on your application. 

------------------------------------------------------------------------------------------------------------------------------------------------------------------------
KNOWN AND UNKNOWN INDIVIDUAL IDENTIFICATION

8) 'Known_and_Unknown_ID_GH.m' identifies calls from known and unknown individuals in the dataset. This code uses the results from CDCN_GH.m (which must be run after Kiwinet_GH.m). 
Use this code if you want to identify known and unknown individuals of a different species, or replicate the results of our manuscript from the raw acoustic data (see link at the beginning of the ReadMe). If you only 
want to identify known individuals, we recommend using KiwiNet_GH.m as it gives you the results directly and you do not have to perform the joint training with LAMDA.

If you want to replicate the plots from our manuscript, please use the two codes below:

9) 'Known_and_Unknown_Identification_Males_GH.m' replicates figures 3(A,C,E) and S2(A) of the manuscript. The algorithm uses as input Known_and_Unknown_ID_Males.mat (provided in the folder ‘models’), which contains the 
features extracted from the calls using KiwiNet (trained in the CDCN framework) and the data labels.
 
10) 'Known_and_Unknown_Identification_Females_GH.m' replicates figures 3(B,D,F) and S2(B) of the manuscript. The algorithm uses as input Known_and_Unknown_ID_Females.mat (provided in the folder ‘models’), which contains the 
features extracted using KiwiNet (trained in the CDCN framework) and the data labels.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
ACOUSTIC CENSUSING

11) 'Censusing_GH.m' census the number of individuals and assigns each call to their respective individual. This code uses the results from CDCN_GH.m (which must be run after Kiwinet_GH.m). Use this code if you want to census 
a different species, or replicate the results of our manuscript from the raw acoustic data (see link at the beginning of the ReadMe).

If you want to replicate the plots from our manuscript, please use the two codes below:

12) 'Censusing_Males_GH.m' replicates figures 4(A,C,E) and Figure S2(C) of the manuscript. The algorithm uses as input Censusing_Males.mat, which is provided in the folder ‘models’. It contains the 
features extracted from the calls using KiwiNet (trained in the CDCN framework) and the data labels.

13) 'Censusing_Females_GH.m' replicates figures 4(B,D,F) and Figure S2(D) of the manuscript. The algorithm uses as input Censusing_Females.mat, which is provided in the folder ‘models’. It contains the features 
extracted from the calls using KiwiNet (trained in the CDCN framework) and the data labels.

--------------------------------------------------------------------------------------------------------------------------------------------
ACOUSTIC FEATURES

The next three codes replicate Figures 2(A,B,C) of our manuscript, which compares our feature set with two common feature sets used in Bioacoustics and Ecoacoustics (i.e., spectro-temporal features and MFCCs). All plots are 
generated directly from the raw acoustic data.

14) 'Call_Features_GH.m' extracts seven spectro-temporal features (i.e., call duration, syllable duration, inter-syllable interval, number of syllables, dominant frequency, spectral centroid, bandwidth) from the roroa calls and 
uses t-SNE to generate a 2D ordination using these features. This code replicates Figure 2A of our manuscript.

15) 'MFCCs_GH.m' estimates 40 Mel-Frequency Cepstral Coefficients (MFCCs) from the roroa calls and performs a 2D ordination (t-SNE) on them. This code replicates Figure 2B of our manuscript.

16) 'Features_CDCN_GH.m' plots a 2D ordination (t-SNE) of the deep features extracted from the roroa calls using our proposed approach. This code replicates Figure 2C of our manuscript.

*****************************************************************************************************************************************************************************
Please cite any of the previous codes as: CL Bedoya and LE Molles. (2021) "Acoustic censusing and individual identification of birds in the wild".

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
TOOLBOXES

A) For the t-SNE plots, I used the code of van der Maaten and Hinton, please cite them if you use it:
*L. van der Maaten and G. Hinton. (2008). Visualizing data using t-SNE. Journal of Machine Learning Research 9(86):2579-2605. 

B) For the MFCCs estimation, I used the code from K. Wojcicki, please cite him if you use it:
*Wojcicki K. (2015). HTK MFCC MATLAB (Source Code).  Matlab Central File Exchange.

Enjoy it! ^^
