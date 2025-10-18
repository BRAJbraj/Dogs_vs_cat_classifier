Dogs vs Cats Classifier — README

A simple Convolutional Neural Network (Keras) classifier for the classic Cats vs Dogs task.
This repo contains the training script, a saved model, and a lightweight evaluation routine.

❖ Project summary

Model type: Keras Sequential CNN (Conv2D → MaxPool → Dense → Sigmoid).

Task: Binary classification — cat vs dog images.

Saved model (included): dogs_and_cats_classifier_model_2(1).keras.

Training script (included): dogs_and_cats_classifier (Python script). 

Note: The included Keras model in this repository achieved ~78% accuracy on the provided challenge/test set when it was trained. Running the training script yourself may not reproduce that exact number because training is stochastic (random initialization, data augmentation via ImageDataGenerator, data shuffling, nondeterministic GPU ops, etc.).

❖ What’s included

dogs_and_cats_classifier — the main training & evaluation script (Python). 

efae6e70-6c22-4e41-9c2c-64d583d…

dogs_and_cats_classifier_model_2(1).keras — pre-trained Keras model (trained run, included for demo / inference). 

efae6e70-6c22-4e41-9c2c-64d583d…

The script handles dataset download/preparation (it downloads the cats-and-dogs archive during runtime).


❖ Quick results (from the included model)

Reported accuracy: 78% (model file included in repo). 

efae6e70-6c22-4e41-9c2c-64d583d…

Loss: The training script logs and plots training/validation loss curves during training. Exact numeric loss values vary per run (see Reproducibility).


❖ Requirements

Python 3.8+

TensorFlow 2.x

matplotlib, numpy (standard Python packages)

(Optional) GPU + CUDA for faster training


❖ How to run

Clone this repository and open a terminal in the repo root.

Run the script (it will download the dataset automatically — the script uses a dataset archive download inside):

# if the file has a .py extension
python dogs_and_cats_classifier.py

# or, if the script is executable and present without extension
python dogs_and_cats_classifier


The script will:

Download and prepare the dataset,

Create ImageDataGenerator instances for training/validation/test,

Train the CNN for the configured number of epochs,

Save the trained model as dogs_and_cats_classifier_model_2(1).keras,

Plot training/validation accuracy and loss,

Run the evaluation and print the test/challenge accuracy.

To run inference with the included saved model only (without retraining), load the .keras file with:

   from tensorflow.keras.models import load_model
   model = load_model('dogs_and_cats_classifier_model_2(1).keras')
   # then call model.predict(...) on preprocessed images

❖ Training details (as used in script)

Input size: 150 x 150 x 3

Batch size: 128 for datasets and 256 for training.

Epochs: 20

Optimizer: rmsprop

Loss: binary_crossentropy

Metric: accuracy

Data augmentation: rotation, width/height shift, shear, zoom, horizontal flip via ImageDataGenerator.


❖ Reproducibility — why your run may differ

The included model reached ~78% accuracy on the challenge set, but your training run may produce different numbers because of:

  Random weight initialization.

  Random data augmentation (ImageDataGenerator).

  Data shuffling and batch ordering.

  Non-deterministic GPU operations (cuDNN).

  Different TensorFlow / CUDA / driver versions.

To reduce variability, set seeds and control generator randomness before training:

 import os, random, numpy as np, tensorflow as tf
 os.environ['PYTHONHASHSEED'] = '0'
 random.seed(0)
 np.random.seed(0)
 tf.random.set_seed(0)

# pass a fixed seed to ImageDataGenerator.flow_from_directory(..., seed=123)


Also try:

shuffle=False or fixed seed in generators,

workers=1, use_multiprocessing=False in model.fit(...),

enabling TF determinism if supported by your TF version.

Even with these, exact parity with the included .keras artifact may be difficult: the saved model is provided as the reproducible artifact of the original run. 





❖ Troubleshooting

If dataset download fails, download the cats_and_dogs archive manually and extract to the expected train/, validation/, test/ structure.

If GPU errors occur, try CPU-only or fix CUDA/driver compatibility.

For inference errors, ensure image preprocessing (resize / scaling) matches the preprocessing used during training.

❖ Acknowledgements

The training script was adapted from a Colab export. See the script for the complete training pipeline and model architecture. 


❖ License & Contact

This repository is for educational/demo purposes. Use responsibly.

If you want help reproducing the 78% run or want an inference-only notebook, open an issue or contact me and I’ll add a ready-to-run example.

