# Torch

## LIST

1. CPUs and GPUs
2. Deeplearning Framework

## CPUs and GPUs

- CPU ： Central Processing Unit
  - Fewer cores but each cores is much faster and much more capable
  - Great at squential  tasks
- GPU :  Graphics Processing Unit
  - More cores,but each core is much slower and "dumber"
  - Great for parallel tasks,like mutipulation of matrix

![avatar](./L8_Pic1.png)

In deeplearning,we usaully use NVIDIA rather than AMD.

### Programming on GPUs
- CUDA(NVIDIA only)
  - Write C-like code that run directly on GPU
  - Higher level APIs:cuBLAS,cuFFT,cuDNN,etc
- OpenCL
  - Similer to CUDA,but runs on anything
  - Usually slower

### Disk
The model is stored in GPUs but the large data are stored in the disk.If uncarefully,the trainning can bottleneck on reading data and transfering to GPU.

Solution:
- Read all data into RAM
- Use SSD instead of HDD
- Use mutiple CPU threads to perfetch data.

## DeepLearning Framework

Points of learning frame:
  1. Easy to build big computational graphs
  2. Easy to compute gradients in computational graphs
  3. Run in efficiently on GPU

Main Framework

- Caffe 
- Torch --> Pytorch
- Theano --> TensorFlow

Numpy's Problem:
- Cannot work on GPUs
- Have to calulate the gradients with our own.

### Pytorch
- Three level of abstraction
  - Tensor:Imperative(重要的) ndarray but run on GPU
  - Variable:Node in a computational graph;stores data and gradient
  - Module:A neural network layer(nn.Module);may store state or learnable weights

- Coding step
  - Create random tensors for data and weights
  - Forward pass:compute the prediction and loss
  - Backward pass:manually compute gradients
  - Update the weight

To run on GPU,just casy tensors to a cuda data type:
```python
dtype = torch.cuda.FloatTensor
x = Variable(torch.randn(N,D_in).type(dtype))
# use tensor.type(dtype)
```

If gradients of some variable is needed,set required_grad = True. 
```python
require_grad = True
```

nn.Module is a liberay that contain the base class of the model,usually we redefine a model under it and overload the __init__ adn __forward__
```python
class MyModel(torch.nn.Module):
    def __init__(self,...):
      super(MyModel,self).__init__()
      """ Instantiation of Function from nn.Linear(),nn.ReLu,etc"""
    def __forward__(self,x)
      """Def forward function"""
      """return y_pred"""

# Instantiation
model = MyModel()
```

Optimizer and Loss can be instantiatied,like:

```python
criterion = torch.nn.MSELoss(size_average = False)

optimizer = torch.optim.Adam(
    model.parameters,
    lr = learning_rate
)
```

Dataloader is the API for batch the data,be-like.
```python
from torch.utils.data import TensorDataset,DataLoader

loader = DataLoader(TensorDataset(x,y),batch_size = 8)

...

"""In trainning"""
for epoch in range(trainning_time):
  for x_batch,y_batch in loader:
    """Trainning Details"""
```

Variable in torch.autograd has the same APIs with the torch.tensor,and easy to clean the code.
```python
from torch.autograd import Variable

x = Variable(torch.randn(N,D_in).type(dtype),required_grad = False)
```

torch.autograd.Function gives a change to overloading somefunction for the example of the ReLU():
```python
class ReLU(torch.autograd.Function):
  def forward(self,x):
    """Def forward pass details"""
    self.save_for_backward(x)
    y_pred = x.clamp(min = 0)
    return y_pred

  def backward(self,grad_y):
    x,=self.saved_tensors
    grad_input = grad_y.clone()
    grad_input[x<0] = 0
    return grad_input

```


### example
```python
import torch
from torch.autograd import Variable

dtype = torch.cuda.FloatTensor

N,D_in,H,Dout = 64,1000,100,10
learning_rate = 1e-6
learning_time = 500

x = Variable(torch.randn(N,D_in).type(dtype),required_grad = False)
y = Variable(torch.randn(N,D_out).type(dtype),required_grad = False)

model = torch.nn.Sequential(
    torch.nn.Linear(D_in,H),
    torch.nn.ReLU(),
    torch.nn.Linear(H,D_out)
)
loss_fn = torch.nn.MSELoss(size_average = False)
optimizer = torch.optim.Adam(
    model.parameters,
    lr = learning_rate
)

for t in range(learning_time)
    y_pred = model(x)
    loss = loss_fn(y_pred,y)

    optimizer.zero_grad
    loss.backward()

    optimizer.step()
```

To Define new Modules
```python
#0 import libary
import torch
from torch.autograd import Variable
from torch.utils.data import TensorDataset,DataLoader

#1 Overload the model
class TwoLayerNet(torch.nn.Module):
    def __init__(self,D_in,H,D_out):
      #Def the constructor
        #the Super.__init__must be use
        super(TwoLayerNet, self).__init__()
        #def the linear mapping
        self.linear1 = torch.nn.Linear(D_in,H)
        self.linear2 = torch.nn.Linear(H,D_out)
    def forward(self,x)：
      # Def the forward pass
        h_relu = self.linear1(x).clamp(min = 0)
        y_pred = self.linear2(h_relu)
        return y_pred

#2 setting the Macro
N,D_in,H,Dout = 64,1000,100,10
learning_rate = 1e-6
learning_time = 500

#3 Loading the train data
x = Variable(torch.randn(N,D_in).type(dtype),required_grad = False)
y = Variable(torch.randn(N,D_out).type(dtype),required_grad = False)

loader = DataLoader(TensorDataset(x,y),batch_size = 8)
# Divide the data to batches

#4 Instantiation
model = TwoLayerNet(D_in,H,D_out)
# Instantiation of the model

criterion = torch.nn.MSELoss(size_average = False)
# Instantiation of the loss function

optimizer = torch.optim.Adam(
    model.parameters,
    lr = learning_rate
)
# Instantiation of the optimizer


#5 start trainning
for epoch in range(learning_time):
    for x_batch,y_batch in loader:
        y_pred = model(x_batch)
        loss = criterion(y_pred,y_batch)

        optimizer.zero_grad()
        loss.backward()

        optimizer.step()
```

### Visdom
  Somewhat similar to TensorBoard:add logging to your code,then visuallized in browser