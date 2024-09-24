# How-To: Setup Pytorch

Pytorch is a package for deep learning with optional support for GPUs.
You can find the original Pytorch installation instructions [here](https://pytorch.org/get-started/locally/).

This article describes how to set up Pytorch with GPU support using Conda.

## Install pytorch environment

```terminal
$ module load miniconda3
$ conda create -n pytorch python=3.12
$ source activate pytorch
$ conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

Enter the gpu environment.<br>
It assumes that you have permission to access the `sfscai` partition.


```terminal
[hpc@hpclogin ~]$ srun -p sfscai -N 1 -n 2 --gres=gpu:1 --pty /bin/bash
[hpc@gpu190 ~]$ module load miniconda3
[hpc@gpu190 ~]$ source activate pytorch
(pytorch) [hpc@gpu190 ~]$ python
>>> import torch
>>> print(torch.__version__)
2.4.0+cu118
>>>
```

We thus end up with an installation of Python 3.12 with pytorch 2.4.0.

## Run Pytorch Example

Let us now see whether Pytorch has recognized our GPU correctly.

```terminal
>>> import torch
>>> print(torch.__version__)
2.4.0+cu118
>>> if torch.cuda.is_available():
...     print("GPU is available")
...     print(f"Number of available GPUs: {torch.cuda.device_count()}")
...     for i in range(torch.cuda.device_count()):
...         print(f"GPU {i}: {torch.cuda.get_device_name(i)}")
... else:
...     print("No GPU available")
... 
GPU is available
Number of available GPUs: 1
GPU 0: NVIDIA L20
```

Yay, we can proceed to run the [Quickstart Tutorial](https://pytorch.org/tutorials/beginner/basics/quickstart_tutorial.html).

## Writing Pytorch Slurm Jobs

Writing Slurm jobs using Pytorch is as easy as creating the following scripts.

`pytorch_script.py`

```python
import torch
from torch import nn
from torch.utils.data import DataLoader
from torchvision import datasets
from torchvision.transforms import ToTensor

# Download training data from open datasets.
training_data = datasets.FashionMNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor(),
)

# Download test data from open datasets.
test_data = datasets.FashionMNIST(
    root="data",
    train=False,
    download=True,
    transform=ToTensor(),
)

batch_size = 64

# Create data loaders.
train_dataloader = DataLoader(training_data, batch_size=batch_size)
test_dataloader = DataLoader(test_data, batch_size=batch_size)

for X, y in test_dataloader:
    print(f"Shape of X [N, C, H, W]: {X.shape}")
    print(f"Shape of y: {y.shape} {y.dtype}")
    break

# Get cpu, gpu or mps device for training.
device = (
    "cuda"
    if torch.cuda.is_available()
    else "mps"
    if torch.backends.mps.is_available()
    else "cpu"
)
print(f"Using {device} device")

# Define model
class NeuralNetwork(nn.Module):
    def __init__(self):
        super().__init__()
        self.flatten = nn.Flatten()
        self.linear_relu_stack = nn.Sequential(
            nn.Linear(28*28, 512),
            nn.ReLU(),
            nn.Linear(512, 512),
            nn.ReLU(),
            nn.Linear(512, 10)
        )

    def forward(self, x):
        x = self.flatten(x)
        logits = self.linear_relu_stack(x)
        return logits

model = NeuralNetwork().to(device)
print(model)

loss_fn = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model.parameters(), lr=1e-3)

def train(dataloader, model, loss_fn, optimizer):
    size = len(dataloader.dataset)
    model.train()
    for batch, (X, y) in enumerate(dataloader):
        X, y = X.to(device), y.to(device)

        # Compute prediction error
        pred = model(X)
        loss = loss_fn(pred, y)

        # Backpropagation
        loss.backward()
        optimizer.step()
        optimizer.zero_grad()

        if batch % 100 == 0:
            loss, current = loss.item(), (batch + 1) * len(X)
            print(f"loss: {loss:>7f}  [{current:>5d}/{size:>5d}]")

