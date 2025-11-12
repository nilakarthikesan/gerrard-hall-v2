# How to Run the Visualizations

## Quick Start (All Visualizations)

### Step 1: Start the Server

Open your terminal and run:

```bash
cd /Users/nilakarthikesan/gerrard-hall-v2
python3 -m http.server 8000
```

Keep this terminal window open while you use the visualizations.

### Step 2: Choose Your Visualization

Open your browser and go to the **Viewer Selector**:
- **URL:** http://localhost:8000/web/viewer-select.html

This landing page lets you click on any of the three visualizations.

---

## Running Each Visualization Separately

### 1️⃣ Merge Tree (Frank's Literal Version)

**Purpose:** Shows the exact hierarchical merge process - how reconstructions appear and disappear as they merge.

**Steps:**
1. Make sure server is running: `python3 -m http.server 8000`
2. Open browser: http://localhost:8000/web/merge-tree.html
3. Use the controls:
   - **Timeline Slider:** Move through merge events
   - **Play Button:** Auto-animate the sequence
   - **Step Forward/Back:** Move one event at a time
   - **Tree Panel (left side):** See which reconstructions are visible

**What to Look For:**
- Watch the tree sidebar change colors as clusters become visible
- Notice how child reconstructions disappear when parent merges appear
- See the point count change in real-time

---

### 2️⃣ Cluster Animation (Cinematic Demo)

**Purpose:** Beautiful animation showing viewpoint clusters merging over time - great for presentations.

**Steps:**
1. Make sure server is running: `python3 -m http.server 8000`
2. Open browser: http://localhost:8000/web/cluster-animation.html
3. Use the controls:
   - **Timeline Slider:** Scrub through the animation (0-100%)
   - **Play Button:** Auto-play the full sequence
   - **Mouse:** Left-click to rotate, right-click to pan, scroll to zoom

**What to Look For:**
- Clusters start spatially separated (repulsed)
- They rotate and move toward the center
- Different colors for each viewing angle (front, side, back, roof)
- Smooth merging animations

---

### 3️⃣ Static Viewer (Comparison Tool)

**Purpose:** Traditional point cloud viewer for exploring individual reconstructions and comparing stages.

**Steps:**
1. Make sure server is running: `python3 -m http.server 8000`
2. Open browser: http://localhost:8000/web/index.html
3. Use the controls:
   - **Dropdown Menu:** Switch between different clusters/stages
   - **Point Size Slider:** Adjust point size
   - **Show Cameras Checkbox:** Toggle camera frustums
   - **Mouse:** Same as cluster animation

**What to Look For:**
- Compare `ba_input` vs `ba_output` vs `ba_gt`
- Explore individual clusters (C_1, C_2, C_3, C_4)
- See camera positions and orientations

---

## Troubleshooting

### Server won't start?
```bash
# Try a different port
python3 -m http.server 8080

# Then use http://localhost:8080/web/... in your browser
```

### Browser shows blank page?
- Check the browser console (F12 → Console tab)
- Make sure you're accessing via `http://localhost:8000` (not `file://`)
- Ensure the server is still running in your terminal

### Visualization loads but no points?
- Check that the data files exist in `data/gerrard-hall/results/`
- Look for error messages in the browser console
- Make sure you didn't move any files

---

## Stopping the Server

When you're done:
1. Go back to the terminal where the server is running
2. Press `Ctrl + C` to stop it

---

## Quick Reference URLs

Assuming server is running on port 8000:

| Visualization | URL |
|--------------|-----|
| **Viewer Selector** (Landing Page) | http://localhost:8000/web/viewer-select.html |
| **Merge Tree** (Frank's Version) | http://localhost:8000/web/merge-tree.html |
| **Cluster Animation** (Demo Version) | http://localhost:8000/web/cluster-animation.html |
| **Static Viewer** (Comparison Tool) | http://localhost:8000/web/index.html |

---

## For Presentations/Demos

**Best Order to Show:**
1. Start with **Viewer Selector** to explain the three options
2. Show **Merge Tree** to explain the GTSFM algorithm
3. Show **Cluster Animation** for the "wow factor"
4. Use **Static Viewer** for Q&A and detailed exploration

**Pro Tip:** Have multiple browser tabs open with each visualization pre-loaded so you can switch between them quickly!

