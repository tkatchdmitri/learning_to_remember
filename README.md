# Learning To Remember

What if you taught a neural network how to remember before teaching it arbitrary things? 

The initial configuration of a neural network is not guaranteed to be optimal for remembering. By arriving at zero loss while alternating between batches A and B, we conjecture that the parameter configuration of the network becomes initialized  to a state that is better suited for remembering new data and internal states.

## Zig-Zag MNIST
What if you take two batches A and B, and alternative between training on them 100 times? ABABABAB.. (100 times). A 1.33% improvement over baseline in the first epoch of MNIST after only seeing 120 unique batches (batch_size = 1000 with 2 data loaders each with 60 batches). Baseline code based on official PyTorch examples. 

This may allow for faster training when data loading is expensive. Note that complete uniqueness of data between A and B has not been guaranteed in the implementation. This also shows a maybe counterintuitive result. We do not overfit by alternating in this way. On the contrary, this suggests the loss landscape changes enough when you step that you can afford to revisit the same batch 100 times and still see nonzero loss.

## Results

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

## Code

```py
    model.train()
    for batch_idx, pair in enumerate(zip(train_loader, train_loader2)):
        (data, target), (data2, target2) = pair
        data, target = data.to(device), target.to(device)
        data2, target2 = data2.to(device), target2.to(device)
        for _ in range(100):
            optimizer.zero_grad()
            output = model(data)
            loss = F.nll_loss(output, target)
            loss.backward()
            optimizer.step()
            optimizer.zero_grad()
            output = model(data2)
            loss = F.nll_loss(output, target2)
            loss.backward()
            optimizer.step()
```
