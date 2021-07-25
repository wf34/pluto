# Pluto: Motion Detection for Navigation in a VR Headset

Data, supplementing the [paper](https://arxiv.org/abs/2107.12030), provided in two flawors:
 * Motion detecter dataset, where data are preprocessed for off-the-shelf training of ML classification algorithms
 * Raw dataset, where data are vanilla

The Pluto dataset was taken with a headset prototype, spanning 30 min in time,
  where 4 subjects, varying in age and gender were asked to explore
  virtual reality scene, while moving freely and naturally.
Recorded trajectories are rich with sporadic movements, side- and backward steps,
  participants lean and change direction and orientation restlessly.
3 subject trajectores are in the `train` folder, one is in the `test`.

## Motion Detector Dataset
Each subject trajectores have corresponding input `inertial.csv` and ground truth `gt.csv` files

### Input format
First two columns describe event timestamps in seconds, former being the time of event
   and latter the events arrival time to the computer.
Both were preserved to model real system latency when needed. 3D accelerometer and gyroscope data follows.
These measurements are:
 * rotated into a world coordinate frame (as defined by the ground truth motion campure system)
 * accelerometer data hava a gravity vector substracted
 * accelerometer data is high-pass filtered

```
timestamp arrival_timestamp ax ay az wx wy wz
0.001034 0.003761 0.344765 -9.576807 -0.679953 -0.054328 -0.018109 -0.025566
0.002874 0.003841 0.000000 0.000000 0.000000 -0.053263 -0.017044 -0.028762
0.004825 0.007047 -0.000416 -0.018828 0.045381 -0.056459 -0.023436 -0.028762
0.006776 0.010303 0.006061 0.006494 -0.026956 -0.058590 -0.023436 -0.027697
0.008732 0.010388 0.004529 0.069129 -0.027299 -0.059655 -0.014914 -0.031958
```

### Groud truth format
A ground truth file holds event times; arrival timestamps were not retained and filled in with invalid data (-1).
Third column is system status: 0 for stillness, 1 for motion.

```
timestamp arrival_timestamp xx
0.001034 -1.000000 0.000000
0.002874 -1.000000 0.000000
0.004825 -1.000000 0.000000
0.006776 -1.000000 0.000000
0.008732 -1.000000 0.000000
```

Another ground truth representation was also supplied:  `gt_md_start_trajectory.csv`, `gt_md_stop_trajectory.csv` files.
That file has a timestamp entry for every event (start or stop) happepening in trajectory.
```
timestamp arrival_timestamp
9.258333 -1.000000
11.466666 -1.000000
16.475000 -1.000000
18.025000 -1.000000
19.866666 -1.000000
20.183333 -1.000000
```

## Raw Dataset
Each subject trajectores have corresponding `accelerometer.csv`, `gyroscope.csv`, `magnetometer.csv`, `temperature.csv` and ground truth `optitrack_gt.csv` files.
### Input format
Excerpt from `accelerometer.csv`:
```
timestamp arrival_timestamp ax ay az
0.001034 0.003761 0.344765 -9.576807 -0.679953
0.002874 0.003841 0.354342 -9.615114 -0.689530
0.004825 0.007047 0.354342 -9.653421 -0.603339
0.006776 0.010303 0.363919 -9.624691 -0.689530
```
The measurements are unprocessed data in a sensor coordinate frame, as registered by an IMU inside the head-mounted virtual reality device prototype. Trajectories in other CSV comply with that also.


### Groud truth format
Excerpt from `optitrack_gt.csv`:
```
timestamp arrival_timestamp rx ry rz tx ty tz
42.625000 42.625000 0.000942 0.104057 0.030928 -0.059133 0.000013 0.198530
42.633333 42.633333 0.001344 0.103925 0.030680 -0.059399 0.000122 0.199456
42.641666 42.641666 0.001065 0.103867 0.030849 -0.059645 0.000247 0.200441
42.666666 42.666666 0.000276 0.103466 0.031830 -0.060446 0.000752 0.203527
```
Positions are stored in a `global from local` coordinate frame, defined by the motion capture system, as an axis angle (`rx, ry, rz`) and a translation 3-vector (`tx, ty, tz`).

For further details, refer to the [paper](https://arxiv.org/abs/2107.12030). If the dataset is used in an academic work, please cite it.
