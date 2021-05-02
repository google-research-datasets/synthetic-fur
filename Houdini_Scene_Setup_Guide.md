## Houdini Scene Setup Guide

If you're interested in creating a new Houdini scene to generate your own
dataset, or new unseen inputs for evaluating your model, you can follow this
guide. These aren't strict instructions, so please free make neccessary
adjustment for your needs.

I won't go over the basic of using Houdini -- for that, you can take a look at
SideFX [Learning Houdini](https://www.sidefx.com/learn/getting_started/). For
more information about Houdini Grooming, you can also look at the
[Introduction to Grooming in Houdini](https://www.sidefx.com/tutorials/introduction-to-grooming-in-houdini/)
by Gabriela Salmeron.

### Basic scene

Our scene set up contains a camera, a light rig, the groomed skin primitive, and
a simple background.

1.  Start by placing your base skin geometry at the center of the scene `(0, 0,
    0)`. You can choose a default primitive geometry like `Sphere` or `Box`, or
    import your own. If you use Houdini's default geometry, make that you set
    its `Primitive Type` to be `Polygon Mesh`.
2.  Place a perspective camera some distance away, looking at this geometry.
    Choose a resolution of your choice -- ours is 1024x1024. We find that square
    makes it easier for training with convolutional layers. Set your viewport to
    look through this camera, this will make it easy to make sure your geometry
    is within view.
3.  Setup the light source however you prefer for your scene.
4.  [Optional] Place a diffuse grid to act as a dark background behind the
    geometry. Adjust the transformation so it covers the view from the camera.
    We used this to experiment with training for a variety of different
    backgrounds.

To recreate exactly our setup, you can use these parameter values:

**Perspective Camera**

*   Position: [-5, 0, 0]
*   Rotation: [0, -90, 0]
*   Aperture: 41.4214
*   Pixel aspect ratio: 1
*   Look at: [0,0,0]
*   Focal length: 50
*   Resolution: 1024x1024
*   Near clipping: 0.01
*   Far clipping: 20000

**Light Rig**

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

**Background**

A 1x1 grid primitive, size 125x125, placed about 62.5 units behind the skin
primitive.

### Grooming

1.  Select `Add Fur` from the shelf tool, then pick the geometry you created
    earlier. You should now have a new `Guide Groom`, `Guide Deform`, and `Hair
    Simulate` nodes. In Houdini workflow, you mostly design your guide curves
    with the `Guide Groom`node, and then the `Hair Generate` node uses the
    guides to generate the final groom. The `Guide Deform` node is optional.
    It's only required if you have deformation for the skin primitive.

2.  To enable simulation, add a `Guide Simulate` node before the `Hair Generate`
    node. Adjust the simulation settings as you see fit. **Tip**: to quickly
    simulate your fur, reduce the density of guide curves to make it faster when
    previewing your sim. You can also take advantage of the `Caching` tab to
    write out your sim so subsequent runs do not have to resimulate.

3.  At the top level of the `Guide Groom` node, adjust the `Density` and
    `Length` parameters as you desire. Then dive inside the `Guide Node`, you
    can add `Lift`, `Frizz`, `Clump`, for the style of your choice.

4.  To generate the random color per guide similar to our `GuideColored` buffer,
    add a `Color` node before the `GUIDES` output, and set its `Color Type` to
    `Random`.

5.  On the `Hair Generate` node, adjust the `Density`, `Influence Radius`,
    `Influence Decay` to get your desired look. **Note**: to render just the
    guides, and not the generated hair, you can check the `Render Guides` box
    under the `Groom Source` setting.

### Material

Materials can be created in the `Material` network. There are 3 materials we
used.

-   *Hair materials*: this is just an instance of the `Hair Shader` material
    provided by Houdini. From here, you can adjust the settings for `Primary
    Reflection`, `Secondary Reflection`, `Transmission`, and `Opacity`. We have
    2 instances, one for white fur, and one for brown fur.

-   *Mask material*: this is just a black constant material to render the `Mask`
    buffer.

### Export Raster Frames

To export the raster-based buffers for new unseen buffer inputs for evaluation,
we used Houdini's `Render Flipbook` feature to create raster frames from the
viewport directly. The general process is like this:

1.  Press `D` in the viewport to open `Display Options: world`. Under `Scene`
    tab, maximize the `Antialias Samples` to something like 32x.

2.  Except for the `Rasterized` buffer, first, hide the groom by deselecting the
    `Display` flag (the blue button on the right side of the node) on the `Guide
    Groom`, `Guide Deform`, `Guide Simulate`, and `Hair Generate` nodes.

3.  In the viewport's right sidebar, specify the appropriate lighting mode
    depending on the buffer type. `Normal Lighting` for `LitPrimitive`, and
    `Disable Lighting` for `Rasterized`, `Mask`, `WorldNormal`, `SceneDepth`.

4.  With mouse over the viewport, `Ctrl + B` to expand the viewport to be larger
    (this is so flipbook gets higher resolution when exporting)

5.  Right click on the `Render Flipbook` icon on the viewport's left side bar,
    then select `Flipbook with New Settings...`. In the render settings, choose
    your render frame range. From the drop down menu of `Render`, choose
    `Current Beauty Pass`. From the drop down menu of `Object Types`, choose
    `Geometry Only`.

6.  In the `Output Files` box, choose your local directory for output. **Note**:
    make sure your local directory already exists. Then filename should follow
    this format of `<BufferType$F4.png>`. The `$F4` is to make sure the images
    are created as sequence with frame numbers. The naming convention we use is:

    -   Rasterized buffer: Rasterized$F4.png
    -   World Normal buffer: WorldNormal$F4.png
    -   Scene Depth buffer: SceneDepth$F4.png
    -   Mask buffer: Mask$F4.png

7.  Under the `Effects` tab -> `Antialias` dropdown, choose `Use viewport
    setting`.

8.  In the `Size` tab, choose 1024x1024 resolution.

9.  Click `Start` to render!

#### Specifics for each buffer type

-   **Rasterized**: Rendered as is, with the groom on (make sure `Hair Generate`
    -> `Render Guides` is unchecked.

-   **LitPrimitive**: This buffer is simply the diffsue shading of just the skin
    primitive.

-   **Mask**: To generate this buffer, change the skin primitive material to a
    constant black material.

For the other buffers, we will be using Houdini's
[`Visualizer`](https://www.sidefx.com/docs/houdini/basics/visualizers.html). You
can find the tool in the viewport's right sidebar (you might have to scroll down
to see it).

-   **WorldNormal**: Add a new color visualizer to the scene, and specify `N` in
    the `Attribute`.

-   **SceneDepth**: To visualize the normalize scene depth, we'll have to add a
    new attribute `nPz` to the skin and background geometries. There are
    different ways to this, but essentially you will need to fit the closest and
    furthest points on the geometry from the camera to the [0.0, 1.0] range.
    Then add a new color visualizer to the scene, and specify `nPz` in the
    `Attribute` field. In the `Color Type`, instead of `Attribute As Is`,
    changed it to `Ramped Attribute`, and use the `Grayscale` preset for its
    color ramp.

-   **GuideColored**: First, in the `Hair Generate` node, select the `Render
    Guides` checkbox. Then you can choose the colors that you want to use. To
    use the random colors for the guides, inside the `Guide Groom` node, make
    sure you have added a `Color` node before the `GUIDES` output, and set its
    `Color Type` to `Random`. For another color, such as the brown fur, go to
    the `Hair Generate` -> `Groom Source` -> `Material`, and change it to brown
    fur.

### Export Ground Truth

This step isn't needed if you are only interested in creating new input images
for evaluation.

**Note**: This is a time consuming process if you don't have access to a render
farm. The render time on a local machine can take hours to days.

We used `Mantra` to render out the ground truth images. You can follow the
instruction [here](https://www.sidefx.com/docs/houdini/render/render.html).
