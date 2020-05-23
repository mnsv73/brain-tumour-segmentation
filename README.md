# Brain Tumour Segmentation

### Research Paper Summary

---

### About the dataset
The dataset is taken from SICAS Medical Image Repisotry, BRATS 2015: Brain Tumour Image Segemntation Challenge. It has MRI scans of 274 brains of which 220 contain HGG(High Grade Gliomas) and 54 contain LGG(Low Grade Gliomas). Each brain in the dataset has four Modalities(T1,T2, T1_C and Flair). The shape of each of these modalities as numpy arrays is 240x240x155. More information on the dataset can be viewed from the Dataset_Information.ipynb.

---
### Dataset Preprocessing
N4ITK bias correction is applied to T1 and T1_C modalitites. Each of the four modalities are normalized by subtracting the corresponding mean and dividing by the corresponding standard deviation. All the 155 slices from each of the four modalities(T1,T1C,T2 and Flair) are extracted. The slices from the four modalities are stacked together to form input of 240x240x4.
### CNN Models 
The inputs to the CNN are given as patches which are created by converting the 3D MRI scans into 2D. These patches are used to train the CNN to predict the class of the pixel centered at the patch.  
#### Two-pathway CNN
![Screenshot (116)](https://user-images.githubusercontent.com/64637263/82737775-0af59580-9d51-11ea-9391-fada21a6954f.png)

This CNN takes a patch of dimension 33x33x4 from the input and sends it to two pathways: one focuses on details on the region around middle pixel  while the other focuses where the patch is located in the brain. The output of the CNN is the label of the central pixel, one hot encoded. 
#### Input Cascade CNN

#### Local Cascade CNN
---

### Issue Faced

---

### Results

---

### Instructions to run the code

---

