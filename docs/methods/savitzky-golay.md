### Savitzky-Golay filter

The Savitzky-Golay filter can be applied on kernels with different size and shape.

```toml
[parameter.savitzky-golay]
    size = [2,2,1]
    shape = 0
```

- ```size``` is the number of voxels along each semi-axis of the kernel (default: ```[1,1,1]```).
- ```shape``` selects the kernel shape according to the following table (default: ```0```).

| Code | Shape     | Example with ```size = [2,2,1]```                                        |
|------+-----------+--------------------------------------------------------------------------|
| 0    | Cross     | ![](/assets/images/savitzky-golay-cross.png){: style="width:128px"}      |
| 1    | Ellipsoid | ![](/assets/images/savitzky-golay-ellipsoid.png){: style="width:128px"}  |
| 2    | Cuboid    | ![](/assets/images/savitzky-golay-cuboid.png){: style="width:159px"}     |
