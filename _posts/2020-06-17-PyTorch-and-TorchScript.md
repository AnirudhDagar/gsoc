---
layout: post
description:
categories: [tmva, pytorch, torchscript]
title: PyTorch & TorchScript
image: images/PyTorch.jpg
search_exclude: true
comments: true
---

Though there are numerous tutorials on PyTorch out there, I wanted to compose a little post for individuals who may wind up utilizing the TMVA PyTorch Interface. I also want to draw some contrasts, defining distinctions between Keras and PyTorch interfaces in TMVA.

PyTorch is a Python-based scientific computing package supporting a​utomatic differentiation.​ An ​open source machine learning​ framework that accelerates the path from research prototyping to production deployment. ​A research platform that provides maximum flexibility​ and s​ peed.

<img alt="Can't See? Something went wrong!" src="{{site.baseurl}}/images/Keras-or-PyTorch.png" width="80%">

Both the frameworks are really useful in their own ways. For your daily machine learning models, you can achieve similar results with any of the two.

Keras shines especially with it's super simple high-level tensorflow API. It may be easier to get into and experiment with standard layers. If your work involves some simple experiments, Keras maybe the goto framework due to its plug & play spirit. 

But, things get interesting when one requires low level control and flexibility. That's when the argument for Keras starts losing water. PyTorch on the other hand is amazing in terms of control, flexibility and raw power that it can provide to the user. PyTorch offers a lower-level approach for the more mathematically-inclined users.

PyTorch is widely used​ among researchers and hence has a large community around it continuously pushing the state of the art developements and advancements into the framework.

**Need for PyTorch Interface?**

● Allows to combine ROOT, which is good at handling HEP data and PyTorch which can be used for Deep Learning needs.

● **Power & Flexibility**: ​Not easy to develop using TMVA, as they require complex configuration strings. Even with PyKeras Interface, designing custom layers is not feasible. PyTorch offers the power and flexibility to achieve complex models with custom layers, optimizers, loss functions and training methodologies.

● **Ease of Debugging**: ​PyTorch models make use of dynamic computation graphs and are based on eager execution. This makes it easier to use debugging tools like pdb.

● **Performance**: PyTorch performs much better than Keras due to it's highly optimized C++ backend.


### Defining Models in PyTorch

Below I'm sharing a few options which can be employed to define models in pytorch based on the requirement. As mentioned earlier the customizations possible here are limited to your imagination.

```python
import torch

# Implemented using the torch.nn.Module
class ​TwoLayerNet​(torch.nn.Module):
	def ​__init__​(self, D_in, H, D_out):
	​"""
	In the constructor we instantiate two nn.Linear
	modules and assign them as member variables.
	"""
	super(TwoLayerNet, self).__init__()
	self.linear1 = torch.nn.Linear(D_in, H)
	self.linear2 = torch.nn.Linear(H, D_out)

	def ​forward​(self, x):
	​"""
	In the forward function we accept a tensor of input data
	and we must return a tensor of output data. We can use
	modules defined in the constructor as well as arbitrary
	operators on tensors.
	"""
	h_relu = self.linear1(x).clamp(min=​0​)
	y_pred = self.linear2(h_relu)
	
	return y_pred


# N is batch size; D_in is input dimension;
# H is hidden dimension; D_out is output dimension.
N, D_in, H, D_out = 64, 1000, 100, 10


# Construct our model by instantiating the class defined above
model = TwoLayerNet(D_in, H, D_out)

##########
# OR
##########

# Similar to Keras
# Implemented using the torch.nn.Sequential Module
model = torch.nn.Sequqntial(
	torch.nn.Linear(D_in, H),
	torch.nn.ReLU(),
	torch.nn.Linear(H, D_out)
	)

##########
# OR
##########

# One can choose to implement using ModuleList/ModuleDict
# See the PyTorch Documentation for more details on containers.
```


Anyways to develop our interface we need the ability to store a defined model into a file which can be later loaded into the TMVA C++ backend when Booking and Training the models.

While `model.save()` in keras saves the whole model in `.hdf5` file along with other information, in PyTorch, one can achieve similar functionality using `torch.save()`. The issue here is that before we load back the model into memory from the saved file, it requires you to define the model class again. `torch.save` inputs a dictionary to save only the model parameters and other learnable parameters (optimizers state etc.) or variables like `current_epoch`. Though that is neat, we require our model to be completely stored in a file which can be later used without any knowledge of model definition.

**Enter torchscript!**

TorchScript is a way to create serializable and optimizable models from PyTorch code. Any TorchScript program can be saved from a Python process and loaded in a process where there is no Python dependency.

PyTorch provides tools to incrementally transition a model from a pure Python program to a TorchScript program that can be run independently from Python, such as in a standalone C++ program. This makes it possible to train models in PyTorch using familiar tools in Python and then export the model via TorchScript to different environment where Python programs may be disadvantageous.

Though torchscript, we can serialize and save the model which can be later loaded into the TMVA c++ backend from a stand-alone `.pt` file.

Anyways, this was supposed to be a short post on torch and torchscript.
I'll be ending this now. There will be a more detailed post discussing the details of the internals and design choices made for the TMVA PyTorch Interface.

Bbye for now!

