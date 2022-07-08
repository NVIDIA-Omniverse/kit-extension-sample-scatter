# Scatter Kit Extension Sample
## [Scatter Tool (omni.example.ui_scatter_tool)](exts/omni.example.ui_scatter_tool)

![](https://github.com/NVIDIA-Omniverse/kit-extension-sample-scatter/raw/main/exts/omni.example.ui_scatter_tool/data/preview.png)
​
### About

This Extension uses `Scatter Properties` to scatter a selected primitive on the X, Y, and Z Axis per object count, distance, and randomization set by the user.
​
### [README](exts/omni.example.ui_scatter_tool)
See the [README for this extension](exts/omni.example.ui_scatter_tool) to learn more about it including how to use it.

### [Tutorial](exts/omni.example.ui_scatter_tool/Tutorial/Scatter_Tool_Guide.md)

This extension sample also includes a step-by-step tutorial to accelerate your growth as you learn to build your own Omniverse Kit extensions. 

In the tutorial you will learn how to build upon modules provided to you to create the `Scatter Window UI` and the `Scatter Properties`.

​[Get started with the tutorial.](exts/omni.example.ui_scatter_tool/Tutorial/Scatter_Tool_Guide.md)

​
## Adding This Extension

To add this extension to your Omniverse app:
1. Go into: Extension Manager -> Gear Icon -> Extension Search Path
2. Add this as a search path: `git://github.com/NVIDIA-Omniverse/kit-extension-sample-scatter?branch=main&dir=exts`


## Linking with an Omniverse app

For a better developer experience, it is recommended to create a folder link named `app` to the *Omniverse Kit* app installed from *Omniverse Launcher*. A convenience script to use is included.

Run:

```bash
> link_app.bat
```

There is also an analogous `link_app.sh` for Linux. If successful you should see `app` folder link in the root of this repo.

If multiple Omniverse apps is installed script will select recommended one. Or you can explicitly pass an app:

```bash
> link_app.bat --app code
```

You can also just pass a path to create link to:

```bash
> link_app.bat --path "C:/Users/bob/AppData/Local/ov/pkg/create-2022.1.3"
```

## Contributing
The source code for this repository is provided as-is and we are not accepting outside contributions.
