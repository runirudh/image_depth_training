# image_depth_training
data is stored originally in a npz format. (Numpy ndarray)
independent variable -> input data is a 4d array containing 3500 color (rgb) images. 
                        size of image is 96 x 96 pixels
                        data shape is (3500,3,96,96)     .. (no. of images, channels, pixel value, pixel value)
dependent variable   -> output data is depth information for these 3500 images (over a single channel) stored in a 4d array.
                        size of image is 96 x 96 pixels.
                        this is the time of flight information, coloring the image such that nearer objects are differently colored than far objects in an image.
                        shape is (3500,1,96,96)

This is a regression problem where the predictions are multi-output, namely the 9216 pixels of the depth matrix. 

It is presumed that the model violates iid assumptions, since adjacent pixels are likely to be related and not independent.
While a customized Generalized model would be the right choice, due to limited time, simple models are fitted. 
Residual analysis is done for the best predictive models to see variance of residuals and if any patterns exist, as when iid assumptions are not met, the results we see are often erroneous.  

Test metric for model is mean sqaured error. CV is used to get accurate results. 

Models fitted are - Linear, Linear regularized, KNeighborsRegressor , MLPRegressor, CNN, RandomForest, DecisionTreeRegressor, SVM Regressor (chained reg.)
with both normal flattened and convoluted data to see predicted power.

Predicted depth images have been plotted as well. 



