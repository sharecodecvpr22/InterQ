# Code for InterQ

## Requirements

Python >= 3.7.10

Pytorch == 1.7.1

## Reproduce results

### Stage1: Generate data.



```
cd data_generate
```

Please install all required package in requirements.txt.

"--save_path_head" in *.sh is the path where you want to save your generated data pickle.

For cifar10/100
```
bash run_generate_cifar10.sh
bash run_generate_cifar100.sh
```

For ImageNet

"--model" in *.sh is the pre-trained model you want (also is the quantized model). 
You can use resnet18/mobilenet_w1/mobilenetv2_w1.
```
bash run_generate.sh
```

### Stage2: Train the quantized network

```
cd InterQ
```

1. Modify "qw" and "qa" in *.hocon to select desired bit-width.

2. Modify the "Path_to_data_pickle" in main_direct.py (line 122 and line 135) to the data_path and label_path you just generate from Stage1.

3. Use the below commands to train the quantized network. Pleas noted that the model that generates the data and the quantized model should be the same.


For cifar10/100
```
python main_direct.py --model_name resnet20_cifar10 --conf_path cifar10_resnet20.hocon --id=0

python main_direct.py --model_name resnet20_cifar100 --conf_path cifar100_resnet20.hocon --id=0
```

For ImageNet, you can choose the model by modifying "--model_name" (resnet18/mobilenet_w1/mobilenetv2_w1)
```
python main_direct.py --model_name resnet18 --conf_path imagenet.hocon --id=0
```


## Evaluate pre-trained models

The pre-trained models can be downloaded in [here]() 

Pleas make sure the "qw" and "qa" in *.hocon, *hocon, model_name and model_path are correct.

```
python test.py --model_name resnet18/mobilenet_w1/mobilenetv2_w1 --model_path path_to_pre-trained model --conf_path path_to_*.hocon
```