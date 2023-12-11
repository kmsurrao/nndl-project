# nndl-project

Exploration of image classification into superclasses and subclasses, with low resolution images and novel classes introduced at test time.  

See [data_augmentation](data_augmentation/) for information about augmenting the dataset.  

See [CNN](CNN/) for a simple convolutional neural network implementation of superclass and subclass classification combined, superclass alone, and subclass alone. Here there is no special handling of novel classes introduced at test time.    

See [transfer_learning](transfer_learning/) for a fine-tuned ResNet-18 model, again with implementations for superclass and subclass classification combined, superclass alone, and subclass alone. There is also a fine-tuned MobileNet_V2 model for superclass and subclass classification combined. Here there is no special handling of novel classes introduced at test time.    

See [novel_classes](novel_classes/) for code that handles classifying superclasses and/or subclasses as novel at test time.  The notebook [probabilistic_superclass_and_subclass.ipynb](novel_classes/probabilistic_superclass_and_subclass.ipynb) does this (using a fine-tuned ResNet-18 model) by comparing softmax probabilities, looking at the maximum probability among the previously seen classes, and comparing it to a threshold probability to determine whether the test image belongs to the previously seen class or a novel class. The threshold probabilities are determined based on the mean and variance of the probabilities of image classification in the validation set.   

