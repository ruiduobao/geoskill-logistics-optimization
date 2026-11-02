# Logistics Route Optimization (geoskill-logistics-optimization)

> VRP/TSP with time windows and capacity constraints to compute optimal routes and cost for logistics

---

## 1. Overview

Solves logistics delivery route optimization with two modes — TSP (traveling salesman) and VRP (capacity-constrained vehicle routing) — outputting optimal routes and total cost. TSP: nearest-neighbor heuristic + 2-opt local search to find the shortest tour that visits all nodes and returns to the depot. VRP: a greedy assignment of customers to vehicles ("demand descending + nearest loadable") under capacity constraints, followed by an internal TSP optimization for each vehicle; a time-window feasibility check is also provided. Distances support both Haversine (lon/lat) and Euclidean metrics.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-logistics-optimization.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `routes.geojson` | GeoJSON | Per-vehicle routes (LineString, with load/mileage) |
| `nodes.geojson` | GeoJSON | Depot and customer nodes (with demand) |
| `solution.json` | JSON | Optimization solution (vehicle count / total mileage / routes) |
| `output-manifest.json` | JSON | Run manifest |


## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
