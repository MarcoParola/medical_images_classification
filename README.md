# Convolutional Neural Network for Medical Imaging Analysis - Abnormality detection in mammography

### Description of abnormalities and Classification Tasks
The CBIS-DDSM dataset represents a collection of images from two classes of abnormalities. Indeed, it enables a task of **abnormality classification**, which aims at distinguishing the following classes:
- Mass
- Calcification


Furthermore, several csv files are hosted [here](https://wiki.cancerimagingarchive.net/display/Public/CBIS-DDSM#510bc7863f3042f59a27301f1f4b6bef) and provide a detailed description of each image, according to the following fields:

`patient_id, breast density, left or right breast, image view, abnormality id, abnormality type, calc type, calc distribution, assessment, pathology, subtlety, image file path, cropped image file path, ROI mask file path`
 
Such description enables another fine-grained task: **abnormality diagnosis classification**. It aims at distinguishing the following classes:
- Mass, Benign (with or without callback)
- Mass, Malignant
- Calcification, Benign
- Calcification, Malignant



## Dataset as it is provided for the final project

Dealing with original dataset is critical since full images are high resolution and the DICOM format is not natively supported in tf.keras.

**Indeed, you are provided with numpy arrays containing images and labels from training and test sets.**

The steps performed on each original image are described below:

- the **abnormality patch** has been extracted from the original image according to the binary mask;
- a patch of healthy tissue (**baseline patch**) adjacent to the abnormality patch has been extracted from the original image (left, right, top or bottom - no overlap). Both **abnormality patch** and **baseline patch** have been added to the images tensor; in other words, an abnormality patch has been ignored if a related baseline patch could not be extracted.
- both abnormality patch and baseline patch have been resized to shape (150x150)  using OpenCV resize function: `cv2.resize(img, dsize=(shape, shape), interpolation=cv2.INTER_NEAREST)`
- class labels have been assigned to the patches according to the following mapping:
  - 0: Baseline patch
  - 1: Mass, benign
  - 2: Mass, malignant
  - 3: Calcification, benign
  - 4: Calcification, malignant 
- images of baseline patch and abnormality patch, and their related labels, have been added to distinct numpy arrays for images and labels. 
  - `train_tensor.npy`: images tensor for training
  - `train_labels.npy`: labels tensor for training
  - `public_test_tensor.npy`: images tensor for test
  - `public_test_labels.npy`: images tensor for test
- The images tensor of a private test set is also provided. The relative labels tensor is not published within the project files.
  - `private_test_tensor.npy`
  - `private_test_labels.npy`