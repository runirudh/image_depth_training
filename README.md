# Training depth information for objects in an image
### data download [link](https://www.kaggle.com/kmader/showing-the-rgbd-images/notebook) via K Scott Mader's kaggle nb

data is stored originally in a npz format (a Numpy file format)

**independent variable** -> 

      input data is a 4d array containing 3500 color (rgb) images. 
                        
      size of image is 96 x 96 pixels
                        
      <data shape is (3500,3,96,96)>   .. (no. of images, channels, pixel value, pixel value)
                        
      pixel range is originally [0,255] changed to [0,1]
                        
**dependent variable**   -> 

      output data is depth information for these 3500 images (over a single channel) 
      
      stored in a 4d array with shape (3500,1,96,96). size of image is 96 x 96 pixels 
                        
      pixel value here is assumed to be the time of flight information, 
                     
      coloring the image such that Nearer Objects in image are colored differently from Distant Objects
                        
      depth range is originally [-1.2,0] which is normalized to [0, 1.2] 

### Data looks like:

<img width="642" alt="Screen Shot 2022-01-15 at 1 52 48 PM" src="https://user-images.githubusercontent.com/96305841/149634354-fdca6d6b-ea37-4d68-8e0a-56f8fdb638a5.png">

<img width="603" alt="Screen Shot 2022-01-15 at 1 24 43 PM" src="https://user-images.githubusercontent.com/96305841/149633462-c55f0991-4e8c-4895-84d4-87a017a7c64c.png">



This is a **_Multi-Output_ Regression problem** where we make **multiple predictions**, 
namely **predicting the 9216 pixels** of the output depth matrix (image). 

### Convolution
We convolute the RGB input matrix to yield **convoluted matrix to make predictions ->**
  
  This drastically reduces the feature space from 27648 (3x96x96) to 96 features.

  Training depth on this matrix, allows us better run times and better learning.

**Note** ->

      It is presumed that the model violates iid assumptions on which we usually 
      base our regression model, since adjacent pixels are likely to be related and 
      not independent. 
      
      While a Generalized model might be the right choice here, 
      
      due to time/ knowledge constraints, simple models are fitted & tested
      
      by changing hyperparameters. 

      Residuals are printed out for the best predictive models to see variance of residuals and 
      
      if any patterns exist, as when iid assumptions are not met, the results are often erroneous.  

**Test metric for model is Mean Squared error**

**CV is used to get accurate result**

## Models fitted are - 
### Linear, Linear regularized, KNeighborsRegressor , Multi Layer Perceptron Regressor, CNN, RandomForest, DecisionTreeRegressor

Models are fitted with both normal flattened array data and convoluted data. 

The contrast in predictive power of models of using different inputs is showcased through computation time and by comparing mse values.  

**Predicted depth images** have been plotted at each stage to highlight difference between models.

### Finding : Convoluted data achieves better prediction MSE and run time. 

### Predictions:

  1. CNN :
  <img width="334" alt="Screen Shot 2022-01-15 at 2 06 44 PM" src="https://user-images.githubusercontent.com/96305841/149634742-a48e3cbc-e8c5-4bca-9041-7c3e4b07c1f2.png">
  
  2. Decision Tree Regressor :
<img width="351" alt="dtr" src="https://user-images.githubusercontent.com/96305841/149638084-94419999-edcd-45c1-9403-32e86ba3da5c.png">

  3. K Neighbors Regressor :
<img width="315" alt="knn" src="https://user-images.githubusercontent.com/96305841/149638088-6f1540f1-7526-4366-9813-2e82912e310b.png">

  4. Linear Regressor (mse = 0.008) :
<img width="317" alt="linear" src="https://user-images.githubusercontent.com/96305841/149638101-160af238-88cd-47c2-89ab-9993297c3e2d.png">

  5. Random Forest :
<img width="386" alt="rf" src="https://user-images.githubusercontent.com/96305841/149638126-b06865c5-f484-43bb-81a2-1fda723a3cc9.png">

  6. MLP Regressor :
<img width="444" alt="mlp" src="https://user-images.githubusercontent.com/96305841/149638198-9b866ca6-1ffb-49fe-a6db-acbda1521035.png">


## Model Performance (cross validation results):

**Linear**        =              okay model, best mse = 0.008     (for unregularized)

**Knn**           =              good fit, best mse = 0.0051 at k=2

**MLP Regressor**   =             Bad model. reduces overall mse by making the same predictions, mse = 0.0057

**CNN**           =              good fit, **best mse** = 0.0035, R2 = 0.86

**Random Forest** =              good fit, **best mse** = 0.0032

**Decision Tree** =              good fit, **best mse** = 0.0034


