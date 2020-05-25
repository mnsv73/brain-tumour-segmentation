# Brain Tumour Segmentation
This project implements the CNN architectures for brain tumour segmentation described in the research paper Havaei et al.(2017). The paper can be accessed from the link  https://arxiv.org/pdf/1505.03540v3.pdf.

### Research Paper Summary
The main objective of Brain Tumor Segmentation is to identify the exact location and extension of tumor which can help in providing better treatment. This is done using the per-pixel classification into one of the five labels, namely healthy voxels, edema, necrosis, non-enhancing and enhancing tumors. Based on the presence of these, the tumors can be categorised into complete tumor, tumor core and enhanced tumor. 

---

### About the dataset
The dataset is taken from SICAS Medical Image Repository, BRATS 2015: Brain Tumour Image Segemntation Challenge. It has MRI scans of 274 brains of which 220 contain HGG(High Grade Gliomas) and 54 contain LGG(Low Grade Gliomas). Each brain in the dataset has four Modalities(T1,T2, T1_C and Flair). The shape of each of these modalities as numpy arrays is 240x240x155. More information on the dataset can be viewed from the Dataset_Information.ipynb.

---
### Dataset Preprocessing
N4ITK bias correction is applied to T1 and T1_C modalitites. Each of the four modalities are normalized by subtracting the corresponding mean and dividing by the corresponding standard deviation. All the 155 slices from each of the four modalities(T1,T1C,T2 and Flair) are extracted. The slices from the four modalities are stacked together to form input of 240x240x4.

---
### CNN Models 
The inputs to the CNN are given as patches which are created by converting the 3D MRI scans into 2D. These patches are used to train the CNN to predict the class of the pixel centered at the patch.  
#### Two-pathway CNN
![Screenshot (116)](https://user-images.githubusercontent.com/64637263/82737775-0af59580-9d51-11ea-9391-fada21a6954f.png)

This CNN takes a patch of dimension 33x33x4 from the input and sends it to two pathways: one focuses on details on the region around middle pixel  while the other focuses where the patch is located in the brain. The output of the CNN is the label of the central pixel, one hot encoded. This whole architecture is depicted as a green box in the subsequent CNN architectures.
#### Input Cascade CNN
![Screenshot (118)](https://user-images.githubusercontent.com/64637263/82737976-29a85c00-9d52-11ea-9787-399806cf1aaa.png)

CNNs often predict each label too far from each other. To prevent this, the above architecture feeds the output of the first Two-pathway CNN as an additional input directly into the second Two-pathway CNN as shown in the figure. This whole architecture is referred to as Input Cascade CNN. 

#### Local Cascade CNN
![Screenshot (119)](https://user-images.githubusercontent.com/64637263/82738078-d2ef5200-9d52-11ea-99a9-b8db388fac46.png)

In this architecture the output of the first CNN is concatenated to the hidden layer of the second CNN as shown instead of passing it as an input.

---

### Training 
To account for the high imbalance in the dataset, a **two-phase training procedure** was employed. The steps followed in it are given below:
- The input patches were constructed in such a way that all the labels were equally likely.
- These patches were passed to the CNN in the first training phase.
- The weights of all layers except the output layer were declared as non-trainable.
- The output layer was re-trained with a more representative data in the second phase.
- So, the lower layers could recognise all the classes and the output layer was calibrated correctly.

#### Loss Function
Categorical Cross Entropy is used as we are dealing with multi-label classification.

#### Optimiser
Stochastic Gradient Descent(SGD) with a learning rate of 0.005 and momentum factor of 0.5 are used as suggested in the paper.

### Issue Faced
There was too much regularization in the paper as the dataset implemented was 9 times smaller. Too much regularisation casued our models to severely underfit. To avoid overfitting, we trained both the models with as much data as our limited resources allowed.
Another approach for reducing class imbalance is by passing class_weight parameter during the training. This parameter gives more importance (assigns larger weights) to under-represented classes. However, this method does works well for mild imbalance (eg - 30% and 70%) and not in highly skewed data such as BRATS 2015 Dataset.
Due to computational and hardware limitations, our model was trained on barely sufficient data. 

---

### Results
Precision, recall and dice coefficient are the metrics used for evaluation. 

![Screenshot (126)](https://user-images.githubusercontent.com/64637263/82751910-2657b380-9dd8-11ea-8956-9984abcd6154.png)

![Screenshot (133)](https://user-images.githubusercontent.com/64637263/82752151-aaf70180-9dd9-11ea-8345-e716eddb1676.png)


---

### Instructions to run the code
Download the pre-trained models and weights stored as json file and h5 file respectively along with the LocalReTrain and InputCascadeCNNReTrain notebooks. Load the model and pre-trained weights in the jupyter notebooks. 
Change the hard-coded file path and use the notebooks for either viewing the segmented images or for transfer learning.

---

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |

