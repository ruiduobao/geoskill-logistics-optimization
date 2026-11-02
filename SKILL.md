---
name: geoskill-logistics-optimization
description: 'VRP/TSP with time windows and capacity constraints to compute optimal routes and cost for logistics'
---

# 物流路径优化 | Logistics Route Optimization

Solves logistics distribution route optimization with two modes — TSP (traveling salesman) and VRP (capacitated vehicle routing) — and outputs the optimal routes and total cost.

TSP: nearest-neighbor heuristic + 2-opt local search to find the shortest tour that visits all nodes and returns to the start. VRP: a greedy "demand descending + nearest feasible vehicle" rule assigns customers to vehicles (capacity constraint), and each vehicle's route is then internally optimized with TSP; a time-window feasibility check is also provided. Distances support both Haversine (lon/lat) and Euclidean metrics.

## Dependencies / 依赖

```bash
pip install 'numpy' 'scipy' 'geopandas' 'shapely'
```

## Usage / 使用方法

### Basic usage

```bash
python geoskill-logistics-optimization.py --bbox 116.0 39.0 117.0 40.0 [other parameters]
```

### Example 1 (TSP on synthetic nodes, offline)

```bash
python geoskill-logistics-optimization.py --bbox 116 39 117 40 --synthetic --mode tsp --output-dir ./out
```

### Example 2 (VRP on synthetic nodes)

```bash
python geoskill-logistics-optimization.py --bbox 116 39 117 40 --synthetic --mode vrp --capacity 12 --output-dir ./out
```

### Example 3 (real nodes GeoJSON, first point is the depot)

```bash
python geoskill-logistics-optimization.py --input nodes.geojson --mode vrp --capacity 15 --output-dir ./out
```

### Example 4 (Euclidean distance + 2-opt disabled (faster))

```bash
python geoskill-logistics-optimization.py --input nodes.geojson --mode tsp --metric euclidean --no-2opt --output-dir ./out
```

### Example 5 (more synthetic customers)

```bash
python geoskill-logistics-optimization.py --bbox 116 39 117 40 --synthetic --mode vrp --n-customers 30 --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `routes.geojson` | GeoJSON | Per-vehicle routes (LineString, with load/mileage) |
| `nodes.geojson` | GeoJSON | Depot and customer nodes (with demands) |
| `solution.json` | JSON | Optimal solution (vehicle count/total mileage/per-route detail) |
| `output-manifest.json` | JSON | Run manifest |

## Data Source / 数据源 / Source

GeoJSON point features; the first point is the depot and the rest are customers; demand can be specified via the `demand` attribute. Alternatively, use `--synthetic` to generate physically consistent simulated data (fully offline).

## Privacy / 隐私声明 / Privacy

- Offline by default; `--synthetic` mode requires no network at all.
- All processing is done locally; no user data is uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
