![](./Images/logo.png)

# How to Create a Scatter Tool 

In this guide, we will be providing you with the steps you need to create a scatter tool that can randomize primitives' around the World space. You will create a tool that can scatter on X, Y, and Z axis by the amount of objects, their distance, and their random count. This guide is well suited for intermediate engineers. 

# Learning Objects

- How to use Omniverse UI Framework
- Add Extension from Local Path
- Set Scatter Properties
-  Analyze Random Number Generator
-  Apply USD API to set up PointInstancer
- When to use undo function

# Prereqs.

It is recomemended to understand the concepts in this guide that you have completed the folllowing:

-  Installed Omniverse Code version 2022.1.2 
- [Extension Environment Tutorial](https://github.com/NVIDIA-Omniverse/ExtensionEnvironmentTutorial)
- [Spawn Prims Extension Tutorial](https://github.com/NVIDIA-Omniverse/kit-extension-sample-spawn-prims)

# Table of Contents

-   [Step 1: Download the Starter Project ](#step-1-download-the-starter-project)
    - [Step 1.1: Add Extension from Local Path](#step-11-add-extension-from-local-path)
-   [Step 2: Build the UI](#step-2-build-the-ui)
    - [Step 2.1: Build Source Function](#step-21-build-source-function)
    - [Step 2.2: Build Scatter Function ](#step-22-build-scatter-function)
    - [Step 2.3: Build Axis Function ](#step-23-build-axis-function)
    - [Step 2.4: Build Function ](#step-24-build-function)
- [Step 3: On Scatter Function](#step-3-on-scatter-function)
- [Step 4: (Optional) scatter.py](#step-4-optional-scatterpy)
- [Step 5: (Optional) command.py](#step-5-optional-commandpy)
- [Conclusion](#this-completes-the-scatter-tool-guide-you-now-have-a-working-scatter-tool-and-understanding-of-the-ui-framework-scatter-properties-and-the-modules-within-the-scatter-tool)


# Step 1: Download the Starter Project

To get the assets for this hands-on-lab, please clone the `tutorial-start` branch of `kit-extension-sample-scatter` [github repository](https://github.com/NVIDIA-Omniverse/kit-extension-sample-scatter)


## Step 1.1: Add Extension from Local Path

Let's now import the extension into the `Extension Manager` in `Omniverse Code` by adding the local path of the `tutorial-start` branch you cloned from the [github repository](https://github.com/NVIDIA-Omniverse/kit-extension-sample-scatter)

Navigate to `Omniverse Code` and find the `Extensions` tab. If it is not there, you can make sure it is checked in Window > Extensions.

In the `Extensions` tab, you will see a gear icon - select this icon to open `Extensions Search Paths`. Locate the <span style="color:green"> green </span> :heavy_plus_sign: icon at the bottom right of that console to add a new path. Then copy/paste the local path of the `exts` folder from `tutorial-start` branch, as so: 

![](./Images/add_ext.PNG)


>:memo: If you do not see `Scatter Window` pop up after adding the local path, ensure the extension in enabled in the `Extensions Manager`.

# Step 2: Build the UI

### Theory

We start this guide with a blank `Scatter Window`. In the following steps, you will learn to use `Omniverse UI Framework` to build out the UI for Source, Scatter, and each Axis. 

This step will focus on the `window.py` file found in the `exts/omni.example.ui_scatter_tool/omni/example/ui_scatter_tool/` directory. To learn more about the other files in the respository, please refer to [How to Make an Extension by Spawning Primitives](https://github.com/NVIDIA-Omniverse/kit-extension-sample-spawn-prims/blob/main/exts/omni.example.spawnPrims/tutorial/Spawn_PrimsTutorial.md).


By the end of Step 2, your `Scatter Window` UI will be built out.

## Step 2.1: Build Source Function

### Theory

To begin the scatter tool UI, we will start by slotting the source of the primitive that we intend to scatter. 

For this step, we will be taking a look at `_build_source` function. Currently, it should look like this:

```python
def _build_source(self):
    """Build the widgets of the "Source" group"""
    pass
```

The objective of this function is create a place to display the source path of the primitive.  

We will start setting up this by using the `Omniverse UI Framework` ([click here for more information](https://docs.omniverse.nvidia.com/py/kit/source/extensions/omni.ui/docs/index.html)) to create a `Collapsable Frame`. 

```python
    def _build_source(self):
        """Build the widgets of the "Source" group"""
        # Create frame
        with ui.CollapsableFrame("Source", name="group"):
```

Then, we will use `VStack` to create the column and `HStack` to create a row.

```python
    def _build_source(self):
        """Build the widgets of the "Source" group"""
        # Create frame
        with ui.CollapsableFrame("Source", name="group"):
            # Create column
            with ui.VStack(height=0, spacing=SPACING):
                # Create row
                with ui.HStack():
```

Next, let's create a `Label` to give a name for the field.

```python
    def _build_source(self):
        """Build the widgets of the "Source" group"""
        # Create frame
        with ui.CollapsableFrame("Source", name="group"):
            # Create column
            with ui.VStack(height=0, spacing=SPACING):
                # Create row
                with ui.HStack():
                  # Give name of field
                    ui.Label("Prim", name="attribute_name", width=self.label_width)
                    ui.StringField(model=self._source_prim_model)
```

Finally, we will add a button to give a selection to the string field.

```python
    def _build_source(self):
        """Build the widgets of the "Source" group"""
        # Create frame
        with ui.CollapsableFrame("Source", name="group"):
            with ui.VStack(height=0, spacing=SPACING):
                with ui.HStack():
                  # Give name of field
                    ui.Label("Prim", name="attribute_name", width=self.label_width)
                    ui.StringField(model=self._source_prim_model)
                    # Button that puts the selection to the string field
                    ui.Button(
                        " S ",
                        width=0,
                        height=0,
                        style={"margin": 0},
                        clicked_fn=self._on_get_selection,
                        tooltip="Get From Selection",
                    )
```

Don't worry if you do not see your changes in the `Scatter Window`, the UI will be built all together at the end of Step 1 in `build_fn`.

## Step 2.2: Build Scatter Function

### Theory

  In this section, we will use the `Omniverse UI Framework` to create the `Prim Path` and `Prim Type`. 

  Let's move further down `window.py` to `_build_scatter` function, which currently looks like this:

  ```python
    def _build_scatter(self):
        """Build the widgets of the "Scatter" group"""
        pass
  ```

  Just as we did in the previous step, we will build out the UI using Collapsable Frame:

  ```python
    def _build_scatter(self):
        """Build the widgets of the "Scatter" group"""
        with ui.CollapsableFrame("Scatter", name="group"):
  ```

  Then create one column that will nest 3 rows. Within these rows we will create a `Label`, one for `Prim Path`, `Path Type`, and `Seed`.

  ```python
    def _build_scatter(self):
        """Build the widgets of the "Scatter" group"""
        with ui.CollapsableFrame("Scatter", name="group"):
            # Column
            with ui.VStack(height=0, spacing=SPACING):
                # Row
                with ui.HStack():
                    ui.Label("Prim Path", name="attribute_name", width=self.label_width)
                    ui.StringField(model=self._scatter_prim_model)

                # Row
                with ui.HStack():
                    ui.Label("Prim Type", name="attribute_name", width=self.label_width)
                    ui.ComboBox(self._scatter_type_model)

                # Row
                with ui.HStack():
                    ui.Label("Seed", name="attribute_name", width=self.label_width)
                    ui.IntDrag(model=self._scatter_seed_model, min=0, max=10000)
  ```
  
  ## Step 2.3: Build Axis Function

  ### Theory

  As for our last function before we build everything together in the `Scatter Window`, we will build the UI needed to set the parameters of scatter on the X, Y, and Z Axis.

  Let's take a look at `_build_axis` function, which looks like this currently:

  ```python
    def _build_axis(self, axis_id, axis_name):
        """Build the widgets of the "X" or "Y" or "Z" group"""
        pass
  ```

  We will use the `UI Framework` for this function as well, following the same structure as `_build_scatter` with a Collapsable Frame, one column, and 3 rows. Our rows will each have a `Label` for Object Count, Distance, and Random.
 

  We only need to build this one because we will use this framework to be called for each axis later on.

  ```python
    def _build_axis(self, axis_id, axis_name):
        """Build the widgets of the "X" or "Y" or "Z" group"""
        with ui.CollapsableFrame(axis_name, name="group"):
            # Column
            with ui.VStack(height=0, spacing=SPACING):
                # Row
                with ui.HStack():
                    ui.Label("Object Count", name="attribute_name", width=self.label_width)
                    ui.IntDrag(model=self._scatter_count_models[axis_id], min=1, max=100)

                # Row
                with ui.HStack():
                    ui.Label("Distance", name="attribute_name", width=self.label_width)
                    ui.FloatDrag(self._scatter_distance_models[axis_id], min=0, max=10000)

                # Row
                with ui.HStack():
                    ui.Label("Random", name="attribute_name", width=self.label_width)
                    ui.FloatDrag(self._scatter_random_models[axis_id], min=0, max=10000)

  ```

  ## Step 2.4: Build Function

  ### Theory

Now that we have the `UI Framework` built for `Scatter Window`, we will use the `_build_fn` to call source, scatter, and our X, Y, and Z Axis.

Currently, our `_build_fn` looks like this:

```python
    def _build_fn(self):
        """
        The method that is called to build all the UI once the window is
        visible.
        """
        pass
```

We will house each function in a `ScrollingFrame` and nest it in one column. 

Notice that when calling for the Axis UI, we call `_build_axis` three times and identify which Axis it will represent. 

After the frame is built, we will add a button. This will be our `GO` button to start the Scatter. 

```python
    def _build_fn(self):
        """
        The method that is called to build all the UI once the window is
        visible.
        """
        # Frame
        with ui.ScrollingFrame():
            # Column
            with ui.VStack(height=0):
                # Build it
                self._build_source()
                self._build_scatter()
                self._build_axis(0, "X Axis")
                self._build_axis(1, "Y Axis")
                self._build_axis(2, "Z Axis")

                # The Go button
                ui.Button("Scatter", clicked_fn=self._on_scatter)
```

## Congratulations!

You have created the UI Framework for `Scatter Window`!

![](./Images/ScatterWindowUIComplete.PNG)

## Step 3: On Scatter Function

### Theory

In this step, we will add to the function that is called when the `Scatter` button is pressed. 

Locate the `_on_scatter` function at line 73 of `window.py`, it will look like this:

```python
    def _on_scatter(self):
        """Called when the user presses the "Scatter" button"""
        prim_names = [i.strip() for i in self._source_prim_model.as_string.split(",")]
        if not prim_names:
            prim_names = get_selection()

        if not prim_names:
            pass
```

We already have the variable `prim_names` defined so now we need to set the `Scatter Properties`. For this guide we have provided you with some arrays and loops for the properties but we encourage and challenge you to experiment with your own as well.

In addition to the properties of the scatter, we will also set the properties for duplicating the primitive.

```python
    def _on_scatter(self):
        """Called when the user presses the "Scatter" button"""
        prim_names = [i.strip() for i in self._source_prim_model.as_string.split(",")]
        if not prim_names:
            prim_names = get_selection()

        if not prim_names:
            pass

        transforms = scatter(
            count=[m.as_int for m in self._scatter_count_models],
            distance=[m.as_float for m in self._scatter_distance_models],
            randomization=[m.as_float for m in self._scatter_random_models],
            id_count=len(prim_names),
            seed=self._scatter_seed_model.as_int,
        )

        duplicate_prims(
            transforms=transforms,
            prim_names=prim_names,
            target_path=self._scatter_prim_model.as_string,
            mode=self._scatter_type_model.get_current_item().as_string,
        )
```

## Congratulations!

Now, we when you create a primitive in the Viewport, set it in `Source`, and click `Scatter`, your primitive will scatter using the properties set above.

![](./Images/scatterClicked.gif)

# Step 4: (Optional) scatter.py

>:memo: This is a review and no code is being added

### Theory

  For this section we will introduce `scatter.py` and briefly showcase its function in the scatter tool.

  Let's begin by navigating to `scatter.py` in your `exts` folder hierarchy. 

  This script is where the `on_scatter` function in `window.py` is pulling it's information from. You will notice the following arguments in `scatter.py`:

```python
def scatter(
    count: List[int], distance: List[float], randomization: List[float], id_count: int = 1, seed: Optional[int] = None
):
...
```

These arguments match up with the properties in `transforms` from the previous step of `on_scatter`.

Below the arguments we have commented a description of each:

```python
    """
    Returns generator with pairs containing transform matrices and ids to
    arrange multiple objects.

    ### Arguments:

        `count: List[int]`
            Number of matrices to generage per axis

        `distance: List[float]`
            The distance between objects per axis

        `randomization: List[float]`
            Random distance per axis

        `id_count: int`
            Count of differrent id

        `seed: int`
            If seed is omitted or None, the current system time is used. If seed
            is an int, it is used directly.
""""
```

Below this comment is where we initialize the loop to randomly generated sets of points as well as create a matric with position randomization for each axis:

```python
    # Initialize the random number generator.
    random.seed(seed)

    for i in range(count[0]):
        x = (i - 0.5 * (count[0] - 1)) * distance[0]

        for j in range(count[1]):
            y = (j - 0.5 * (count[1] - 1)) * distance[1]

            for k in range(count[2]):
                z = (k - 0.5 * (count[2] - 1)) * distance[2]

                # Create a matrix with position randomization
                result = Gf.Matrix4d(1)
                result.SetTranslate(
                    Gf.Vec3d(
                        x + random.random() * randomization[0],
                        y + random.random() * randomization[1],
                        z + random.random() * randomization[2],
                    )
                )

                id = int(random.random() * id_count)

                yield (result, id)
```

`scatter.py` is where you can adjust to create different scatters, such as scatter on the geometry or a scatter that uses texture.

## Step 5: (Optional) command.py

>:memo: This is a review and no code is being added.

In this section we will introduce `command.py` and briefly review its importance to the scatter tool.

Naviagte to `command.py` in the `exts` folder hierarchy and let's review what's inside. 

At the start of the `ScatterCreatePointInstancerCommand` class, you will notice that we have added a comment of the arguments used and a description of each.

```python
    """
    Create PointInstancer undoable **Command**.

    ### Arguments:

        `path_to: str`
            The path for the new prims
            
        `transforms: List`
            Pairs containing transform matrices and ids to apply to new objects

        `prim_names: List[str]`
            Prims to duplicate
    """
```

Below the comment is where we initialize these arguments, set the USD stage, and unzip the list of tuples, as so:

```python
    def __init__(
        self,
        path_to: str,
        transforms: List[Tuple[Gf.Matrix4d, int]],
        prim_names: List[str],
        stage: Optional[Usd.Stage] = None,
        context_name: Optional[str] = None,
    ):
        omni.usd.commands.stage_helper.UsdStageHelper.__init__(self, stage, context_name)
        self._path_to = path_to

        # We have it like [(tr, id), (tr, id), ...]
        # It will be transformaed to [[tr, tr, ...], [id, id, ...]]
        unzipped = list(zip(*transforms))

        self._positions = [m.ExtractTranslation() for m in unzipped[0]]
        self._proto_indices = unzipped[1]
        self._prim_names = prim_names.copy()
```

Following that we set up the `PointInstancer` Command where we use the USD API to create the geometry and create the points during scatter. 

```python
    def do(self):
        stage = self._get_stage()

        # Set up PointInstancer
        instancer = UsdGeom.PointInstancer.Define(stage, Sdf.Path(self._path_to))
        attr = instancer.CreatePrototypesRel()
        for name in self._prim_names:
            attr.AddTarget(Sdf.Path(name))
        instancer.CreatePositionsAttr().Set(self._positions)
        instancer.CreateProtoIndicesAttr().Set(self._proto_indices)
```

Finally, we call the `undo` function. This is called when the user undo's the scatter to restore the stage before `Do()`. The reason we use `undo` rather than `execute` is so that undo bypasses the queue of the stage. 

```python
    def undo(self):
        delete_cmd = omni.usd.commands.DeletePrimsCommand([self._path_to])
        delete_cmd.do()
```

## This completes the Scatter Tool guide. You now have a working Scatter Tool and understanding of the UI Framework, Scatter Properties, and the modules within the Scatter Tool.