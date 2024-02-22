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
3. In the Anatomy tab: Right-click on <b>Subject01</b> and select <i>Import MRI</i>. Select the file format <b>MRI: NIfTI-1...</b> and then select the folder containing the <b>.nii</b> MRI file. 
5. Compute MNI normalization. Select <b>maff8</b>. Click the Save button on the MRI Viewer.
6. Under <b>Subject01</b>, right-click on the T1-weighted scan. Under <i>MRI segmentation</i>, click on <i>Generate head surface</i>. Use the given defaults and click OK.
7. In the Functional data tab: Right-click on <b>Subject01</b> and select <i>Review raw file</i>. Select the file format <b>MEG/EEG: Ricoh...</b> and then select the folder containing the <b>.mrk</b> and <b>.con</b> files. The following warning will appear:
   <img width="500" alt="image" src="https://github.com/margarita-sison/MEGpipelines/assets/130074310/71a0ab63-702a-41c2-9f2e-ba34d80fb3f7">
   ```
   KIT> Warning: The dataset has not been co-registered with the MRI with the RICOH software...
   BST> Not enough digitized head points to perform automatic registration.
   ```
8. Click OK anyway. The following image will appear:\
    <img width="500" alt="image" src="https://github.com/margarita-sison/MEGpipelines/assets/130074310/2058b327-cea4-4d43-8949-435635615602">
9. In the Anatomy tab: From the <b>Default anatomy</b> folder, click and drag the <b>cortex_15002V</b> file to the <b>Subject01</b> folder.
10. Then right-click on the <b>cortex_15002V</b> file and hover over <i>Align manually on...</i>. Select <i>raw_anat_t1_Ber009</i>. The following image will appear:
   ![image](https://github.com/margarita-sison/filterDBSartifacts/assets/130074310/46a05f06-9ea8-4be9-9de2-0fe0834246c1)
11. In the Functional data tab: Right-click on the channel file <b>RICOH channels (160)</b> and select <i>Compute head model</i>. Use the given defaults and click OK.\
    <img width="312" alt="image" src="https://github.com/margarita-sison/filterDBSartifacts/assets/130074310/be3536f2-b70d-4504-91a7-91165de5e075">
13. A new file will be generated in the same folder where the channel file is located. This file contains the leadfield matrix.
    <img width="239" alt="image" src="https://github.com/margarita-sison/filterDBSartifacts/assets/130074310/2765446f-87d8-4db4-add2-ee7a5ef80fca">
14. Right-click on the <b>Overlapping spheres</b> file and select <i>Check spheres</i>. The following image will appear:
    <img width="439" alt="image" src="https://github.com/margarita-sison/filterDBSartifacts/assets/130074310/7cb552c9-7896-41cd-bd73-66778d677d54">
15. The leadfield matrix is contained in the headmodel_surf_os_meg.mat file. Right-click on the <b>Overlapping spheres</b> file and hover over <i>File</i>. Select <i>View file contents.</i> Here, you can see that the leadfield matrix has a dimension of 160 sensors-by-45006 sources (or 3*15002 vertices).\
    <img width="579" alt="image" src="https://github.com/margarita-sison/filterDBSartifacts/assets/130074310/ae0d9f60-372d-4824-9446-ddbd7656b1b9">


   

