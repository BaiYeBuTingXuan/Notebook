# Backpropagation And Neural Network

## Outline

1. Backpropagation
2. Sigmoid Function
3. Vectorized Operation
4. Activation Function
5. Full-connected Layer


## Backpropagation

  For those Mutilayer,complex neurual model and loss function,it is difficult to
  calculate the Analytic Gradients manually.

  Backporpagetion solves this problem by take the complex calculation into pieces as 
  set of addition and multipition.With chain rulex of gradient,compute from result to the cause.

## Sigmoid Function
The Function is usually used:
$$ \sigma(x) = \frac{1}{1+\exp(-x)} $$
whose gradients occasionally is:
$$\frac{d\sigma(x)}{dx} = \sigma(x)(1-\sigma(x))$$ 

## Vectorized Operation
  A node with input and output of 4096 elements,
  $$f((x_1,x_2,...x_{4096})^T) = (y_1,y_2,...y_{4096})^T$$ 
  it function is an mutinomial function work on vection space,while its gradient is the Jacobian matrix.
    $$ \frac{\partial(y_1,y_2,...y_{4096})}{\partial(x_1,x_2,...x_{4096})} = 
    \begin{bmatrix}
    \frac{\partial y_1}{\partial x_1} & \frac{\partial y_2}{\partial x_1} &\dots & \frac{\partial y_n}{\partial x_1}\\
    \vdots &\vdots & \ddots  & \vdots\\
    \frac{\partial y_1}{\partial x_{4096}} &\frac{\partial y_1}{\partial x_{4096}} &\cdots &\frac{\partial y_{4096}}{\partial x_{4096}}
    \end{bmatrix}_{n \times n}
$$

## Activation Function
  Common use:
    - Sigmoid: $\sigma(x) = \frac{1}{1+\exp(x)}$
    - tanh: $\tanh(x)$
    - ReLU: $\max\{0,x\}$
    - Leaky(漏) ReLU：$max\{\beta x,x\},\beta\in(0,1)$
    - Maxout:$\max\{\omega^T_1x+b_1,\omega^T_2x+b_2\}$
    - ELU:$\begin{cases}
            0& {x \geq 0}\\
            1& \text{x<0}
            \end{cases}$$
            $

## Full-connected Layer
  Full-connected Layer links each node of this layer and last layer.Due to the fully connected,it has the most parameter.