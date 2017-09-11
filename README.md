<font size=4><b>Reproduced ResNet on CIFAR-10 and CIFAR-100 dataset.</b></font>

<b>Dataset:</b>

https://www.cs.toronto.edu/~kriz/cifar.html

<b>Related papers:</b>

Identity Mappings in Deep Residual Networks

https://arxiv.org/pdf/1603.05027v2.pdf

Deep Residual Learning for Image Recognition

https://arxiv.org/pdf/1512.03385v1.pdf

Wide Residual Networks

https://arxiv.org/pdf/1605.07146v1.pdf

<b>Settings:</b>

* Random split 50k training set into 45k/5k train/eval split.
* Pad to 36x36 and random crop. Horizontal flip. Per-image whitening.
* Momentum optimizer 0.9.
* Learning rate schedule: 0.1 (40k), 0.01 (60k), 0.001 (>60k).
* L2 weight decay: 0.002.
* Batch size: 128. (28-10 wide and 1001 layer bottleneck use 64)
* LAMBDA: 0. (for center loss weight)
* ALPHA: 0.5 (center loss learning rate)

<b>Prerequisite:</b>

1. Install TensorFlow, Bazel.

2. Download CIFAR-10/CIFAR-100 dataset.

```shell
curl -o cifar-10-binary.tar.gz https://www.cs.toronto.edu/~kriz/cifar-10-binary.tar.gz
curl -o cifar-100-binary.tar.gz https://www.cs.toronto.edu/~kriz/cifar-100-binary.tar.gz
```

<b>How to run:</b>

```shell
# cd to the models repository and run with bash. Expected command output shown.
# The directory should contain an empty WORKSPACE file, the resnet code, and the cifar10 dataset.
# Note: The user can split 5k from train set for eval set.
$ ls -R
.:
cifar10  resnet  WORKSPACE

./cifar10:
data_batch_1.bin  data_batch_2.bin  data_batch_3.bin  data_batch_4.bin
data_batch_5.bin  test_batch.bin

./resnet:
BUILD  cifar_input.py  g3doc  README.md  resnet_main.py  resnet_model.py

# Build everything for GPU.
$ bazel build -c opt --config=cuda resnet/...

# Train the model.
$ bazel-bin/resnet/resnet_main --data_path=cifar10/ 
                               --result_path=RESULT_DIR 
                               --LAMBDA=LAMBDA_VALUE 
                               --ALPHA=ALPHA_VALUE
                               --weight_decay=WEIGHT_DECAY_VALUE
```
