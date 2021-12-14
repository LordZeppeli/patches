# patches K nearest neighbors encodng


## Custom Dataset (Mouse ROIs)

    python auc_kneighbors.py --n_channel_convolution 2048 --batchsize 128 --dataset DTD --stride_avg_pooling 3 --spatialsize_avg_pooling 5 --finalsize_avg_pooling 6 --lr_schedule "{0:3e-3,50:3e-4,75:3e-5}" --nepochs 80 --optimizer SGD --bottleneck_dim 128 --padding_mode reflect --kneighbors_fraction 0.4 --convolutional_classifier 6 --whitening_reg 1e-3 --sgd_momentum 0.9 --batch_norm --save_model --save_best_model 

    python auc_kneighbors.py --task_name "adjusted_weight" --loss_type "mse" --n_channel_convolution 2048 --batchsize 16 --dataset DTD --stride_avg_pooling 3 --spatialsize_avg_pooling 5 --finalsize_avg_pooling 6 --lr_schedule "{0:3e-3,50:3e-4,75:3e-5}" --nepochs 80 --optimizer SGD --bottleneck_dim 8 --padding_mode reflect --kneighbors_fraction 0.4 --convolutional_classifier 4 --whitening_reg 1e-1 --sgd_momentum 0.9 --batch_norm --save_model --save_best_model --relu_after_bottleneck --batch_norm --bn_after_bottleneck

    python auc_kneighbors.py --task_name "adjusted_weight" --loss_type "mse" --n_channel_convolution 2048 --batchsize 64 --dataset DTD --stride_avg_pooling 3 --spatialsize_avg_pooling 5 --finalsize_avg_pooling 6 --lr_schedule "{0:3e-3,50:3e-4,75:3e-5}" --nepochs 80 --optimizer SGD --bottleneck_dim 32 --padding_mode reflect --kneighbors_fraction 0.4 --convolutional_classifier 6 --whitening_reg 1e-1 --sgd_momentum 0.9 --batch_norm --save_model --save_best_model --relu_after_bottleneck  --bn_after_bottleneck --verbose

--batch_norm is optional for metadata task. We have noticed that a large batch size results in loss explosion and eventually nan results.

## CIFAR 10

### Linear regression
    * 2K patches
      ```
      python kneighbors.py --n_channel_convolution 2048 --batchsize 128 --dataset cifar10 --stride_avg_pooling 3 --spatialsize_avg_pooling 5 --finalsize_avg_pooling 6 --lr "{0:3e-3,50:3e-4,75:3e-5}" --nepochs 80 --optimizer SGD --bottleneck_dim 128 --padding_mode reflect --kneighbors_fraction 0.4 --convolutional_classifier 6 --whitening_reg 1e-3 --sgd_momentum 0.9 --batch_norm
      ```
      Best test acc. 82.32 at epoch 76/79, final acc 82.3
    * 16K patches
      ```
      python kneighbors.py --num_workers 4 --n_channel_convolution 16384 --batchsize 128 --dataset cifar10 --stride_avg_pooling 3 --spatialsize_avg_pooling 5  --lr "{0:3e-3,100:3e-4,150:3e-5}" --nepochs 175 --optimizer SGD --bottleneck_dim 128 --padding_mode reflect  --kneighbors_fraction 0.4 --convolutional_classifier 6 --whitening_reg 1e-3 --sgd_momentum 0.9 --batch_norm
      ```
      Best test acc. 85.62% at epoch 158/174, final test acc 85.5%

### 1 hidden layer classifier
    * 2K patches
      ```
       python kneighbors.py --n_channel_convolution 2048 --batchsize 128 --dataset cifar10 --stride_avg_pooling 2 --spatialsize_avg_pooling 3 --spatialsize_convolution 6 --lr "{0:3e-3,100:3e-4,150:3e-5}" --nepochs 175 --optimizer SGD --convolutional_classifier 7 --bottleneck_dim 2048 --bottleneck_spatialsize 3 --relu_after_bottleneck --padding_mode reflect --kneighbors_fraction 0.4 --whitening_reg 1e-3 --sgd_momentum 0.9 --batch_norm
      ```
      Best test acc. 88.53 at epoch 140/174, final test acc 88.4


## Imagenet 64

### Linear regression
    * 2K patches
      ```
      python kneighbors.py --num_workers 4 --n_channel_convolution 2048 --batchsize 256 --dataset imagenet64 --path_train ~/datasets/imagenet64/out_data_train --path_test ~/datasets/imagenet64/out_data_val --spatialsize_convolution 6  --stride_avg_pooling 3 --spatialsize_avg_pooling 5  --lr "{0:3e-3,50:3e-4,75:3e-5}" --nepochs 80 --optimizer SGD --bottleneck_dim 192 --padding_mode reflect --kneighbors_fraction 0.4 --convolutional_classifier 12 --whitening_reg 1e-3 --sgd_momentum 0.9 --batch_norm
      ```
      Best test acc. 33.21 top1 (54.4 top5) at epoch 79/79.
