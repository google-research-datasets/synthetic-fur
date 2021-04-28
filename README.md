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

### Scene Parameters

**Note**: The syntax $F means frame count.

#### Perspective Camera

*   Position: [-5, 0, 0]
*   Rotation: [0, -90, 0]
*   Aperture: 41.4214
*   Pixel aspect ratio: 1
*   Look at: [0,0,0]
*   Focal length: 50
*   Resolution: 1024x1024
*   Near clipping: 0.01
*   Far clipping: 20000

#### Light Rig

The light rig contains 3 lights that are parented by a common root node. This
allows the light rig to be rotated at once by updating the root node’s
orientation per frame.

*   Parent / Root node of all 3 lights:
    *   Position: [0, 0, 0]
    *   Rotation: [if ((360&lt;$F && $F&lt;720), $F-360, 0), if ($F&lt;360, $F,
        0), 0]
        *   Note: This effectively rotate the light rig along Y axis from frame
            0-360, then along X axis from frame 360-720.
*   Key light:
    *   Type: Sphere (act also as spot light)
    *   Color: [1, 1, 1]
    *   Intensity: 100
    *   Exposure: 0
    *   Area size: 10x10
    *   Cone angle: 48.4
    *   Position: [-4.1, 4.3, -7.0]
    *   Rotation: [-31.2, -158.5, 9.4]
*   Rim light:
    *   Type: Point
    *   Color: [1, 1, 1]
    *   Intensity: 6.48
    *   Exposure: 0
    *   Position: [4.3, 1.0, 0]
*   Fill light:
    *   Type: Point
    *   Color: [1, 1, 1]
    *   Intensity: 5.46
    *   Exposure: -0.28
    *   Position: [-5.1, 2.8, 4.8]

#### Grid light

This is used only for scene 9: sphere\_3Lights\_sbPlateRL\_static and scene 10:
sphere\_3Lights\_sbPlateRotateUp\_static.

