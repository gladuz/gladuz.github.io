---
title: Neural Network training from scratch in C++
#date: '2024-06-29'
cover:
  image: "mnist_ex.png"
  hidden: false
hideSummary: true
hideMeta: True
---

## Project Overview
[Project Repository](https://github.com/gladuz/nn-cpp)
Developed a simple neural network framework in C++ as a learning exercise to enhance C++ skills and deepen understanding of neural network fundamentals.

## Key Features
- Custom `Tensor` class for matrix operations
- PyTorch-like modular architecture (`Linear`, `ReLU`)
- Implementation of forward and backward passes
- Basic optimization using SGD

## Technical Highlights
1. **Tensor Operations**: 
   - Implemented a `Tensor` class for efficient matrix computations
   - Supported basic operations required for neural network computations

2. **Neural Network Modules**:
   - Created `Linear` and `ReLU` modules mimicking PyTorch's `torch.nn`
   - Implemented a `Sequential` container for easy model construction

3. **Training Loop**:
   - Developed forward pass, loss computation, and backward propagation
   - Implemented Stochastic Gradient Descent (SGD) optimizer

## Example Usage
```cpp
Tensor data({1, 2, 3, 4, 5, 6}, 2, 3);
Tensor target({2, -2}, 2, 1);

nn::Sequential seq;
seq.add(std::make_shared<nn::Linear>(3, 5));
seq.add(std::make_shared<nn::ReLU>());
seq.add(std::make_shared<nn::Linear>(5, 1));

auto optimizer = optim::SGD(seq.parameters(), 0.01);

for(int i=0; i<100; i++){
   auto out = seq.forward(data);
   auto [loss, loss_grad] = nn::functional::mse_loss(out, target);
   seq.backward(loss_grad);
   optimizer.step();
}
```

## Challenges Overcome
- Implementing efficient matrix operations in C++
- Managing memory and optimizing performance
- Designing a modular and extensible architecture

## Tools and Technologies
C++, Make, Git

## Future Plans
- Add dataloader functionality for MNIST dataset
- Implement additional layer types and activation functions
- Optimize performance for larger networks