def test(dataloader, model, loss_fn):
    size = len(dataloader.dataset)
    num_batches = len(dataloader)
    model.eval()
    test_loss, correct = 0, 0
    with torch.no_grad():
        for X, y in dataloader:
            X, y = X.to(device), y.to(device)
            pred = model(X)
            test_loss += loss_fn(pred, y).item()
            correct += (pred.argmax(1) == y).type(torch.float).sum().item()
    test_loss /= num_batches
    correct /= size
    print(f"Test Error: \n Accuracy: {(100*correct):>0.1f}%, Avg loss: {test_loss:>8f} \n")

epochs = 5
for t in range(epochs):
    print(f"Epoch {t+1}\n-------------------------------")
    train(train_dataloader, model, loss_fn, optimizer)
    test(test_dataloader, model, loss_fn)
print("Done!")
```

`pytorch_job.sh`

```bash
#!/usr/bin/bash

#SBATCH --job-name=pytorch-job
#SBATCH -N 1
#SBATCH --ntasks-per-node=6
#SBATCH --partition=sfscai
#SBATCH --gres=gpu:1
#SBATCH --output=%j.out
#SBATCH --error=%j.err

module load miniconda3
source activate pytorch
python pytorch_script.py
```

And then calling

```terminal
$ sbatch pytorch_job.sh
```

You can find the reuslts in `xxx.out` after completion.

```
$ cat xxx.out 
Epoch 1
-------------------------------
loss: 2.303494  [   64/60000]
loss: 2.294637  [ 6464/60000]
loss: 2.277102  [12864/60000]
loss: 2.269977  [19264/60000]
loss: 2.254235  [25664/60000]
loss: 2.237146  [32064/60000]
loss: 2.231055  [38464/60000]
loss: 2.205037  [44864/60000]
loss: 2.203240  [51264/60000]
loss: 2.170889  [57664/60000]
Test Error:
 Accuracy: 53.9%, Avg loss: 2.168588

Epoch 2
-------------------------------
loss: 2.177787  [   64/60000]
loss: 2.168083  [ 6464/60000]
loss: 2.114910  [12864/60000]
loss: 2.130412  [19264/60000]
loss: 2.087473  [25664/60000]
loss: 2.039670  [32064/60000]
loss: 2.054274  [38464/60000]
loss: 1.985457  [44864/60000]
loss: 1.996023  [51264/60000]
loss: 1.917241  [57664/60000]
Test Error:
 Accuracy: 60.2%, Avg loss: 1.920374

Epoch 3
-------------------------------
loss: 1.951705  [   64/60000]
loss: 1.919516  [ 6464/60000]
loss: 1.808730  [12864/60000]
loss: 1.846550  [19264/60000]
loss: 1.740618  [25664/60000]
loss: 1.698733  [32064/60000]
loss: 1.708889  [38464/60000]
loss: 1.614436  [44864/60000]
loss: 1.646475  [51264/60000]
loss: 1.524308  [57664/60000]
Test Error:
 Accuracy: 61.4%, Avg loss: 1.547092

Epoch 4
-------------------------------
loss: 1.612695  [   64/60000]
loss: 1.570870  [ 6464/60000]
loss: 1.424730  [12864/60000]
loss: 1.489542  [19264/60000]
loss: 1.367256  [25664/60000]
loss: 1.373464  [32064/60000]
loss: 1.376744  [38464/60000]
loss: 1.304962  [44864/60000]
loss: 1.347154  [51264/60000]
loss: 1.230661  [57664/60000]
Test Error:
 Accuracy: 62.7%, Avg loss: 1.260891

Epoch 5
-------------------------------
loss: 1.337803  [   64/60000]
loss: 1.313278  [ 6464/60000]
loss: 1.151837  [12864/60000]
loss: 1.252142  [19264/60000]
loss: 1.123048  [25664/60000]
loss: 1.159531  [32064/60000]
loss: 1.175011  [38464/60000]
loss: 1.115554  [44864/60000]
loss: 1.160974  [51264/60000]
loss: 1.062730  [57664/60000]
Test Error:
 Accuracy: 64.6%, Avg loss: 1.087374

Done!
```
