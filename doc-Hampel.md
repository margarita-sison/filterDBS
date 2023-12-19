# Documentation for getting Hampel filter to work on Yokogawa (Ricoh) data
## 🔖 References
The Hampel filter was adapted from: Kandemir, A.L., Litvak, V., Florin, E., 2020. The comparative performance of DBS artefact rejection methods for MEG recordings, NeuroImage, 2020, https://doi.org/10.1016/j.neuroimage.2020.117057 \
\
The Brainstorm-compatible MATLAB script was downloaded from: https://gitlab.com/lkandemir/dbs-artefact-rejection \
\
Please also cite: Allen, D.P., 2009. A frequency domain Hampel filter for blind rejection of sinusoidal interference from electromyograms. J. Neurosci. Methods 177, 303310. https://doi.org/10.1016/j.jneumeth.2008.10.019

## 🔧 Modifications
To get the script to work smoothly on Brainstorm, I added the ```badseg``` and ```demount``` functions as local functions at the end of the file. \
\
If you are interested in taking a closer look at the implementation, please see: https://github.com/margarita-sison/filterDBS/commit/8f793b758500b4b4adb43c28c277540ba5fe1f9c#diff-4f688c0e9f758bca8c9d0b8f47f58dd56d8a4d37b24a7fe4276b396c45709531 \
\
The ```demount``` function differentiates between manufacturers of MEG systems: 

```
    switch type
        case 'neuromag'
            bp=and(contains(channels, 'MEG'), flag);
        case 'ctf'
            bp=and(strcmp(channels, 'MEG'), flag);
        case 'megmag'
            bp=and(strcmp(channels, 'MEG MAG'), flag);
        case 'megplanar'
            bp=and(strcmp(channels, 'MEG GRAD'), flag);
    end
```

Our data was extracted using the Yokogawa (Ricoh) MEG system. The Yokogawa (Ricoh) channel file contains three different types of channels: 'MEG', 'MEG REF', and 'EEG'. 
- 'MEG' channels correspond to <b>MIT KIT system gradiometers</b>. 
- 'MEG REF' channels correspond to <b>KIT magnetometers</b>. 

If you want to process only the gradiometers, add the following ```otherwise``` block:
```
        otherwise
            bp=and(strcmp(channels, 'MEG'), flag);
```

If you want to process both gradiometers and magnetometers, add the following ```otherwise``` block:
```
        otherwise
            bp=and(contains(channels, 'MEG'), flag);
```

These modifications are implemented here: https://github.com/margarita-sison/filterDBS/commit/6ad9d3c610db28b42dceb0ec581b3f5e9edba7ed

## 📐 Parameters
To learn more about how the Hampel filter works, please read section 2.3.1. of the [article](https://doi.org/10.1016/j.neuroimage.2020.117057) by Kandemir et al. (2020). Section 4.1. lays out recommendations for optimal parameter selection. \
\
<b>The important points from both sections are included below.</b> 

1. Window Length (Hz)
    - First, the frequency spectrum of sensor-space data is computed using the fast Fourier transform (FFT). In the frequency domain, narrow frequency peaks induced by DBS are considered outliers. The Hampel operates on a sliding window, identifying outliers in both the real and imaginary part of the frequency spectrum within a given window. The identified outliers are replaced with the median of the window. After the frequency spectrum is filtered, it is transformed back into the time domain using the inverse FFT.
    - DBS-induced peaks are typically 0.2 Hz in width. To effectively filter these artifacts, a sliding window length of at least twice their width is recommended. Note that the computational complexity of the algorithm scales with the length of the window.
    - In the study performed by Kandemir et al. (2020), window lengths of 0.5, 6, and 10 Hz were tested on a CTF MEG system. 6 and 10 Hz windows yielded better results than the 0.5 Hz window, but the results did not differ between the 6 and 10 Hz windows. Therefore, a 6 Hz window was used for further analyses.
    - Options: 0.25, 0.50, 0.75, 1, 2, 3, 4, 5
3. Constant
    - This is referred to as the <i>threshold parameter C</i>. If a value is <i>C</i> standard deviations above the median of the frequency spectrum within a given window, then it is considered an outlier. The higher <i>C</i> is, the less sensitive the Hampel filter is to outliers. Accordingly, by adjusting this parameter, we can adjust the sensitivity of the Hampel filter to DBS-induced peaks.
    - <i>C</i> = 1 over-filters the artifacts and is therefore not recommended. <i>C</i> values greater than 1 (i.e., from 2 to 8) yield comparably good results.
    - Options: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
4. Frequencies to Reject
    - Default: All (recommended)
5. Lowpass Frequency
    - Default: 100.00 Hz

In general, it is advisable to decide on the optimal parameters by performing a visual inspection of the filtered frequency spectrum.
<img width="390" alt="image" src="https://github.com/margarita-sison/filterDBS/assets/130074310/d524f374-7707-4ca2-a2f9-20b599913d31"> 

## 📈 Demo
Shown here are spectra of the 'MEG' and 'MEG REF' channels. The upper figure shows the channel spectra before a Hampel filter was applied to the MEG recording. \
\
A Hampel filter was applied to both the 'MEG' and 'MEG REF' channels with the following parameter specifications:
- Window Length (Hz) = 0.5
- Constant = 2
- Frequencies to Reject = All
- Lowpass Frequency = 250.00 Hz
<img width="960" alt="image" src="https://github.com/margarita-sison/filterDBSartifacts/assets/130074310/e1810eb3-145a-44c9-b2c2-9537e3d9eb05">