*   Type: Grid
*   Color: [1, 1, 1] (White
*   Area size: 5x5
*   Position: [0, 5, 0]
*   Rotation: [90, 0, 0]
*   Intensity in scene 9: 100
*   Intensity in scene 10: 50

#### HDRI Environment Light

*Used in scene 5, 6, 7, 8.*

*   Rotation: [0, 45, 0[
*   Intensity in scene 5, 6, 7: 2
*   Intensity in scene 8: 1
*   HDRI image in scene 5:
    [Topanga Forest](http://www.hdrlabs.com/sibl/archive/)
*   HDRI image in scene 6: [CapeHill](https://hdrihaven.com/hdri/?h=cape_hill)
*   HDRI image in scene 7:
    [Malibu Overlook](http://www.hdrlabs.com/sibl/archive/)
*   HDRI image in scene 8:
    [Hansaplatz](https://hdrihaven.com/hdri/?h=hansaplatz)

#### Shadow Blockers

*Used in scene 9 and 10.*

*   Scene 9: Box shadow blocker above fur moves left then right
    *   Blocker type: Box
    *   Size: [2.1, 0.14, 0.14]
    *   Position: [0, 1, -sin($F)]
    *   Rotation: [0, 0, 0]
*   Scene 10: Box shadow blocker above fur rotates along up
    *   Blocker type: Box
    *   Size: [2.1, 0.14, 0.7]
    *   Position: [0, 1, 0]
    *   Rotation: [0, $F, 0]

#### Geometry properties

The following geometries are used as skin primitive to groom the fur on:

<span style="text-decoration:underline;">Sphere</span>

*   Radius: 0.5

<span style="text-decoration:underline;">Torus</span>

*   Radiusx: 1
*   Radiusx: 0.5

<span style="text-decoration:underline;">Tube</span>

*   Radius 1 (top): 0.3
*   Radius 2 (bottom): 0.5
*   Height: 2.33

<span style="text-decoration:underline;">Stanford Bunny</span>

*   This is the Stanford Bunny model from
    [here](https://graphics.stanford.edu/software/scanview/models/bunny.html).

<span style="text-decoration:underline;">Rubbertoy</span>

*   This is a test model provided by default with Houdini.

<span style="text-decoration:underline;">Box</span>

*   Size: [1, 1, 1]

#### Fur Types

The dataset includes two fur types:

*   White fur: small clumps, straight, white, more transparent
*   Brown fur: large clumps, curly, brown, less transparent

The fur is procedurally generated and evenly distributed on the surface of the
skin primitive.

*   Guide curve: These are simple line segmnets of curves (see GuideColored buffer) used to
    generate additional hair strands. The white fur has randome guide colors, and the brown fur 
    has brown guide color.
*   Segments / guide curve: 8
*   Hair strands: These are line segments that together create the groom assets.
    These are what we exported in the Alembic files.
*   Segments / hair strand: 8
*   Gravity and External Forces: None

### Scene Description

Use the scene parameters above if you need to look up specific description for a
particular setting, such as lighting or fur type.

**Table 1**. Scenes with ground truths

Scene # | Scene Name                                                | Short Description                                                 | Light Type                                        | Fur Type | Skin Primitive Motion
:------ | :-------------------------------------------------------- | :---------------------------------------------------------------: | :-----------------------------------------------: | :------: | :-------------------:
1       | `sphere_3Lights_static`                                   | Sphere static                                                     | Light rig                                         | White    | Position: `[0, 0, 0]`
2       | `sphere_keyLight_static`                                  | Sphere static                                                     | Key                                               | White    | Position: `[0, 0, 0]`
3       | `sphere_fillLight_static`                                 | Sphere static                                                     | Fill                                              | White    | Position: `[0, 0, 0]`
4       | `sphere_rimLight_static`                                  | Sphere static                                                     | Rim                                               | White    | Position: `[0, 0, 0]`
5       | `sphere_hdriTopanga_static`                               | Sphere static                                                     | HDRI Topanga light Intensity = 2                  | White    | Position: `[0, 0, 0]`
6       | `sphere_hdriCapeHill_static`                              | Sphere static                                                     | HDRI CapeHill light intensity = 2                 | White    | Position: `[0, 0, 0]`
7       | `sphere_hdriMalibu_static`                                | Sphere static                                                     | HDRI Malibu light intensity = 2                   | White    | Position: `[0, 0, 0]`
8       | `sphere_hdriHansaplatz_static`                            | Sphere static                                                     | HDRI Hansaplatz light intensity = 1               | White    | Position: `[0, 0, 0]`
9       | `sphere_3Lights_sbPlateLR_static`                         | Sphere static; Box shadow blocker above fur moves left then right | Light rig & Grid light above with intensity = 100 | White    | Position: `[0, 0, 0]`
10      | `sphere_3Lights_sbPlateRotateUp_static`                   | Sphere static; Box shadow blocker above fur rotates along up      | Light rig & Grid light above with intensity = 50  | White    | Position: `[0, 0, 0]`
11      | `sphere_3Lights_moveRL`                                   | Sphere moves right, left                                          | Light rig                                         | White    | Position: `[0, 0, sin($F)]`
12      | `sphere_3Lights_moveCW`                                   | Sphere moves in clockwise circle                                  | Light rig                                         | White    | Position: `[0, cos($F), sin($F)]`
13      | `torus_3Lights_static60`                                  | Torus static, tilted at 60 degree                                 | Light rig                                         | White    | Position: `[0, 0, 0]`; Rotation: `[0, 0, 60]`
14      | `torus_3Lights_moveRL`                                    | Torus moves right, left                                           | Light rig                                         | White    | Position:`[0, 0, sin($F)]`; Rotation: `[0, 0, 90]`; Uniform Scale: `0.5`
15      | `torus_3Lights_moveCW`                                    | Torus moves in clockwise circle                                   | Light rig                                         | White    | Position: `[0, cos($F), sin($F)]`; Rotation: `[0, 0, 90]`; Uniform Scale: `0.5`
16      | `torus_3Lights_rotateUp`                                  | Torus rotates along up vector                                     | Light rig                                         | White    | Position: `[0, 0, 0]`; Rotation: `[$F, 0, 90]`; Uniform Scale: `0.5`
17      | `torus_3Lights_rotateRightForward`                        | Torus rotates along right and forward vector                      | Light rig                                         | White    | Position: `[0, 0, 0]`; Rotation: `[$F/3, 0, $F]`; Uniform Scale: `0.5`
18      | `tube_3Lights_static`                                     | Tube static                                                       | Light rig                                         | White    | Position: `[0, 0, 0]`
19      | `tube_3Lights_rotateRight`                                | Tube rotates along right vector                                   | Light rig                                         | White    | Position: `[0, 0, 0]`; Rotation: `[0, 0, $F]`
20      | `bunny_3Lights_static`                                    | Bunny static                                                      | Light rig                                         | White    | Position: `[0, 0, 0]`
21      | `bunny_3Lights_rotateUp`                                  | Bunny rotates along up vector                                     | Light rig                                         | White    | Position: `[0, 0, 0]`; Rotation: `[0, $F, 0]`
22      | `torus_curly_clumpLarge_brown_3Lights_static60`           | Torus static, tilted at 60 degree angle                           | Light rig                                         | Brown    | Position: `[0, 0, 0]`; Rotation: `[0, 0, 60]`
23      | `torus_curly_clumpLarge_brown_3Lights_rotateUp`           | Torus rotates along up vector                                     | Light rig                                         | Brown    | Position: `[0, 0, 0]`; Rotation: `[$F, 0, 90]`; Uniform Scale: `0.5`
24      | `torus_curly_clumpLarge_brown_3Lights_rotateRightForward` | Torus rotate along right and forward vectors                      | Light rig                                         | Brown    | Position: `[0, 0, 0]`; Rotation: `[$F/3, 0, $F]`; Uniform Scale: `0.5`
25      | `bunny_curly_clumpLarge_brown_3Lights_static`             | Bunny static                                                      | Light rig                                         | Brown    | Position: `[0, 0, 0]`

(\* 4/27/2021): we recently discovered an issue from frame 410-720 that contains
unstable simulation of fur. We advise avoiding using those frames.

**Table 2**. Scenes with no ground truths

Scene # | Scene Name                    | Short Description                 | Light Type                          | Fur Type | Skin Primitive Motion
:------ | :---------------------------- | :-------------------------------: | :---------------------------------: | :------: | :-------------------:
26      | `bunny_hdriCapeHill_static`   | Bunny static                      | Cape Hill HDRI light intensity = 2  | White    | Position: `[0, 0, 0]`
27      | `bunny_hdriHansaplatz_static` | Bunny static                      | Hansaplatz HDRI light intensity = 1 | White    | Position: `[0, 0, 0]`
28      | `sphere_3Lights_moveDiag`     | Sphere moves diagonally           | Light rig                           | White    | Position: `[0, sin($F), sin($F)]`
29      | `rubbertoy_3Lights_rotateUp`  | Rubbertoy rotates along up vector | Light rig                           | White    | Position: `[0, 0, 0]`; Rotation: `[0, $F, 0]`
30      | `box_3Lights_rotateXYZ`       | Box rotates all 3 directions      | Light rig                           | White    | Position: `[0, 0, 0]`; Rotation: `[$F, $F, $F]`

**Table 3**. Scenes with *Rasterized* buffer. If not listed, all the other
buffer types are still included with the scene.

Scene # | Scene Name
:------ | :---------------------------------
1       | `sphere_3Lights_static`
2       | `sphere_keyLight_static`
3       | `sphere_fillLight_static`
4       | `sphere_rimLight_static`
11      | `sphere_3Lights_moveRL`
12      | `sphere_3Lights_moveCW`
13      | `torus_3Lights_static60`
14      | `torus_3Lights_moveRL`
15      | `torus_3Lights_moveCW`
16      | `torus_3Lights_rotateUp`
17      | `torus_3Lights_rotateRightForward`
18      | `tube_3Lights_static`
19      | `tube_3Lights_rotateRight`
20      | `bunny_3Lights_static`
21      | `bunny_3Lights_rotateUp`

**Table 4**. Scenes with Alembic simulation files.

**Note**: *Since nothing changes with static scenes, the Alembic files for those
scenes only contain 1 frame.*

Scene # | Scene Name                              | Alembic file
:------ | :-------------------------------------- | :-----------
1       | `sphere_3Lights_static`                 | `sphere_static_one_frame.abc`
2       | `sphere_keyLight_static`                | `sphere_static_one_frame.abc`
3       | `sphere_fillLight_static`               | `sphere_static_one_frame.abc`
4       | `sphere_rimLight_static`                | `sphere_static_one_frame.abc`
5       | `sphere_hdriTopanga_static`             | `sphere_static_one_frame.abc`
6       | `sphere_hdriCapeHill_static`            | `sphere_static_one_frame.abc`
7       | `sphere_hdriMalibu_static`              | `sphere_static_one_frame.abc`
8       | `sphere_hdriHansaplatz_static`          | `sphere_static_one_frame.abc`
9       | `sphere_3Lights_sbPlateLR_static`       | `sphere_static_one_frame.abc`
10      | `sphere_3Lights_sbPlateRotateUp_static` | `sphere_static_one_frame.abc`
11      | `sphere_3Lights_moveRL`                 | `sphere_moveRL.abc`
12      | `sphere_3Lights_moveCW`                 | `sphere_moveCW.abc`
13      | `torus_3Lights_static60`                | `torus_static60_one_frame.abc`
14      | `torus_3Lights_moveRL`                  | `torus_moveRL.abc`
15      | `torus_3Lights_moveCW`                  | `torus_moveCW.abc`
16      | `torus_3Lights_rotateUp`                | `torus_rotateUp.abc`
17      | `torus_3Lights_rotateRightForward`      | `torus_roateRightForward.abc`
18      | `tube_3Lights_static`                   | `tube_rotate_static_one_frame.abc`
19      | `tube_3Lights_rotateRight`              | `tube_rotateUp.abc`
20      | `bunny_3Lights_static`                  | `bunny_static_one_frame.abc`
21      | `bunny_3Lights_rotateUp`                | `bunny_rotateUp.abc`

**Table 5**. Fur attributes for some select scenes for reference.

Skin Primitive             | # Guide Curves | # Generated Hair Strands
-------------------------- | :------------: | :----------------------:
Sphere                     | 4021           | 310528
Torus, uniform scale = 1   | 25287          | 1943880
Torus, uniform scale = 0.5 | 6233           | 486385
Tube                       | 9012           | 687403
Stanford Bunny             | 19121          | 1469444
Rubber Toy                 | 10361          | 802088
Box                        | 7818           | 600462

## Citation

> @article{uid, title = {SyntheticFur dataset for neural rendering}, author =
> {Trung Tuan Le and Andeep Singh Toor and Fred Bertsch and Ryan Poplin and
> Maggie Oh}, journal = {Arxiv} }
