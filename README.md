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
                        stored in a 4d array with shape (3500,1,96,96).
                        
                        size of image is 96 x 96 pixels.
                        
                        **pixel value is assumed to be the _time of flight information_, 
                     
                        coloring the image such that Nearer Objects in image are colored differently from Distant Objects
                        
                        depth range is originally [-1.2,0] which is normalized to [0, 1.2] 

This is a regression problem where we make **multiple predictions**, namely **predicting the 9216 pixels** of the output depth matrix (image). 

**Note** ->

It is presumed that the model violates iid assumptions, since adjacent pixels are likely to be related and not independent.
While a Generalized model might be the right choice here, due to time/ knowledge constraints, 
simple models are fitted & tested by changing hyperparameters. 

Residuals are printed out for the best predictive models to see variance of residuals and if any patterns exist, 
as when iid assumptions are not met, the results we get are often erroneous.  

**Test metric for model is Mean Squared error**

**CV is used to get accurate result**

## Models fitted are - 
### Linear, Linear regularized, KNeighborsRegressor , Multi Layer Perceptron Regressor, CNN, RandomForest, DecisionTreeRegressor

Models are fitted with both normal flattened array data and convoluted data. 

The contrast in predictive power of models of using different inputs is showcased through computation time and by comparing mse values.  

### Predicted depth images have been plotted at each stage to highlight difference between models.

### Finding : Convoluted data achieves better prediction MSE and run time. 

## Model Performance:

Linear        =              okay , best mse = 0.008     (for unregularized)

Knn           =              better, best mse = 0.0051 at k=2

MLRegressor   =              learns to recognize patterns but in same part of the image, ends making same predictions for test data, mse = 0.0057

**CNN**           =              good fit, best mse = 0.0035, R2 = 0.86

**Random Forest** =              good fit, best mse = 0.0032

**Decision Tree** =              good fit, best mse = 0.0034


