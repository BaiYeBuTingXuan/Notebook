# Loss and Optimizer

## Outline

1. Loss Function
2. Loss Calculating Algorithm:SVM,Softmax
3. Optimization:Gradient Descent
4. Gradient Descent Method
5. Mini-Batch Idea
6. Featuring before Learning


## Loss Function

  Loss Funtion is designed to measure the unaccuracy of the Classifier.

    "To quantify how bad are different mistakes"

## Loss Calculating Algorithm

General Formalism:$$Loss = \Sigma Loss_{i}(f(input;Weight);fact)$$

- SVM: 
$$Loss = \Sigma_{i \neq j} \max\{0, s_i - s_j + 1\}$$       

  note: $ i \neq j$ is necessary, or the Loss for a perfect answer is result in 1 not 0.

- Regularization(惩罚) Term: to pick a simpler $\mathbb{W}$
    $$Full\_Loss = \Sigma Loss_{i}+ \lambda R(Weight)$$

    $\lambda$ called regularization strengh,is a hyporparameter to balance the two terms.

        Occam's Razor:"Among competing hypotheses, the simplest is the best."

    Common use:
      - L2 regularization : $R(W) = \Sigma_i \Sigma_j W^{2}_{i,j}$
        ''Average the Weight!''
      - L1 regularization : $R(W) = \Sigma_i \Sigma_j |W_{i,j}|$ 
        "More zeros in Weight!"
      - Elastic net(L1+L2) : $R(W) = \Sigma_i \Sigma_j \beta|W_i,j|+W^{2}_{i,j}$ 
      - Max Norm Regularization
        ''Average each column,not each item''
      - Dropout 

- SoftMax Loss:Mutinomial Logistic Regression(多元逻辑斯蒂回归模型) or named Cross-Entropy Loss
    It give the scores with some unique meaning:
    
        scores = Unnomaized Log Probability of the Classes
    From Logistic Regression
   $$ P(Y = k| X = x_i) = \frac{e^{s_k}}{\Sigma_j e^{s_j}} $$ where s is the score = f(x;W)
    Then define the Loss be_like:
   $$Loss_i = - log P(Y = y_i| X = x_i)$$
   $$Loss = \Sigma_i Loss_i$$

   Idea: Score -(exp)->-(normalize)->-(-log)->Loss

## Optimization:Gradient Descent
  Method: Caculating the Gradients.
    - Numeric Gradients: Approximate,Slow,easy to write.
    - Analytic(Derived from the expression) Gradient: Exact,Fast,Erro-prone(容易出错)
    - $\nabla_{Weight}$ is an linear opeater,so that：
    $$\nabla_{Weight} Full\_Loss = \Sigma_i \nabla_{Weight}Loss_{i}(f(x_i,W);y_i)+ \lambda \nabla_{Weight} R(Weight)$$

## Gradient Descent Method:
  $$  Weight += - \nabla Weight * Learning\_rate $$
  in Python Code:
  
  ```python
  #Train :
  while True:
    Weight_grad = Grad(Loss_func,data,Weight)
    step = -Learning_Rate*Weight_grad
    Weight += step
  ```

  then the parameter sheet $Weight$ go close to the Steady Point step by step.
    
  Q:Will it go to a local min-value but not a global min value？？

  Ans：Yes,so that we search for more path like convex optimization,Adam,momentum-motivation and so on.

## Mini-Batch Idea
  To improve the speed of the learning or precisely the update rate of the $Weight$ for a millions of data base, we can pack the train data as a Batch randomly,like:

  ```python
  #Train :
  while True:
    # batch the data:
    data_batch = sample_train_data(data,BATCH_SIZE)
    # Use the batch to guide the grad calulate
    Weight_grad = Grad(Loss_func,data_batch,Weight)
    step = -Learning_Rate*Weight_grad
    Weight += step
  ```

  In addition,BACTH_SIZE is often the power of 2,like 2,4,16,256.

## Featuring before Learning
  提取图像中的一些特征信息，根据这些信息进行学习，而不是根据完整的信息。因为心理学研究标明图像信息大多是无用的。在图像识别中，有用的一般是图像的有向边缘向量。

  卷积神经网络以及深度学习与特征提取没有太大的差别，前者是通过学习的方法认知特征，而不是先人工进行特征的提取。

  Featuring the information and learing with the results of it,but no the total infomation,for most of them is useless,according to some study of psycology. In Image Reconization,the egde arrows is the effective information as known.

  CNN and DL seem similar to Featuring,but the former is featuring by learning,not by people. 