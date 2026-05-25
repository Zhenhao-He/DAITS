# DAITS
DAITS is a driver attention prediction dataset collected in interactive and dynamic traffic scenarios. Inspired by the NuPlan dataset format, DAITS provides synchronized driving scene images, driver gaze annotations, fixation maps, attention maps, point clouds, ego-motion information, maneuver labels, and structured database metadata.

# Overview
The dataset is available at [DAITS](https://huggingface.co/datasets/hezhenhao/DAITS). DAITS is designed to support research on driver attention allocation in complex traffic interactions. The sample sequence `demo_dataset` includes:
- Front-view driving images and video.
- Driver gaze point annotations.
- Fixation maps and dense attention maps.
- Merged LiDAR point clouds.
- Ego-vehicle motion states and maneuver categories.
- A SQLite database containing scene, camera, LiDAR, ego pose, track, traffic light, and scenario metadata.
- MATLAB matrix files for pixel-level attention supervision.

The repository also provides official sequence split files:
- `train+val.txt`: sequence IDs used for training and validation.
- `test.txt`: sequence IDs used for testing.

Each line in these files is a sequence name, such as `boston-1`, `pittsburgh-2`, `singapore-4`, or `vegas-0`. When training or evaluating a model, please split the dataset according to these two files instead of randomly splitting frames, so that data from the same driving sequence does not appear in both training and test sets.

## Data Description
### Dataset Split
DAITS should be divided at the sequence level using the provided split files:

```text
train+val.txt
test.txt
```

`train+val.txt` contains the sequence names for the training subset, and `test.txt` contains the sequence names for the held-out test subset. 



Each sequence name corresponds to a dataset sequence directory. For example, `boston-1` refers to the sequence folder named `boston-1` in the full dataset. The included `demo_dataset` folder is only a sample for illustrating the file format.

### Image and Video
- `image`: RGB driving scene images with a resolution of `3840 x 1080`.
- `2021.09.15.12.32.43_veh-28_00708_00866-0.avi`: driving scene video with a resolution of `3840 x 1080`, recorded at `10 fps` using Motion JPEG.
- `video_files.json`: metadata describing the frame range of the video sequence. In the sample sequence, it stores the first and last frame file names.
### Gaze Points
The `gaze_point` directory contains driver gaze annotations in JSON format.
- `0_gaze_point.json` to `6_gaze_point.json`: gaze annotations from individual annotation sources or participants.
- `combone_gaze_point.json`: combined gaze annotations merged from multiple sources.
### Fixation and Attention Maps
- `fixation`: grayscale fixation maps with a resolution of `3840 x 1080`.
- `map`: grayscale attention or saliency maps with a resolution of `3840 x 1080`.
- `mat`: MATLAB v5 files containing `mat_data` with shape `(1080, 3840)` and data type `uint8`.
These files can be used as dense supervision targets for driver attention prediction.
### Point Cloud
- `MergedPointCloud`: merged point clouds stored in PCD v0.7 format.
### Motion and Maneuver Labels
The `label` file stores the ego-vehicle motion state and the corresponding high-level maneuver category.
Example:
```json
{
  "motion": [
    331782.04008395976,
    4690692.307180318,
    -4.695628576150617,
    0.26354199884490226,
    0.004201419694238835,
    0.002873082804781658,
    0.9646344946727663,
    3.482486376683642,
    0.07173694310159753,
    0.040234449221355675,
    -0.9573170749897498,
    0.004436344331358788,
    0.22219665632401128,
    0.00011281752840246499,
    0.00012944988654382155,
    -0.0033487373291798714
  ],
  "maneuver category": "Straight-line Deceleration"
}
```
The motion field is a 16-dimensional vector describing the ego-vehicle state:

| Index Range | Field | Description | Dimension |
|---|---|---|---|
| `0:3` | Position | Ego-vehicle position `(x, y, z)` | 3 |
| `3:7` | Quaternion | Ego-vehicle orientation `(qx, qy, qz, qw)` | 4 |
| `7:10` | Velocity | Linear velocity `(vx, vy, vz)` | 3 |
| `10:13` | Acceleration | Linear acceleration `(ax, ay, az)` | 3 |
| `13:16` | Angular Rate | Angular velocity `(wx, wy, wz)` | 3 |

The maneuver category field provides the high-level driving behavior label of the current frame or segment. Example categories include:

* Straight-line Driving
* Straight-line Acceleration
* Straight-line Deceleration
* Left Turn
* Right Turn
* Lane Change
* Stopping

These annotations provide additional behavioral context for driver attention prediction, scene understanding, and motion-aware modeling.

### SQLite Metadata

The .db file is a SQLite database containing structured scene metadata. The sample database includes the following tables:
```
camera
category
ego_pose
image
lidar
lidar_box
lidar_pc
log
scenario_tag
scene
track
traffic_light_status
```
This metadata can be used to align camera frames, LiDAR frames, ego poses, object tracks, traffic light states, and scenario tags.

# License

The DAITS dataset is released under the CC BY-NC 4.0 license for non-commercial research and educational purposes only.

Commercial use of the dataset is prohibited without explicit permission from the authors.

# Citation

If you find DAITS useful in your research or applications, please consider citing the following paper:
```
@article{heNovelDatasetModel2026,
  title = {A Novel Dataset and Model for Driver Attention Prediction in Interactive Dynamic Traffic},
  author = {He, Zhenhao and Li, Qian and Nie, Linzhen and Yin, Zhishuai},
  year = {2026},
  month = aug,
  journal = {Expert Systems with Applications},
  volume = {323},
  pages = {132493},
  issn = {0957-4174},
  doi = {10.1016/j.eswa.2026.132493}
}
```
# Acknowledgement
- [Nuplan](https://github.com/motional/nuplan-devkit)
