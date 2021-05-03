## Scene Description

**Note**: The syntax `$F` means frame count.

Use the section [Scene parameters](#scene-parameters) below if you need to look up specific description for a
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


## Scene Parameters

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

#### Background

A 1x1 grid primitive, size 125x125, placed about 62.5 units behind the skin
primitive.

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

