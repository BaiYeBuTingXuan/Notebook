# Convolutional Neutral Network

## Outline

1. Convolutional Layer
2. Pooling Layer
3. Max Pooling


## Convolutional Layer
To deal with a picture of 32 * 32 * 3
Traditional:Strech it to a vector of 3072 * 1.

Convolutional:
   - maintain the shape of the input,which is a 32 * 32 * 3 (W* H * D)Cube.
   - choose a Fliter(卷积核) $\omega$,which is smaller than the input but always expend the whole depth of the input volume.For example,5 * 5 * 3.
   -Convolution between fliter and the sample:$$\omega^T x +b$$
    which is the dot production of the Fliter and the Overlapping regions. In this case,mean 5 * 5 * 3 = 75 times calculation.
   - Sliding,then get the output of 28 * 28 * 1 in this case , called activation map.
   - Use other fliter,then get a set of activation maps. In this case,if the stride(步长) = 2,it might be 28 * 28 * 6.$$Output\_Size = (N - F)/stride +1$$
    stride of 1 or 2 is often used,and in pratice,common to zero pad the border.
   - Repeat above,get a sequence of Convolutional Layers, called ConvNet.

    32 * 32 * 3 -->28 * 28 * 6-->24 * 24 * 10 --...-->LC(FC)

To interpret this method,we consider a fliter featuring a kind of feature of the image,a layer contains various level of features. In the end of the ConvNet,we use Lineanr Classifier to distinguilish them.

As mentioned above, we should consider 3 hyperparameter:
    - Stride
    - Fliter Size
    - Pooling method

In pratice, a ConvNet often has:
   - Conv Layer
   - ReLU(UnLinear) Layer
   - Pooling Layer 

Typically Architetures:

    [(Conv-ReLU)*N-Pool]*M-(FC-ReLU)*K,SoftMax

where N is usually up to 5, M is large, and k between 0 and 2.

## Pooling Layer
Aim:
- Make the represantations smaller and more magageable
- operates over each activation map more independently.
  
  For example : 224 * 224 * 64 --> 112 * 112 * 64

Common doing:
- Depth is not change.
- No overlapping.

### Max Pooling
Only feature the max value of each block,for example:
    $$\begin{bmatrix}
    1& 1& 2& 4\\
    5 & 6& 7 & 8\\
    3 & 2& 1 & 0\\
    1 & 2 &3 &4
    \end{bmatrix}
    --->
    \begin{bmatrix}
     6  &  8\\
    3  & 4  
    \end{bmatrix}
    $$

which with fliter of 2 * 2 and stride 2.
