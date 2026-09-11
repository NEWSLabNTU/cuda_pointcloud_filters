# Contributing

This package is small and has one job: supply the two CUDA point cloud filters
Autoware does not ship. Changes that make it project-specific belong in the
project, not here.

## Before you send a change

- `colcon build --packages-select cuda_pointcloud_filters`
- `colcon test --packages-select cuda_pointcloud_filters`

The unit tests drive the filter classes against real device memory. There is no
CPU reference implementation to check against, and a mocked CUDA runtime would
only test the mock. They `GTEST_SKIP` when `cudaGetDeviceCount` reports no
device, so a build host without a GPU reports skipped rather than failed.

## Two invariants

Both exist because breaking them produces a plausible-looking cloud rather than
an error:

- **Non-finite points are dropped in both polarities.** Every comparison against
  NaN is false, so `inside` is false for a NaN point, and a `negative`
  implemented as `!inside` would keep it.
- **The crop box does not transform frames.** `input_frame` is an assertion: a
  cloud whose `frame_id` differs is dropped with an error. Cropping the right
  box in the wrong frame removes the wrong points, and nothing downstream
  reports it.

## Licence

Any contribution that you make to this repository will
be under the Apache 2 License, as dictated by that
[license](http://www.apache.org/licenses/LICENSE-2.0.html):

~~~
5. Submission of Contributions. Unless You explicitly state otherwise,
   any Contribution intentionally submitted for inclusion in the Work
   by You to the Licensor shall be under the terms and conditions of
   this License, without any additional terms or conditions.
   Notwithstanding the above, nothing herein shall supersede or modify
   the terms of any separate license agreement you may have executed
   with Licensor regarding such Contributions.
~~~

New files carry the same header as their neighbours.
