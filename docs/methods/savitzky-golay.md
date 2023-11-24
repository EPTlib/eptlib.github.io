### Savitzky-Golay filter <object name="new" class="label">New!</object>

The Savitzky-Golay filter can be applied on kernels with different size and shape.

```toml
[parameter.savitzky-golay]
    size = [2, 2, 1]
    shape = 0
    degree = 2
    weight-param = 0.05
```

- ```size``` is the number of voxels along each semi-axis of the kernel (default: ```[1,1,1]```).
- ```shape``` selects the kernel shape according to the following table (default: ```0```).
- ```degree``` is the degree of the interpolating polynomial. It must be at least 2 (default: ```2```).
- ```weight-param``` is a parameter of the weight function used for the reference image. It is used only if a reference image is provided in input (default: ```0.05```).

| Code | Shape     | Example with ```size = [2,2,1]```                                        |
|------+-----------+--------------------------------------------------------------------------|
| 0    | Cross     | ![](/assets/images/savitzky-golay-cross.png){: style="width:128px"}      |
| 1    | Ellipsoid | ![](/assets/images/savitzky-golay-ellipsoid.png){: style="width:128px"}  |
| 2    | Cuboid    | ![](/assets/images/savitzky-golay-cuboid.png){: style="width:159px"}     |

If a reference image is provided in input to the method, the Savitzky-Golay filter is anatomically adapted with respect to it.
Precisely, in the filter procedure each voxel contribution is weighted according to a Gaussian of standard deviation ```weight-param``` applied to the relative contrast with respect to the central voxel.
