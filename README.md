## SyntheticFur Dataset

[Paper]()

[<img src="images/dataset/bunny_gt.gif" alt="bunny_1" width = "512"/>](https://youtu.be/FNOddH3DXUk)

[Video](https://youtu.be/5uraQu_5Tyg) showing all the ground truth frames of
every scene.

## Overview

Collecting and generating high quality fur images is an expensive and difficult
process that requires content specialists to generate. By releasing this unique
dataset with high quality lighting simulation via ray tracing, this can save
time for researchers seeking to advance studies of fur rendering and simulation,
without having to recreate this laborious process.

The dataset was used for neural rendering research at Google that takes
advantage of rasterized image buffers and converts them into high quality
raytraced fur renders. We believe that this dataset can contribute to the
computer graphics and machine learning community to develop more advanced
techniques with fur rendering.

For a quick overview of the dataset, see the galleries of images contained
within the dataset:

-   [Sphere](https://photos.app.goo.gl/JcRmKfjjqb7Kuwdz5)
-   [Torus](https://photos.app.goo.gl/FJD3dC9RTt8jcBGg8)
-   [Bunny](https://photos.app.goo.gl/3CkwRjWtZzpp5FTL7)

## Download

The dataset is hosted on Google Cloud Storage. It's available for both
[browsing](https://pantheon.corp.google.com/storage/browser/magentadata/datasets/synthetic_fur/release?project=brain-magenta)
(Google account required) and download. The dataset are divded into images
(~21.01 GiB, with 18GB of training and 3GB of holdout set), and simulation
files.

We recommend to download the images separately from the simulation files. To get
started, you can download a subset of scenes instead, such as scene 1 - 4, so
you can test out your pipeline. The simulation files are entirely optional, and
only needed if you want to experiment on building a neural physics model. These
simulation files can be rather large, so make sure you have enough space on your
drive.

To download the dataset, we recommend using the
[gsutil tool](https://cloud.google.com/storage/docs/gsutil) included in the
[Google Cloud SDK](https://cloud.google.com/sdk/docs/). For example, the
command:

```
gsutil -m cp -r gs://magentadata/datasets/synthetic_fur/release/images .
```

will copy only the images in the dataset.

## What's in this?

The dataset contains ~140,000 images and 15 [Alembic]((https://www.alembic.io/)
files generated from scratch using Houdini and Zync.

<p float="left">
    <img src="images/dataset/sphere_3.png" alt="sphere_1" width = "128"/>
    <img src="images/dataset/sphere_hdri_capehill.png" alt="sphere_hdri_capehill" width = "128"/>
    <img src="images/dataset/tube_2.png" alt="tube_2" width = "128"/>
    <img src="images/dataset/torus_2.png" alt="torus_2" width = "128"/>
    <img src="images/dataset/torus_3.png" alt="torus_3" width = "128"/>
    <img src="images/dataset/torus_brown_1.png" alt="torus_brown_1" width = "128"/>
    <img src="images/dataset/bunny_brown_1.png" alt="bunny_brown_1" width = "128"/>
    <img src="images/dataset/bunny_1.png" alt="bunny_1" width = "128"/>
    <img src="images/dataset/bunny_GuideColored_1.png" alt="bunny_LitPrimitive_1" width = "128"/>
    <img src="images/dataset/bunny_LitPrimitve_1.png" alt="bunny_LitPrimitive_1" width = "128"/>
    <img src="images/dataset/bunny_SceneDepth_1.png" alt="bunny_SceneDepth_1" width = "128"/>
    <img src="images/dataset/bunny_WorldNormal_1.png" alt="bunny_SceneDepth_1" width = "128"/>
</p>

The dataset has the following definitions:

-   *Scene*: a sequence of frames that together represents a continuous motion
    of fur. Each scene contains frames of images and optionally 1 corresponding
    Alembic file for simulation data. See Table 1 - 5 below for details about
    each scene.
-   *Frame*: each frame is a set of conditional images that represents the
    ground truth and the input images.

## Data Structure

#### Images

-   Resolution: 1024x1024
-   Format: png
-   Directory naming convention: `<scene_name>/<buffer_name><4 digit frame
    count>.png`
-   Frames / scene: 720
-   Buffer names: `HighQualityRender (ground truth)`, `Rasterized`,
    `SceneDepth`, `LitPrimitive`, `GuideColored`, `WorldNormal`, `Mask`

##### Example

HighQualityRender (ground truth)                                       |
:--------------------------------------------------------------------: |
<img src="images/dataset/bunny_2.png" alt="bunny_2_gt" width = "128"/> |

GuideColored                                                                            | LitPrimitive                                                                                   | SceneDepth                                                                                 | WorldNormal                                                                                  | Mask                                                                          | Rasterized
:-------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------: | :--------:
<img src="images/dataset/bunny_GuideColored_2.png" alt="bunny_2_guides" width = "128"/> | <img src="images/dataset/bunny_LitPrimitive_2.png" alt="bunny_2_lit_primitive" width = "128"/> | <img src="images/dataset/bunny_SceneDepth_2.png" alt="bunny_2_scene_depth" width = "128"/> | <img src="images/dataset/bunny_WorldNormal_2.png" alt="bunny_2_world_normal" width = "128"/> | <img src="images/dataset/bunny_Mask_2.png" alt="bunny_2_mask" width = "128"/> | <img src="images/dataset/bunny_Rasterized_2.png" alt="bunny_2_rasterized" width = "128"/>

**Note**: 

- The *GuideColored* buffer has random colors for white fur scenes, and brown color for brown fur scenes.
- The *Mask* buffer can be used for sampling specific regions of
interest if trained the images as crops. 
- We find that the *Rasterized*
buffer type takes too long and costly to generate and does not fit our
description of inexpensive inputs, therefore excluding it during training.

#### Simulations

The Alembic files capture the fur strand positions per frame. Alembic is an open
computer graphics interchange framework. The Alembic format is described
[here](https://www.alembic.io/).

A groom consists of hair strands. Each hair strand is a series of connected line
segments. The following information can be extracted from the Alembic files:

-   Number of hair strands
-   Number of segments per hair
-   The hair point’s position per frame
-   The hair point’s velocity per frame
-   etc.

**Note**: Each Alembic file only contains the definition of the groom, and does
not include the skin primitives nor the guide curves.

## Scene Description
See [here](docs/scene_description.md).

## Houdini Scene Setup Guide
See [here](docs/houdini_scene_setup_guide.md).

## Citation

> @article{uid, title = {SyntheticFur dataset for neural rendering}, author =
> {Trung Tuan Le and Andeep Singh Toor and Fred Bertsch and Ryan Poplin and
> Maggie Oh}, journal = {Arxiv} }
