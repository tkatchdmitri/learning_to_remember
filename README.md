# Zig-Zag MNIST
What if you take two batches A and B, and alternative between training on them 100 times? ABABABAB.. (100 times). A 1.33% improvement over baseline in the first epoch of MNIST after only seeing 120 unique batches (batch_size = 1000 with 2 data loaders each with 60 batches). Baseline code based on official PyTorch examples. This may allow for faster training when data loading is expensive. Note that complete uniqueness of data between A and B has not been guaranteed in the implementation.

with zigzagging:
```py
python3.10 mnist.py
Train Epoch: 1 [0/60000 (0%)]   Loss: 0.009286
Train Epoch: 1 [10000/60000 (17%)]      Loss: 0.000468
Train Epoch: 1 [20000/60000 (33%)]      Loss: 0.000509
Train Epoch: 1 [30000/60000 (50%)]      Loss: 0.000392
Train Epoch: 1 [40000/60000 (67%)]      Loss: 0.002520
Train Epoch: 1 [50000/60000 (83%)]      Loss: 0.001325

Test set: Average loss: 0.0545, Accuracy: 9922/10000 (99%)
```

without zigzagging:
```py
python3.10 mnist.py
Train Epoch: 1 [0/60000 (0%)]   Loss: 2.187327
Train Epoch: 1 [10000/60000 (17%)]      Loss: 0.442967
Train Epoch: 1 [20000/60000 (33%)]      Loss: 0.213033
Train Epoch: 1 [30000/60000 (50%)]      Loss: 0.181745
Train Epoch: 1 [40000/60000 (67%)]      Loss: 0.145299
Train Epoch: 1 [50000/60000 (83%)]      Loss: 0.139771

Test set: Average loss: 0.0645, Accuracy: 9789/10000 (98%)
```

