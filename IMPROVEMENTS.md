# Visualization Improvements Based on GTSFM Paper

## Context from Paper

The paper "Distributed Global Structure-from-Motion with a Deep Front-End" (Baid et al.) provides critical context for understanding the Gerrard Hall clusters:

### Dataset Characteristics
- **Location:** UNC Chapel Hill campus building
- **Images:** 100 total images (object-facing capture)
- **Registered Cameras:** 98 out of 100 successfully localized
- **Complexity:** Medium-scale with hierarchical clustering

### What Clusters Represent

Clusters are **spatially coherent camera groups** that emerge from the distributed matching process:

1. **View Graph Formation:** Images are matched based on visual similarity (NetVLAD retrieval)
2. **Connected Components:** Matching creates connected components in the image graph
3. **Hierarchical Subdivision:** Large clusters are recursively split for parallel processing
4. **Skeletal Sets:** A minimal subset preserving geometric connectivity

## Visualization Enhancements

### 1. More Accurate Cluster Representation

**Current:** Clusters appear as generic colored blobs
**Should Be:** Clusters represent actual camera viewing directions

```javascript
// Add camera frustums to show what each cluster "sees"
// Color cameras by cluster membership
const frustumGroup = new THREE.Group();
for (let camera of clusterCameras) {
    const frustum = createCameraFrustum(camera);
    frustum.material.color = clusterColor;
    frustumGroup.add(frustum);
}
```

### 2. Show Temporal Progression More Accurately

Based on Table 3 metrics from the paper:

**Stage 0-20%:** Initial cluster detection
- Show 4 main clusters (C_1, C_2, C_3, C_4)
- Spatially separated based on viewpoint differences
- Display: ~25 cameras per cluster

**Stage 20-40%:** Hierarchical subdivision
- C_1 splits into C_1_1, C_1_2
- C_4 splits into C_4_1, C_4_2, then further into C_4_1_1, C_4_1_2
- Display: 8-15 cameras per sub-cluster

**Stage 40-60%:** Rotation averaging
- Clusters align their coordinate frames
- Show rotation angular errors decreasing (1.5-2.0° → 0.5°)
- Animate: Clusters rotating to align

**Stage 60-80%:** Translation averaging
- Clusters find correct relative positions
- Show translation errors decreasing
- Animate: Clusters moving to correct positions

**Stage 80-100%:** Bundle adjustment
- Points and cameras jointly optimized
- Reprojection errors < 1 pixel
- Display: Final unified reconstruction

### 3. Add Metric Overlays

Display real metrics from Table 3:

```javascript
// Show reconstruction quality metrics
const metrics = {
    'Rotation Error': '2.5° → 1.9° (after BA)',
    'Translation Error': '0.8° → 0.9°',
    'Tracks': '10,700 (filtered)',
    'Track Length': '4.0-4.9 views',
    'Reprojection Error': '0.8 pixels'
};
```

### 4. Visualize View Graph Evolution

Show the **image match graph** evolving:

**Initial:** Sparse connections (retrieved image pairs)
**Middle:** Dense intra-cluster connections
**Final:** Few inter-cluster connections (loop closures)

```javascript
// Draw lines between matched images
for (let match of imageMatches) {
    const line = new THREE.Line(
        new THREE.BufferGeometry().setFromPoints([
            camera_i.position,
            camera_j.position
        ]),
        new THREE.LineBasicMaterial({ 
            color: getClusterColor(i, j),
            opacity: 0.3 
        })
    );
}
```

### 5. Show Point Track Lengths

From the paper: average track length is 4.0-4.9 views

**Visualization:**
- Color points by track length (short = red, long = blue)
- Show which points are visible in multiple clusters
- Highlight "bridge points" that connect clusters

```javascript
const trackLengthColors = {
    2: 0xff0000,  // Red - weak
    3: 0xff8800,  // Orange
    4: 0xffff00,  // Yellow
    5: 0x88ff00,  // Yellow-green
    6: 0x00ff00,  // Green
    7: 0x00ff88,  // Cyan
    '8+': 0x0088ff  // Blue - strong
};
```

### 6. Accurate Spatial Layout

The paper shows Gerrard Hall has:
- **Clear building structure** (not arbitrary point cloud)
- **Facades captured from multiple angles**
- **Hierarchical viewpoint organization**

**Repulsion Phase Should:**
- Separate clusters by viewing angle (not randomly)
- Example: Front facade (C_1), Side (C_2), Back (C_3), Roof/High angle (C_4)

```javascript
// Position clusters based on viewing directions
const clusterPositions = {
    'C_1': { angle: 0,    distance: 3 },    // Front
    'C_2': { angle: 90,   distance: 3 },    // Right side  
    'C_3': { angle: 180,  distance: 3 },    // Back
    'C_4': { angle: 270,  distance: 3 }     // Left side/roof
};

function positionCluster(cluster, params) {
    cluster.position.x = Math.cos(params.angle * Math.PI/180) * params.distance;
    cluster.position.z = Math.sin(params.angle * Math.PI/180) * params.distance;
}
```

## Implementation Priority

1. **High Priority:**
   - Add camera frustums to show viewing directions
   - Position clusters based on actual viewing angles (not random)
   - Display real metrics from paper

2. **Medium Priority:**
   - Visualize view graph connections
   - Color by track length
   - Show rotation alignment animation

3. **Low Priority:**
   - Interactive metric overlays
   - Camera trajectory paths
   - Point track visualization

## References

- Paper: Baid et al., "Distributed Global Structure-from-Motion with a Deep Front-End"
- Figure 4, Row 5: Gerrard Hall visualization
- Table 3: Detailed reconstruction metrics
- Section 4.4: Discussion of reconstruction quality

