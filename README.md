# EZanimate Package
Small package for easing the basic animation of data in an Xarray DataArray.

The basic use of this package is to create a function that plots a single frame. 
This can then be passed to `make_animation_xarray()` to loop through a dataset
and make an animation.

### Installation
There is no pip or conda install for this package at the moment

1. Clone this repository.
2. Activate your conda environment of choice.
3. Change directory into the top level of the repo
4. Enter `pip install .`

## Useage examples

### 1. Make animation of dataarray using xarray's standard `.plot()` call

Import what we need:

```
import ezanimate as eza
import xarray as xr
import matplotlib.pyplot as plt
```

Read in your xarray dataset, with chunking if you want:

```
fp = <PATH TO NETCDF FILE>
ds = xr.open_dataset(fp, chunks={'ocean_time':10})
```

Next, define a single function to plot a single frame of this data. This function will be
applied to the xarray dataset to create individual frames. The only things that matter
to this package are that it takes an input argument called `data_ii` (which is data from
a single iteration of the plotting) and outputs the `matplotlib.figure()` object. For example
to apply the basic xarray `.plot()` routine with some preset params:

```
def frame_func( data_ii ):
    f = data_ii.plot(vmin = -.5, vmax = .5, cmap=plt.get_cmap('bwr', 12)).figure
    return f
```

Then we can animate the data using `ezanimate.make_animation_xarray()`:

```
eza.make_animation_xarray( ds.zeta, anim_dim = 'ocean_time', 
                           frame_func = frame_func)
```

There are a number of optional arguments you can pass to this routine, which you can find
in docstring of thef function.
