# 物流路径优化 | Logistics Route Optimization

求解物流配送路径优化，支持 TSP（旅行商）与 VRP（带容量车辆路径）两种模式，输出最优路径与总成本。

TSP：最近邻启发式 + 2-opt 局部搜索求访问所有节点并返回起点的最短回路。VRP：按“需求降序 + 最近可装车”贪心把客户分配到车辆（容量约束），每辆车内部再做 TSP 优化；另提供时间窗可行性检查。距离支持 Haversine（经纬度）与欧氏两种度量。

## 依赖

```bash
pip install 'numpy' 'scipy' 'geopandas' 'shapely'
```

## 使用方法

### 基本用法

```bash
python geoskill-logistics-optimization.py --bbox 116.0 39.0 117.0 40.0 [其他参数]
```

### 示例 1（合成节点跑 TSP，离线）

```bash
python geoskill-logistics-optimization.py --bbox 116 39 117 40 --synthetic --mode tsp --output-dir ./out
```

### 示例 2（合成节点跑 VRP）

```bash
python geoskill-logistics-optimization.py --bbox 116 39 117 40 --synthetic --mode vrp --capacity 12 --output-dir ./out
```

### 示例 3（真实节点 GeoJSON（首点为仓库））

```bash
python geoskill-logistics-optimization.py --input nodes.geojson --mode vrp --capacity 15 --output-dir ./out
```

### 示例 4（欧氏距离 + 关闭 2-opt（更快））

```bash
python geoskill-logistics-optimization.py --input nodes.geojson --mode tsp --metric euclidean --no-2opt --output-dir ./out
```

### 示例 5（更多合成客户）

```bash
python geoskill-logistics-optimization.py --bbox 116 39 117 40 --synthetic --mode vrp --n-customers 30 --output-dir ./out
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `routes.geojson` | GeoJSON | 各车辆路径（LineString，含载重/里程） |
| `nodes.geojson` | GeoJSON | 仓库与客户节点（含需求） |
| `solution.json` | JSON | 优化解（车辆数/总里程/各路线） |
| `output-manifest.json` | JSON | 运行清单 |

## 数据源 / Source

GeoJSON 点要素，第一个点为仓库 depot，其余为客户；可用属性 demand 指定需求。 或使用 `--synthetic` 生成物理一致的模拟数据（完全离线）。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传用户数据。

## License

MIT
