# Recurrent Neutral Network

## List
1. RNN
2. Backforward
3. Vanilla RNN Gradient Flow
4. LSTM
5. Summary for RNN

## Rucurent Neutral Network
1. Traditional Neutral Network:One to One
2. Process Sequences
   1. One to many
   2. many to One
   3. many to many

Idea:A hidden state in the computional graph.The input upgrades the hidden state as while as produces a output.

$$State_t = f_W(State_{t-1},input)$$

For example:
$$h_t = tanh((W_{hh} \ W_{xh})(\begin{array}{c}
   h_{t-1} \\ x_t
\end{array} ))$$$$Output = W_{hy}h_t$$
where $W_{hh}$ $W_{xh}$ $W_{hy}$is the parameters.

The hidden layer is the ''memory'' of the model
![avatar](./L10_Pic1.png)

## Backforward

- Traditional method:Wasting time and occupying more memory.
- Truncated Backpropagation through time
    
    Run forward and backward through chunks of the sequence instead of whole sequences.

## Vanilla RNN Gradient Flow
Problem: Computing gradient of $h_0$,the initial hidden state of the RNN,involve many factors of W.

- If the Largest singular value(奇异值)>1:Exploding the gradients
- If the Largest singular value<1:Vaninshing the gradients

Solution for Explosion:Gradiant clipping:Scale gradient if its norm is too big
```python
grad_norm = np.sum(grad * grad)
if grad_norm > threhold:
   grad *= threhold / grad_norm
```

Solution for Vanishment:Change RNN architecture(more complicated),like LSTM(Long Short Term Memory)

## LSTM
To solve the problem of Exploding Gradients and Vanishing Gradients.

$$\left(\begin{array}{c}i \\ f \\ o \\ g \end{array}\right) = \left(\begin{array}{c} \sigma \\ \sigma \\ \sigma \\ tanh \end{array}\right)W\left(\begin{array}{c}h_{t-1}\\x_t \end{array}\right)$$
$$c_t = f\odot c_{t-1}+i\odot g$$$$h_t = o \odot tanh(c_t)$$
where the$\odot$means mutipling elementally.
- 2 hidden state:$h_t$and $c_t$
- $i,f,o,g$:$i$:input gate(whether to write to cell);$f$:forget gate(whether to eraser cell);$o$:Output gate(How much to reveal cell);$g$:Gate gate(How much to write to cell)
![avatar](./L10_Pic2.png)
A gradient super highway
![avatar](./L10_Pic3.png)

## Summary
- RNNs allow a lot of flexibility in architecture design
![avatar](./L10_Pic4.png)
- Vanilla RNNs are simple but don't work very well
- Common to use LSTM or GRU：their additive interaction improve gradient flow
- Backward flow of gradients in RNN can explode or vanish.Exploding is controlled with gradient clipping.Vanishing is controlled with additive interactions(LSTM)
- Better/simpler architectures are a hot topic of current research
- Better understanding(both theoretical and empirical)



