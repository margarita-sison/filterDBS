# Documentation for arriving at MRI registration
## 📂 ber009_stimoff files
📄 <b>.mrk</b> - contains Head Position Indicator coils in MEG device coordinates\
📄 <b>.con</b> - contains Yokogawa/KIT data\
📄 <b>.sfp</b> - surface point file containing position of fiducial points, marker points, and other points relating to head shape\
📄 <b>.nii</b> - MRI file containing T1-weighted scan\
📄 <b>.cms</b> - ?\
📄 <b>.epf</b> - ?

## 🪖 Brainstorm workflow
1. After starting Brainstorm, create a new protocol and name it <b>ber009_stimoff</b>. Under <b>Default anatomy</b>, select <i>No, use individual anatomy</i>. Under <b>Default channel file</b>, select <i>No, use one channel file per acquisition run (MEG/EEG)</i>.
2. Create a new subject with the same settings as the newly created protocol. The new subject is named <b>Subject01</b> by default.
3. In the Anatomy tab, right-click on <b>Subject01</b> and select <i>Import MRI</i>. Select the file format <b>MRI: NIfTI-1...</b> and then select the folder containing the <b>.nii</b> MRI file. 
5. Compute MNI normalization. Select <b>maff8</b>. Click the Save button on the MRI Viewer.
6. Under <b>Subject01</b>, right-click on the T1-weighted scan. Under <i>MRI segmentation</i>, click on <i>Generate head surface</i>. Use the given defaults and click OK.
7. In the Functional data tab, right-click on <b>Subject01</b> and select <i>Review raw file</i>. Select the file format <b>MEG/EEG: Ricoh...</b> and then select the folder containing the <b>.mrk</b> and <b>.con</b> files.
8. The following warning will appear:
   <img width="747" alt="image" src="https://github.com/margarita-sison/MEGpipelines/assets/130074310/71a0ab63-702a-41c2-9f2e-ba34d80fb3f7">
   ```
   KIT> Warning: The dataset has not been co-registered with the MRI with the RICOH software...
   BST> Not enough digitized head points to perform automatic registration.
   ```
9. Click OK anyway. The following image will appear:
    <img width="701" alt="image" src="https://github.com/margarita-sison/MEGpipelines/assets/130074310/2058b327-cea4-4d43-8949-435635615602">

