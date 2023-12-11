## Data Augmentation

### Initial Idea
After checking the new dataset, I will still suggest performing data augmentation since there are only 6323 samples in the training set (while the classic CNN AlexNet model has 1.2 million high-resolution training examples, but for 1000 categories classification).

Here are the augmentation steps I'm working on:

1. rotation
2. flipping
3. scaling
4. adjusting brightness
5. adjusting contrast
6. random erasing
7. noise injection
8. Gaussian blur
9. edge enhancement

For a loop, it will randomly apply one or multiple steps from above with random parameters in a pre-defined range (to avoid distorting the semantic of image too much) for each image. In this way, we can generate another 6323 new samples for each loop. If we set the number of loops as 10, it will have 63230 training samples.
One concern I have is that if the augmented images are too many, that means there are many 
    similar images for a label. When augmented images are too similar to the original ones, they don't contribute much to the model's ability to generalize. It's crucial to find a balance where each augmented image provides new information.

Thus, I'm thinking about starting with groups of different number of augmented images, such as 
6323x2, 6323x5, 6323x10, etc. When you are training, you can test which is the best size of 
augmentation. Maybe monitor the model's performance and gradually increase the number if it improves the model without causing overfitting.

## Current Approach
Another augmentation step I'm applying is extracting images (with labels of cat, dog, and reptile) from ImageNet's 32x32 downsampled image data. This can surely increase the performance of our three-class model.

I'm thinking of training one general category model and three identical subcategory models for each Dog, Bird, and Reptile. The image will be sent to the general model, which predicts Dog|Bird|Reptile|Novel. Then, it will decide which one of the three models to send to have a more specific subcategory prediction. The pro is each subcategory model will perform better given limited training data. The con is when a novel image is incorrectly predicted as one of three classes, this error rate may be higher since there is no subcategory model that can eliminate novel images.

Thus, it will be important for us to train a decent general model that predicts Novel items. To do this, I'm preparing a balanced training dataset:

1. expand our training set by retrieving all samples labeled as DOG, BIRD, REPTILE from ImageNet. 
The crucial part here is ImageNet does label images with specific species name. I'm using LLMs to map those label names into DOG, BIRD, or REPTILE.
2. After having a larger dataset, I will calculate subcategory_number=min(len(DOG), len(BIRD), len(REPTILE)) . Then, I will collect subcategory_number  images from the ImageNet 32x32 randomly with labels other than the three.
3. Perform data augmentation steps as discussed above.

I didn't perform those conventional augmentation techniques (rotation, flipping, scaling, etc.) since I believe this dataset is sufficiently large for our task. However, if we hit an overfitting problem easily in training, we could consider performing such techniques to make our training set even larger.

## Results
Please check `1.Load_Data.ipynb` for preprocessed dataset.

## Plans
Now, I'm planning to (1) organize my preprocessed dataset within a Jupiter Notebook for you -- 
this will be done in a few minutes (check `1.Load_Data.ipynb`), and (2) since the professor has 
provided a simple CNN demo,
I will explore how increasing the training set affects the performance of the classification with that CNN demo (I would expect a significant improvement).