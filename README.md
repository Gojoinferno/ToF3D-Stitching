<p align="center">
  <img src="images/banner_tof3d_stitching.png" alt="ToF 3D Stitching: two Time-of-Flight cameras, one complete point cloud">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/3D-Time--of--Flight-1f3a5f?style=for-the-badge">
  <img src="https://img.shields.io/badge/Point%20Clouds-Open3D-1f3a5f?style=for-the-badge">
  <img src="https://img.shields.io/badge/Method-Point--cloud%20stitching-e8be60?style=for-the-badge">
</p>

<p align="center">
  <b>Two Time-of-Flight cameras look at an object from opposite corners.<br>
  Stitching their point clouds gives one complete 3D view of all sides.</b>
</p>

> **Source code is private.** It was developed in an industrial project. This page explains the method using my own test captures.

---

## 💡 How a Time-of-Flight camera sees in 3D

```mermaid
flowchart LR
    L["💡 The camera sends<br/>modulated infrared light"] --> O["📦 The light bounces<br/>off the object"]
    O --> S["📷 Every pixel measures<br/>the phase shift"]
    S --> D["⏱️ Phase shift → distance<br/>→ 3D point per pixel"]
```

Every pixel gets its own distance, so a single shot gives a full **point cloud**: thousands of X, Y, Z points. ToF needs no surface texture, so plain, uniform surfaces work well.

## 👀 Why one camera isn't enough

A camera at one corner sees the top and **only two sides**. The back is hidden. A second camera at the **diagonally opposite corner** sees the other two sides.

<p align="center"><img src="images/camera_layout_sketch.png" width="100%" alt="Camera arrangement: top view and side view"></p>

<sub>**①** camera 1 · **②** camera 2 at the opposite corner · **③** camera masts · **④** object · **⑤** pallet · **⑥** conveyor</sub>

## 🧩 Stitching two clouds into one

<p align="center"><img src="images/stitching_camera1_camera2_result.png" width="100%" alt="Camera 1 and camera 2 point clouds stitched into one"></p>

```mermaid
flowchart LR
    C1["📷 Camera 1<br/>point cloud"] --> F["Clean<br/>confidence · edges ·<br/>flying pixels"]
    C2["📷 Camera 2<br/>point cloud"] --> F
    F --> T["Transform both<br/>into one shared<br/>coordinate frame"]
    T --> P["Refine the seam<br/>small rotation<br/>correction"]
    P --> R["☁️ One complete<br/>point cloud"]
```

1. **Capture**: both ToF cameras record a point cloud.
2. **Clean**: low-confidence points, edge artefacts and flying pixels are removed.
3. **Transform**: each camera's known mounting pose moves its cloud into **one shared 3D coordinate frame**.
4. **Refine the seam**: a small rotation correction lines up the two halves where they meet.
5. **Result**: one cloud showing **all four sides** of the object.

## 🧰 Tech stack

**Python** · **Open3D** · **NumPy** · Time-of-Flight camera SDK

---

<p align="center">
  <b>Chiranthan Mahadeva</b> · Industrial machine vision & 3D<br>
  More: <a href="https://github.com/Gojoinferno/Stereo3D-BucketPose">Stereo3D-BucketPose</a> ·
  <a href="https://github.com/Gojoinferno/MachineVision-Research">MachineVision-Research</a>
</p>
