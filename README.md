# Gerrard Hall - GTSFM Cluster Reconstruction Visualization

Interactive 3D visualization demonstrating the hierarchical cluster merging process in Georgia Tech's Structure-from-Motion (GTSFM) pipeline.

![GTSFM Logo](https://img.shields.io/badge/GTSFM-3D%20Reconstruction-blue)
![Three.js](https://img.shields.io/badge/Three.js-Visualization-green)
![Status](https://img.shields.io/badge/Status-Demo-orange)

---

## Project Overview

Using Gerrard Hall as our test scene lets us visualize GTSFM's hierarchical cluster merging process—showing how separate reconstructions combine into a unified 3D model. I've been unable to fully run GTSFM or decompress the VVGT directory due to limited RAM, so I'm testing on a smaller cluster for now. The animation draws inspiration from "Building Rome in a Day," illustrating clusters repulsing and merging over time into the final reconstruction.

### What This Visualization Shows

This interactive visualization demonstrates the hierarchical cluster merging process in GTSFM's structure-from-motion pipeline for Gerrard Hall reconstruction. The animation shows how individual clusters (colored point clouds representing different parts of the building) begin spatially separated and progressively merge into a unified 3D reconstruction over time. Users can scrub through the reconstruction timeline to see the transition from 4 main clusters through sub-cluster refinement to the final merged model containing **23,841 3D points** from **84 camera views**.

---

## Quick Start

### Prerequisites
- Python 3.x (for local server)
- Modern web browser (Chrome, Firefox, Safari, Edge)
- No additional dependencies required!

### Running the Visualization

1. **Clone the repository:**
   ```bash
   git clone https://github.com/nilakarthikesan/gerrard-hall-v2.git
   cd gerrard-hall-v2
   ```

2. **Start a local HTTP server:**
   ```bash
   python3 -m http.server 8000
   ```
   
   Or if you prefer Node.js:
   ```bash
   npx http-server -p 8000
   ```

3. **Open in your browser:**
   - **Cluster Animation:** http://localhost:8000/web/cluster-animation.html
   - **Static Viewer:** http://localhost:8000/web/index.html

### Usage Instructions

#### **Cluster Animation (Recommended for Demos)**
- **Timeline Slider:** Scrub through the reconstruction process (0-100%)
- **Play Button:** Auto-animate the merging sequence
- **Mouse Controls:**
  - Left-click + drag: Rotate camera
  - Right-click + drag: Pan
  - Scroll: Zoom in/out

#### **Static Point Cloud Viewer**
- **Dropdown Menu:** Switch between different clusters and reconstruction stages
- **Point Size Slider:** Adjust point cloud density
- **Camera Controls:** Same as animation viewer

---

## Data Structure

```
data/gerrard-hall/results/
├── merged/                 # Final merged reconstruction (23,841 points)
├── C_1/merged/            # Cluster 1 (Red)
├── C_2/merged/            # Cluster 2 (Green)
├── C_3/merged/            # Cluster 3 (Blue)
├── C_4/merged/            # Cluster 4 (Yellow)
│   ├── C_4_1/merged/      # Sub-cluster 4.1
│   └── C_4_2/merged/      # Sub-cluster 4.2
├── ba_gt/                 # Ground truth bundle adjustment
├── ba_input/              # Bundle adjustment input
├── ba_output/             # Bundle adjustment output
└── metrics/               # Reconstruction metrics and reports
```

Each cluster directory contains:
- `points3D.txt` - 3D point cloud data (X, Y, Z, R, G, B)
- `cameras.txt` - Camera intrinsics
- `images.txt` - Camera poses (quaternion + translation)

---

## Visualization Features

### Animation Stages

1. **Stage 0-20%:** Individual clusters appear spatially separated (repulsed)
2. **Stage 20-40%:** Sub-clusters emerge and refine
3. **Stage 40-60%:** Local cluster merging begins
4. **Stage 60-80%:** Global alignment and progressive merging
5. **Stage 80-100%:** Final unified reconstruction

### Color Coding

- **Red** - Cluster 1
- **Green** - Cluster 2
- **Blue** - Cluster 3
- **Yellow** - Cluster 4
- **Purple** - Sub-clusters
- **Cyan** - Final merged reconstruction

---

## Why Gerrard Hall?

Gerrard Hall serves as our reference dataset because:

- **Medium-scale complexity:** Demonstrates GTSFM's full hierarchical clustering pipeline
- **Multiple merge events:** Shows both local and global cluster merging
- **Clear structure:** 4 main clusters with identifiable sub-clusters
- **Baseline comparison:** Same dataset used in the original GTSFM visualization

---

## Technical Challenges & Limitations

### Current Issues with GTSFM

I've encountered several technical limitations while working with the full GTSFM pipeline:

1. **Memory Constraints:**
   - Cannot fully render the complete Gerrard Hall reconstruction locally
   - Tens of thousands of points + 80+ camera poses overwhelm system RAM
   - Bundle adjustment and merging phases fail on my current hardware

2. **Large Archive Issues:**
   - Unable to decompress large result archives (Dubrovnik, VVGT datasets)
   - Decompression fails midway due to insufficient RAM
   - Cannot access raw cluster outputs for verification

3. **Workaround Strategy:**
   - Using smaller cluster subset (C_4.zip) provided by collaborator
   - Testing with partial merges until higher-memory machine is available
   - Planning to run full pipeline on GPU cluster or cloud instance

---

## Inspiration: Building Rome in a Day

This visualization is conceptually modeled after the seminal ["Building Rome in a Day"](http://grail.cs.washington.edu/rome/) project by Agarwal et al. (2009), which demonstrated:

- Large-scale distributed structure-from-motion
- Progressive merging of image clusters into cohesive 3D city models
- Vocabulary tree-based image matching
- Hierarchical reconstruction strategies

### Key Parallels

| Building Rome | Our Visualization |
|--------------|-------------------|
| City landmarks (Colosseum, Trevi) | Gerrard Hall building |
| 150K+ Flickr images | 100 calibrated images |
| Distributed cluster matching | GTSFM hierarchical clustering |
| Visual merge progression | Animated cluster convergence |

Our approach makes GTSFM's internal clustering and merging stages **visually intuitive**, helping explain how distributed SfM evolves over time rather than just showing the final point cloud.

---

## Technology Stack

- **Three.js** - 3D rendering and point cloud visualization
- **OrbitControls** - Interactive camera controls
- **Vanilla JavaScript** - No framework dependencies
- **COLMAP Format** - Standard SfM data format
- **Python HTTP Server** - Simple local hosting

---

## Related Work

- **GTSFM:** [Georgia Tech Structure from Motion](https://github.com/borglab/gtsfm)
- **Building Rome in a Day:** [Paper (ICCV 2009)](http://grail.cs.washington.edu/rome/rome_paper.pdf)
- **COLMAP:** [SfM & MVS Pipeline](https://colmap.github.io/)
- **Three.js:** [3D Graphics Library](https://threejs.org/)

---

## Data Format

### Point Cloud Format (`points3D.txt`)
```
# POINT3D_ID, X, Y, Z, R, G, B, ERROR, TRACK[] as (IMAGE_ID, POINT2D_IDX)
0 0.559 -1.084 0.717 64 57 51 0.42 21 0 22 0 23 0 ...
```

### Camera Poses Format (`images.txt`)
```
# IMAGE_ID, QW, QX, QY, QZ, TX, TY, TZ, CAMERA_ID, NAME
1 0.999 0.001 -0.034 0.013 -0.123 0.456 1.789 1 image001.jpg
```

---

## Future Work

- [ ] Deploy to GitHub Pages for online access
- [ ] Add camera trajectory visualization
- [ ] Implement cluster selection and isolation
- [ ] Add metrics overlay (reprojection error, track lengths)
- [ ] Export animation as video
- [ ] Support for larger datasets (VVGT, city-scale)
- [ ] GPU-accelerated rendering for massive point clouds

---

## Screenshots

### Cluster Animation
*Individual clusters spatially separated (Stage 0)*

### Final Merged Reconstruction
*Unified 3D model with 23,841 points (Stage 100)*

---

## Acknowledgments

- **GTSFM Team** at Georgia Tech for the reconstruction pipeline
- **Kathir** for providing the Gerrard Hall dataset and C_4 cluster
- **Building Rome in a Day** team for inspiration
- **Three.js Community** for excellent 3D visualization tools

---

## License

This visualization code is provided for educational and research purposes.

The Gerrard Hall reconstruction data is courtesy of the GTSFM project at Georgia Tech.

---

## Contact

For questions or collaboration:
- GitHub: [@nilakarthikesan](https://github.com/nilakarthikesan)
- Repository: [gerrard-hall-v2](https://github.com/nilakarthikesan/gerrard-hall-v2)

---

**Built for advancing 3D reconstruction visualization**

