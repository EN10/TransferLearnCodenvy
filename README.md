# Transfer Learning Codenvy

Retraining one of Google's CNN image classification models to new categories using Transfer Learning.
This can be an much faster (in a few minutes) than training from scratch ([Inception V3](https://github.com/EN10/KerasInception) took Google, 2 weeks).

* Based on [Transfer Learning Colab](https://github.com/EN10/TransferLearnColab)

## Install
    sudo pip install --ignore-installed --upgrade https://github.com/lakshayg/tensorflow-build/releases/download/tf1.9.0-ubuntu16.04-py27-py35/tensorflow-1.9.0-cp35-cp35m-linux_x86_64.whl
    sudo pip install tensorflow-hub

[Tensorflow Builds](https://github.com/lakshayg/tensorflow-build)

## Create Codenvy project
    mkdir retrain
    cd retrain

## Download Flowers
    curl -LO http://download.tensorflow.org/example_images/flower_photos.tgz
    tar xzf flower_photos.tgz

## Download Retrain
    curl -LO https://github.com/tensorflow/hub/raw/master/examples/image_retraining/retrain.py

## Speedup Training 
reduce the number of images by ~70% : 3681 -> 1668

    ls flower_photos/* | wc -l
    rm flower_photos/*/[3-9]*
    rm flower_photos/daisy/ flower_photos/dandelion/ flower_photos/tulips/ -r
    ls flower_photos/* | wc -l
also only use 2 flowers e.g. roses and sunflowers : 1668 -> 591

## Retrain
    python3 retrain.py --image_dir ./flower_photos --tfhub_module https://tfhub.dev/google/imagenet/mobilenet_v2_100_224/feature_vector/2 --how_many_training_steps 500
    
2m29s : Codenvy - Python 3 - 591 images - 500 Steps - mobilenet_v2_100_224 - Test 98.0%    

* [Pre-trained Models ](https://github.com/tensorflow/models/blob/master/research/slim/README.md#pre-trained-models)
* [Comparision](https://1.bp.blogspot.com/-E1qM-CKq-BA/WfuGc22fPBI/AAAAAAAACIg/frpwbO5Jh-oL0cSObyJa29fXkBsuVl7CACLcBGAs/s1600/image3.jpg)

## Download Label Image
    curl -LO https://github.com/tensorflow/tensorflow/raw/master/tensorflow/examples/label_image/label_image.py

## Download Test Image
    wget https://5.imimg.com/data5/AA/KK/MY-6677193/red-rose-500x500.jpg

## Use the Retrained Model
    python label_image.py --graph=/tmp/output_graph.pb --labels=/tmp/output_labels.txt --input_layer=Placeholder --output_layer=final_result --input_height=224 --input_width=224 --image=red-rose-500x500.jpg | grep 'roses\|sunflowers'

## Save Model
    cp /tmp/output* ./

## [Training on Your Own Categories](https://github.com/EN10/TensorFlowForPoets#training-on-your-own-categories)

download images, rename folder, zip, upload, unzip, mkdir, mv   

#### Images
[Batch Image downloader](https://chrome.google.com/webstore/detail/fatkun-batch-download-ima/nnjjahlikiabnchcpehcpkdeckfgnohf?hl=en)    
Loads images on screen, in Google Images Scroll for more images.

**Zip**: in windows right click - Send to - Compressed (zipped) folder

**Upload**: in codenvy - Projects - Upload File

#### Unzip

    unzip foldername.zip

#### Folders

    mkdir images
    mv foldername images

moves `foldername` into `images` folder

## tmp

bottlenecks, graph & model in `/tmp`