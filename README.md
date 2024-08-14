# CS_7643_DL_project
CS 7643 Deep Learning Project

### Abstract:
The Primary purpose of our experiment was to in- vestigate the performance and generalizability of models on both stylized and unstylized image sets. Specifically, we aim to repeat the experiments “IMAGENET-TRAINED CNNS ARE BIASED TOWARDS TEXTURE; INCREAS- ING SHAPE BIAS IMPROVES ACCURACY AND ROBUST- NESS” published by University of Tubingen IMPRS-IS while also comparing the behavior of different types of im- age sets. Unlike the original study, we have also included models built for medical images, which tend to have a much higher focus on texture compared to most multiclass image sets. Each image category had a single model build, whose architecture and hyperparameters were optimized for the unstylized data.

The models’ performances on both data sets were compared when trained on stylized data vs unstyl- ized data. The vehicle dataset showed lower accuracy when trained and tested on stylized data (78.52%) than when trained and tested on unstylized data (82.82%). However, training on stylized and testing on unstylized gave slightly better results (38.66%) than training on unstylized and test- ing on stylized (35.22%), showing that stylized model may have slightly better ability to generalize. Our results do not match the study mentioned above however it is to be noted that our procedure differed slightly in that the same model was used for both training cases rather than optimizing a separate architecture and hyperparameters set for the styl- ized data. This may be one of the factors resulting in this difference in results. The Melanoma dataset behaves as pre-dicted, with stylized dataset having worse results and gen- eralizability across the board. The breast cancer dataset behaved as predicted, with stylizing having little effect. 


This notebook consists of the model built for the breast cancer dataset. 
