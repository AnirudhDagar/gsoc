---
toc: false
layout: post
description: Structural working of the TMVA PyTorch Interface
categories: [tmva, pytorch, torchscript]
title: "Internals: TMVA PyTorch Interface"
image: images/who-handles-what.png
search_exclude: true
comments: true
---

<img alt="Can't See? Something went wrong!" src="{{site.baseurl}}/images/who-handles-what.png">


### TMVA PyTorch Interface Structure

### How does it interface?

## Train


```C
 void MethodPyTorch::Train() {
  
	/* Setup parameters

	* Setup training alternatives to callbacks like keras

	* Store trained model to file (only if option 'SaveBestOnly' is NOT activated,
	* as we do not want to override the best model checkpoint)

	* Load PyTorch (torchscript) model from checkpoint .tar file or .pt/.pth file
	* Load initial model or already trained model

	* Start model training

	*/
 
}
```

## SetupTorchModel

```C
void MethodPyTorch::SetupTorchModel(bool loadTrainedModel) {
	// Load initial model or already trained model
	  
	/* Load pytorch model from file

	* Load pytorch user code methods
	         * Optimizer
	         * Loss Criterion
	         * Train Method
	         * Predict Method

	* load_model_custom_objects = {"optimizer": optimizer, "criterion":
	                criterion, "train_func": fit, "predict_func": predict}

	* Init variables and weights

	*/
}
```


### Function Loading

### Callbacks

### Model Saving Loading 



## Use of custom Python functions in TMVA

