# Building Footprint Segmentation


## Installation
    
    
    pip install building-footprint-segmentation
    

## Dataset 

- [Massachusetts Buildings Dataset](https://www.cs.toronto.edu/~vmnih/data/)
- [Inria Aerial Image Labeling Dataset](https://project.inria.fr/aerialimagelabeling/)

## Training

- [Train With Config](https://github.com/fuzailpalnak/building-footprint-segmentation/blob/main/examples/Run%20with%20config.ipynb)
    , Use [config template](https://codebeautify.org/yaml-validator/cbc60637) for generating training config

- [Train With Arguments](https://github.com/fuzailpalnak/building-footprint-segmentation/blob/main/examples/Run%20with%20defined%20arguments.ipynb)

## Visualize Training

##### Test images at end of every epoch

- Follow [Example](https://github.com/fuzailpalnak/building-footprint-segmentation/blob/main/examples/TestCallBack.ipynb)
 
##### Visualizing on Tensorboard

```python
from building_footprint_segmentation.helpers.callbacks import CallbackList, TensorBoardCallback
where_to_log_the_callback = r"path_to_log_callback"   
callbacks = CallbackList()

# Ouptut from all the callbacks caller will be stored at the path specified in log_dir
callbacks.append(TensorBoardCallback(where_to_log_the_callback))

```

To view Tensorboard dash board

    tensorboard --logdir="path_to_log_callback"

## Defining Custom Callback
```python
from building_footprint_segmentation.helpers.callbacks import CallbackList, Callback

class CustomCallback(Callback):
    def __init__(self, log_dir):
        super().__init__(log_dir)


where_to_log_the_callback = r"path_to_log_callback"   
callbacks = CallbackList()

# Ouptut from all the callbacks caller will be stored at the path specified in log_dir
callbacks.append(CustomCallback(where_to_log_the_callback))
```

## Split the images in smaller sample
```python
import glob
import os

from image_fragment.fragment import ImageFragment

# FOR .jpg, .png, .jpeg
from imageio import imread, imsave

# FOR .tiff
from tifffile import imread, imsave

ORIGINAL_DIM_OF_IMAGE = (1500, 1500, 3)
CROP_TO_DIM = (384, 384, 3)

image_fragment = ImageFragment.image_fragment_3d(
    fragment_size=(384, 384, 3), org_size=ORIGINAL_DIM_OF_IMAGE
)

IMAGE_DIR = r"pth\to\input\dir"
SAVE_DIR = r"pth\to\save\dir"

for file in glob.glob(
    os.path.join(IMAGE_DIR, "*")
):
    image = imread(file)
    for i, fragment in enumerate(image_fragment):
        # GET DATA THAT BELONGS TO THE FRAGMENT
        fragmented_image = fragment.get_fragment_data(image)

        imsave(
            os.path.join(
                SAVE_DIR,
                f"{i}_{os.path.basename(file)}",
            ),
            fragmented_image,
        )

```
## Inference 
- [Perform Augmentation during Inference and aggregate results](https://github.com/fuzailpalnak/building-footprint-segmentation/blob/main/examples/PredictionWithAugmentations.ipynb)
 

## Segmentation for building footprint

- [x] binary
- [ ] building with boundary (multi class segmentation)

## Weight File

- [RefineNet trained on INRIA](https://github.com/fuzailpalnak/building-footprint-segmentation/releases/download/alpha/refine.zip)
- [DlinkNet trained on Massachusetts Buildings Dataset](https://github.com/fuzailpalnak/building-footprint-segmentation/releases/download/alpha/DlinkNet.zip)

## Refer [gtkit](https://github.com/fuzailpalnak/gtkit) for commonly used utility task when working with Geotiff

- [Generate bitmap from shape file](https://github.com/fuzailpalnak/gtkit/blob/main/tutorials/shpToBitmap.ipynb)
- [Generate shape geometry from geo reference bitmap](https://github.com/fuzailpalnak/gtkit/blob/main/tutorials/bitmapToShp.ipynb)
- [Save Multi Band Imagery](https://github.com/fuzailpalnak/gtkit)
