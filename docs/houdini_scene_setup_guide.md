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

We also provide an [example Houdini file](../houdini_files/example.hip) for reference.

### Basic scene

Our scene set up contains a camera, a light rig, the groomed skin primitive, and
a simple background geometry.

1.  Start by placing your base skin geometry at the center of the scene `(0, 0, 0)`. You can choose a default primitive geometry like [Sphere](https://www.sidefx.com/docs/houdini/nodes/sop/sphere.html) or [Box](https://www.sidefx.com/docs/houdini/nodes/sop/box.html), or
    import your own. If you use a Houdini's default geometry, make sure that you set
    its `Primitive Type` to be `Polygon Mesh`.
2.  Place a perspective camera some distance away, looking at this geometry.
    Choose a resolution of your choice -- ours is 1024x1024. We find that a square resolution
    makes it easier for training with convolutional layers.
3.  Set your viewport to
    look through this camera and ensure your object is in view.
4.  Setup the light source however you prefer for your scene. We used a [three-light righ setup](scene_description#light-rig).
5.  [Optional] Place a diffuse grid to act as a dark background behind the
    geometry. Adjust the transformation so it covers the view from the camera.
    We used this to experiment with training for a variety of different
    backgrounds.

To recreate exactly our setup, you can reference the [Scene Parameters](scene_description#scene-parameters).

### Grooming

1.  Select [Add Fur](https://www.sidefx.com/docs/houdini/shelf/groom_addfur.html) from the shelf tool, then pick the geometry you created
    earlier to be the skin primitive. This should automatically generate a [Guide Groom](https://www.sidefx.com/docs/houdini/nodes/sop/guidegroom.html), [Guide Deform](https://www.sidefx.com/docs/houdini/nodes/sop/guidedeform.html), and [Hair Simulate](https://www.sidefx.com/docs/houdini/nodes/sop/hairgen.html) node. 
    - In a typical Houdini grooming workflow, you will mostly design the groom style with guide curves
    using the `Guide Groom`node, and then use the `Hair Generate` node 
    to generate the final groom. The `Guide Deform` node is optional in this case.
    It's only required if you have deformation for the skin primitive.

2.  To enable simulation, add a [Guide Simulate](https://www.sidefx.com/docs/houdini/nodes/obj/guidesim.html) node before the `Hair Generate`
    node. Adjust the simulation settings as you see fit. **Tip**: simulation can take a long time. Reducing the density of guide curves to preview the sim faster when
    you're still prototyping. You can also take advantage of the `Caching` tab to
    write out your sim so subsequent runs do not have to resimulate.

3.  At the top level of the `Guide Groom` node, adjust the `Density` and
    `Length` parameters as you desire. 
        - Diving inside the `Guide Node` (double click on it), you
    can chain mulitple [Guide Process](https://www.sidefx.com/docs/houdini/nodes/sop/guideprocess.html) nodes for `Lift`, `Frizz`, `Wave` etc. To add clumping, use the [Hair Clump](https://www.sidefx.com/docs/houdini/nodes/sop/hairclump.html) node for the style of your choice.

4.  To generate the random color per guide similar to our `GuideColored` buffer,
    add a [Color](https://www.sidefx.com/docs/houdini/nodes/sop/color.html) node before the `GUIDES` output, and set its `Color Type` to
    `Random`.

5.  In the `Hair Generate` node, adjust the `Density`, `Influence Radius`,
    `Influence Decay` to get your desired look. **Note**: to render just the
    guides, and not the generated hair, you can check the `Render Guides` box
    under the `Groom Source` setting.

### Material

Materials can be created in the [Material](https://www.sidefx.com/docs/houdini/shade/build.html) network. There are 3 materials we
used.

- _Hair materials_: this is just an instance of the [Hair Shader](https://www.sidefx.com/docs/houdini/nodes/vop/hairshader.html) material
  provided by Houdini. From here, you can adjust the settings for `Primary Reflection`, `Secondary Reflection`, `Transmission`, and `Opacity`. We have
  2 instances of the hair material: one for white fur, and one for brown fur.

- _Mask material_: this is just a black [Constant](https://www.sidefx.com/docs/houdini/gallery/shop/vopmaterial/constant.html) shader to render the `Mask`
  buffer.

### Export Raster Frames

To export the raster-based buffers,
we used Houdini's [Flipbook](https://www.sidefx.com/docs/houdini/render/flipbook.html) feature to create raster frames from the
viewport directly. The general process is:

1.  Press `D` in the viewport to open [Display Options](https://www.sidefx.com/docs/houdini/ref/windows/displayopts_3d.html). Under `Scene`
    tab, maximize the `Antialias Samples` to something like 32x. This will give us the best possible raster look. But feel free to try with no AA also for your inputs. 

2.  Hide the groom by deselecting the
    [Display flag](https://www.sidefx.com/docs/houdini/network/flags.html#obj) for the `Guide Groom`, `Guide Deform`, `Guide Simulate`, and `Hair Generate` nodes. Except for the `Rasterized` buffer type, you will have to do this step for all the other buffer types.

3.  In the viewport's right sidebar, specify the appropriate lighting mode
    depending on the buffer type. `Normal Lighting` for `LitPrimitive`, and
    `Disable Lighting` for `Rasterized`, `Mask`, `WorldNormal`, `SceneDepth`. More information about the [Lights tab](https://www.sidefx.com/docs/houdini/ref/windows/displayopts_3d.html#lights-tab).

4.  With mouse over the viewport, `Ctrl + B` to make the viewport fullscreen
    (this is so flipbook gets higher resolution when exporting).

5.  Right click on the [Render Flipbook](https://www.sidefx.com/docs/houdini/render/flipbook.html#how-to) icon on the viewport's left side bar,
    then select `Flipbook with New Settings...`. In the render settings, choose
    your render frame range. From the drop down menu of `Render`, choose
    `Current Beauty Pass`. From the drop down menu of `Object Types`, choose
    `Geometry Only`.

6.  In the [Output Files](https://www.sidefx.com/docs/houdini/render/flipbook.html#output-tab) box, choose your local directory for output. **Note**:
    make sure your local directory already exists. Then filename should follow
    this format of `<BufferType$F4.png>`. The `$F4` is to make sure the images
    are created as sequence with frame numbers. The naming convention we use is:

    - Rasterized buffer: Rasterized$F4.png
    - World Normal buffer: WorldNormal$F4.png
    - Scene Depth buffer: SceneDepth$F4.png
    - Mask buffer: Mask$F4.png

7.  Under the [Effects](https://www.sidefx.com/docs/houdini/render/flipbook.html#effects) tab -> `Antialias` dropdown, choose `Use viewport setting`.

8.  In the [Size](https://www.sidefx.com/docs/houdini/render/flipbook.html#size) tab, choose 1024x1024 resolution.

9.  Click `Start` to render!

#### Specifics for each buffer type

- **Rasterized**: Rendered as is, with the groom on (make sure `Hair Generate`
  -> `Render Guides` is unchecked.

- **LitPrimitive**: This buffer is simply the diffsue shading of just the skin
  primitive.

- **Mask**: To generate this buffer, change the skin primitive material to a
  constant black material.

For the other buffers, we will be using Houdini's
[`Visualizer`](https://www.sidefx.com/docs/houdini/basics/visualizers.html). You
can find the tool in the viewport's right sidebar (you might have to scroll down
to see it).

- **WorldNormal**: Add a new color visualizer to the scene, and specify `N` in
  the `Attribute`.

- **SceneDepth**: To visualize the normalize scene depth, we'll have to add a
  new attribute `nPz` to the skin and background geometries. There are
  different ways to this, but essentially you will need to fit the closest and
  furthest points on the geometry from the camera to the [0.0, 1.0] range.
  Then add a new color visualizer to the scene, and specify `nPz` in the
  `Attribute` field. In the `Color Type`, instead of `Attribute As Is`,
  changed it to `Ramped Attribute`, and use the `Grayscale` preset for its
  color ramp.

- **GuideColored**: First, in the `Hair Generate` node, select the `Render Guides` checkbox. Then you can choose the colors that you want to use. To
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
