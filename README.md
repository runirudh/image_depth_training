# image_depth_training
data download link https://www.kaggle.com/kmader/showing-the-rgbd-images/notebook via kaggle nb

data is stored originally in a npz format (a Numpy file format)

independent variable -> input data is a 4d array containing 3500 color (rgb) images. 
                        size of image is 96 x 96 pixels
                        data shape is (3500,3,96,96)     .. (no. of images, channels, pixel value, pixel value)
                        
dependent variable   -> output data is depth information for these 3500 images (over a single channel) stored in a 4d array.
                        size of image is 96 x 96 pixels.
                        this is the time of flight information, coloring the image such that nearer objects are differently colored than far objects in an image.
                        shape is (3500,1,96,96)

This is a regression problem where we make multiple predictions, namely the 9216 pixels of the output depth matrix (image). 

It is presumed that the model violates iid assumptions, since adjacent pixels are likely to be related and not independent.
While a customized Generalized model would be the right choice, due to limited time/knowledge, simple models are fitted. 
Residual analysis is done for the best predictive models to see variance of residuals and if any patterns exist, as when iid assumptions are not met, the results we see are often erroneous.  

Test metric for model is mean sqaured error. CV is used to get accurate results. Different hyperparameters are tested for best fit & results.

Models fitted are - 
Linear, Linear regularized, KNeighborsRegressor , Multi Layer Perceptron Regressor, CNN, RandomForest, DecisionTreeRegressor

Models are fitted with both normal flattened array data and convoluted data. The contrast in predictive power of models of using different inputs is showcased through time taken for model to run and by comparing mse values.  

Predicted depth images have been plotted as well to highlight difference between models. Convoluted data achieves better prediction mse and run time. 

Model Performance:

Linear        =              okay , best mse = 0.008     (for unregularized)

Knn           =              better, best mse = 0.0051 at k=2

MLRegressor   =              learns to recognize patterns but in same part of the image, ends making same predictions for test data, mse = 0.0057

CNN           =              good fit, best mse = 0.0035, R2 = 0.86

Random Forest =              good fit, best mse = 0.0032

Decision Tree =              good fit, best mse = 0.0034



