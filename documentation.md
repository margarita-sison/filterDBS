# Documentation for arriving at MRI registration
## 📂 ber009_stimoff files
📄 <b>.mrk</b> - contains Head Position Indicator coils in MEG device coordinates\
📄 <b>.con</b> - contains Yokogawa/KIT data\
📄 <b>.sfp</b> - surface point file containing position of fiducial points, marker points, and other points relating to head shape\
📄 <b>.nii</b> - MRI file containing T1-weighted scan\
📄 <b>.cms</b> - ?\
📄 <b>.epf</b> - ?

## 🪖 Brainstorm protocol
1. After starting Brainstorm, created a new protocol named <b>ber009_stimoff</b> and new subjects named <b>Subject01_Ricoh</b> and <b>Subject02_Yokogawa_KIT</b>
2. Import MRI > MRI > selected <I>folder</i> containing .nii file
3. Compute MNI normalization
4. Save MRI
5. Review raw file
