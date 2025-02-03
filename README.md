## MoCo Self Supervised Learning Method

We are using STL-10 dataset, following recommended protocol on [cs.stanford.edu/~acoates/stl10/](https://cs.stanford.edu/~acoates/stl10/)
> Perform unsupervised training on the unlabeled.

> Perform supervised training on the labeled data using 10 (pre-defined) folds of 100 examples from the training data. The indices of the examples to be used for each fold are provided.

> Report average accuracy on the full test set.

Also, following original paper [arxiv.org/pdf/1911.05722](https://arxiv.org/pdf/1911.05722) __Momentum Contrast for Unsupervised Visual Representation Learning__ modelled our MoCo from scratch with most of the hyper-parameters recommended there.

Next, we are experiment with only 10 epochs (for not having GPU). Our results are given below:


| Ex. No. | Base Encoder | MLP Head | Queue Size | Mtm |Temp. | Trainable Params |Num Epochs | Learning Rate | Training Time (on unlabeled) | Accuracy on TrainingSet | Accuracy on TestSet|
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ----- | ------| -------| ----| --- |
| 1 | ResNet18  | False | 65,536 | 0.999 | 0.07 |11,242,176 | 10 | 0.001 | 8 Hours 13 Min| 45.76% | 29.21% |
| 2 | ResNet18  | False | 65,536 | 0.999 | 0.07 |11,242,176 | 10 | 0.01 | 7 Hours 40 Min| 36.8% | 21.26% |
| 3 | ResNet18  | False | 65,536 | 0.999 | 0.07 |11,242,176 | 10 | 0.03 | 8 Hours 10 Min| 33.84% | 20.19% |
| 4 | ResNet18  | False | 65,536 | 0.999 | 0.09 |11,242,176 | 10 | 0.001 | 8 Hours 53 Min| 46.78% | 28.81% |
| 5 | ResNet18  | False | 65,536 | 0.999 | 0.05 |11,242,176 | 10 | 0.001 | 8 Hours 48 Min| 45.56% | 24.86% |
| 6 | ResNet50  | False | 65,536 | 0.999 | 0.05 |23,770,304 | 10 | 0.001 | 20 Hours 10 Min| 41.42% | 24.52% |

Certainly, we need to train on ``unlabeled set`` for almost upto $100$ epoch as in __MoCo__ paper, and that will happen when we get access to GPU.

Ran two more time, with increased ``num_epochs`` to 20 and 30 for ResNet18 ``base_encoder``. Results certainly improved. We definitely need to run the model for 100.

| Ex. No. | Base Encoder | MLP Head | Queue Size | Mtm |Temp. | Trainable Params |Num Epochs | Learning Rate | Training Time (on unlabeled) | Accuracy on TrainingSet | Accuracy on TestSet|
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ----- | ------| -------| ----| --- |
| 7 | ResNet18  | False | 65,536 | 0.999 | 0.07 |11,242,176 | 20 | 0.001 | 16 Hours 8 Min| 46.24% | 30.63% |
| 8 | ResNet18  | False | 65,536 | 0.999 | 0.07 |11,242,176 | 30 | 0.001 | 26 Hours 21 Min | 42.86% | 32.15% |

Next, we are experimenting by decreasing __Momentum__ from $0.999$ to $0.95$ and reducing ``queue_size`` from 65,536 to 32768. We will run this for 15 epochs. Results are:
| Ex. No. | Base Encoder | MLP Head | Queue Size | Mtm |Temp. | Trainable Params |Num Epochs | Learning Rate | Training Time (on unlabeled) | Accuracy on TrainingSet | Accuracy on TestSet|
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ----- | ------| -------| ----| --- |
| 9 | ResNet18  | False | 32,768 | 0.95 | 0.07 |11,242,176 | 15 | 0.001 | 13 Hours 26 Min| 42.65% | 31.95% |

Despite being used in the original MoCo paper, we were deliberately avoiding using transfomation of random resized scaling that first crops anywhere between 20-100% and resize them to image size again; but we will implement it now and see if results improve. Results, however, improved only a little (_as we ran it for 25 epochs with previous hyperparameter_)

| Ex. No. | Base Encoder | MLP Head | Queue Size | Mtm |Temp. | Trainable Params |Num Epochs | Learning Rate | Training Time (on unlabeled) | Accuracy on TrainingSet | Accuracy on TestSet|
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ----- | ------| -------| ----| --- |
| 10 | ResNet18  | False | 32,768 | 0.95 | 0.07 |11,242,176 | 25 | 0.001 | 19 Hours 15 Min| 55.9% | 33.89% |

So, we will try one more time without it again as we are using instead ``GaussianBlur``

| Ex. No. | Base Encoder | MLP Head | Queue Size | Mtm |Temp. | Trainable Params |Num Epochs | Learning Rate | Training Time (on unlabeled) | Accuracy on TrainingSet | Accuracy on TestSet|
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ----- | ------| -------| ----| --- |
| 11 | ResNet18  | False | 32,768 | 0.95 | 0.07 |11,242,176 | 25 | 0.001 | 20 Hours 23 Min| 39.6% | 29.06% |

Results instead perished.