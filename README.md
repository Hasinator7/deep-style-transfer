# Deep Style Transfer
Tensorflow implementation of the fast feed-forward neural style transfer network by Johnson et al.

Here is an example of styling a photo of San Francisco with Van Gogh's Starry Night
<img src='https://github.com/albertlai/deep-style-transfer/raw/master/data/sf.jpg' height=256/>
<img src='https://github.com/albertlai/deep-style-transfer/raw/master/data/starry.jpg' height=256/>
<img src='https://github.com/albertlai/deep-style-transfer/raw/master/data/styled.jpg' height=256/>

The code is based off [this paper](http://cs.stanford.edu/people/jcjohns/eccv16/) by Johnson et al which in turn builds
off of [A Neural Algorithm of Artistic Style](https://arxiv.org/abs/1508.06576) by Gatys et al.

This implementation uses [Instance Norm](https://arxiv.org/abs/1607.08022) described by Ulyanov et al and 
[Resize-Convolution](http://distill.pub/2016/deconv-checkerboard/) from Odena et al.

Takes a few hours to train on a P2 instance on AWS and image generation takes a few seconds on a Macbook Pro. Training image dataset was from MS COCO validation set and uses the VGG19 network for texture and style loss.

If you don't want to install tensorflow and deal with training your own models, try the Android app which has other styles trained using this code.

<a href='https://play.google.com/store/apps/details?id=com.shiftingbit.swapstyle&pcampaignid=MKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1'><img alt='Get it on Google Play' height='75px' src='https://play.google.com/intl/en_us/badges/images/generic/en_badge_web_generic.png'/></a>

## Requirements
1. Tensorflow 0.12
2. pip install:
  * scikit-image
  * numpy 1.11 

## Instructions for Processing
1. Go to the project root (there is a pretrained model in the /data directory)
2. Run:
```
$ python style.py --input=path_to_image.jpg --output=your_output_file.jpg
```
Note: The style.py script defaults to a pretrained model based off starry.jpg - if you wish to try a different style you will have to train your own in the next section

## Instructions for Training
1. Download [VGG19 weights](https://mega.nz/#!xZ8glS6J!MAnE91ND_WyfZ_8mvkuSa2YcA7q-1ehfSm-Q1fxOvvs) as vgg19.npy
2. Download [MS COCO validation dataset](http://mscoco.org/dataset/#download) - the training script defaults to look for images in a directory named input_images, though you can add whatever directory you want as a command line argument
3. Run:
```
$ python train.py --data_dir=/path/to/ms_coco --texture=path/to/source_image.jpg
```
## Instructions for Exporting
When training, your model gets saved as a series of tensorflow checkpoints - the default directory for this is the named 'model.' We export these checkpoints to a .pb file for easy usage, using the freeze_graphs.py file copied from the tensorflow source repo. To run the export script run:
``` 
$ python export.py --input_dir=path_to/training_directory
```
