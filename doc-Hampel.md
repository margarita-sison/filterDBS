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

## 📏 Parameters
<img width="390" alt="image" src="https://github.com/margarita-sison/filterDBS/assets/130074310/d524f374-7707-4ca2-a2f9-20b599913d31">

