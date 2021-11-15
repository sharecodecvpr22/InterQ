# Code for under review (CVPR2022) paper: InterQ

## Requirements

Python >= 3.7.10

Pytorch == 1.7.1

## Reproduce results

### Stage1: Generate data.



```
cd data_generate
```

Please install all required package in requirements.txt.

"--save_path_head" in run_generate.sh/run_generate_cifar10.sh/run_generate_cifar100.sh is the path where you want to save your generated data pickle.

For cifar10/100
```
bash run_generate_cifar10.sh
bash run_generate_cifar100.sh
```

For ImageNet

"--model" in run_generate.sh/run_generate_cifar10.sh/run_generate_cifar100.sh is the pre-trained model you want (also is the quantized model). 
You can use resnet18/mobilenet_w1/mobilenetv2_w1.
```
bash run_generate.sh
```

### Stage2: Train the quantized network

```
cd InterQ
```

1. Modify "qw" and "qa" in cifar10_resnet20.hocon/cifar100_resnet20.hocon/imagenet.hocon to select desired bit-width.

2. Modify "dataPath" in cifar10_resnet20.hocon/cifar100_resnet20.hocon/imagenet.hocon to the dataset path.

3. Modify the "Path_to_data_pickle" in main_direct.py (line 122 and line 135) to the data_path and label_path you just generate from Stage1.

4. Use the below commands to train the quantized network. Pleas noted that the model that generates the data and the quantized model should be the same.


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

The pre-trained models and corresponding logs can be downloaded in [here](https://drive.google.com/drive/folders/1wk0WNxHhJiUky2ymEYJBg4o6oXLM15e4?usp=sharing) 

Pleas make sure the "qw" and "qa" in *.hocon, *.hocon, "--model_name" and "--model_path" are correct.

For cifar10/100
```
python test.py --model_name resnet20_cifar10 --model_path path_to_pre-trained model --conf_path cifar10_resnet20.hocon

python test.py --model_name resnet20_cifar100 --model_path path_to_pre-trained model --conf_path cifar100_resnet20.hocon
```

For ImageNet
```
python test.py --model_name resnet18/mobilenet_w1/mobilenetv2_w1 --model_path path_to_pre-trained model --conf_path imagenet.hocon
```

Results of pre-trained models are shown below:

| Model     | Bit-width| Dataset  | Top-1 Acc.  |
| --------- | -------- | -------- | ----------- | 
| resnet18  | W4A4 | ImageNet | 66.47%    | 
| resnet18  | W5A5 | ImageNet | 69.94%    | 
| mobilenetv1  | W4A4 | ImageNet | 51.36%    |
| mobilenetv1  | W5A5 | ImageNet | 68.17%    | 
| mobilenetv2  | W4A4 | ImageNet | 65.10%    | 
| mobilenetv2  | W5A5 | ImageNet | 71.28%    |
| resnet-20  | W3A3 | cifar10 | 77.07%    | 
| resnet-20  | W4A4 | cifar10 | 91.49%    | 
| resnet-20  | W3A3 | cifar100 | 64.98%    | 
| resnet-20  | W4A4 | cifar100 | 48.25%    | 