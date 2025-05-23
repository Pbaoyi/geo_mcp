# GeoMCP - 地理空间数据分析与可视化MCP服务

GeoMCP是一个基于MCP框架的地理空间数据分析与可视化服务，提供空间数据处理、地理编码、POI检索、路径规划、热点分析等地理空间分析功能，可与大语言模型（如ChatGPT、Claude）无缝集成，支持自然语言交互式空间数据分析。

## 功能特点

- **模块化设计**：核心功能模块化，便于扩展和维护
- **完善的类型提示**：使用Python类型注解，提高代码可读性和IDE支持
- **详细的日志记录**：基于logging模块的全面日志系统
- **统一的错误处理**：标准化的错误信息格式
- **完整的文档**：详尽的函数文档和使用说明

### 主要功能

- **空间数据读取**：支持CSV、Excel格式数据的读取与预览
- **测地线距离计算**：支持两点间地球曲面距离计算
- **热点分析（Getis-Ord Gi\*）**：支持自定义邻域距离的空间热点识别
- **距离分布分析**：快速统计距离字段的分布特征
- **高德地图API接口**：
  - 地址地理编码
  - 周边POI检索
  - 路径（驾车/步行/骑行/公交）距离计算

## 安装

### 从源码安装

```bash
# 克隆仓库
git clone https://github.com/yourusername/geo-mcp.git
cd geo-mcp

# 创建并激活虚拟环境（可选）
python -m venv venv
source venv/bin/activate  # Linux/Mac
# 或 venv\Scripts\activate  # Windows

# 安装依赖
pip install -e .
```

### 使用pip安装（未来支持）

```bash
pip install geo-mcp
```

## 配置

1. 复制示例配置文件`config.yaml.example`为`config.yaml`
2. 按需修改配置，尤其是高德地图API密钥（如需使用地图相关功能）

```yaml
# API配置
api:
  # 高德地图API密钥
  gaode_key: "your_gaode_api_key_here"
```

## 使用方法

### 作为命令行工具运行

```bash
# 直接运行模块
python -m geo_mcp

# 或使用安装后的命令行工具
geo-mcp
```

### 在MCP环境中使用

1. 启动MCP服务：`geo-mcp`
2. 在MCP客户端使用提供的工具函数

### 工具函数示例

#### 测地线距离计算

```python
coordinates = "39.90923_116.397428,31.23039_121.473702"
result = await calculate_geodesic_distance(coordinates)
print(result)
```

#### 数据文件读取预览

```python
file_path = "data/geo_hotspot_data.csv"
result = await read_table_file(file_path, nrows=5)
print(result)
```

#### 热点分析

```python
file_path = "data/geo_hotspot_data.csv"
result = await hotspot_analysis_getis_ord_gi_star(
    file_path, 
    lat_col="latitude", 
    lon_col="longitude", 
    value_col="value", 
    distance_threshold=1000
)
print(result)
```

#### 高德地图POI搜索

```python
location = "116.473168,39.993015"
result = await search_nearby_poi(location, keywords="餐馆", radius=1000)
print(result)
```

## 项目结构

```
geo_mcp/
├── api/                 # 第三方API相关功能
│   └── gaode_api.py     # 高德地图API封装
├── core/                # 核心功能模块
│   ├── data_processing.py  # 数据处理功能
│   ├── geo_distance.py     # 地理距离计算功能
│   └── spatial_analysis.py # 空间分析功能
├── tools/               # MCP工具相关功能
│   └── registration.py  # 工具注册和管理
├── utils/               # MCP工具函数
│   └── config.py        # 配置处理功能
├── __init__.py          # 包初始化
└── __main__.py          # 主入口程序

data/                   # 示例数据目录
├── geo_hotspot_data.csv # 热点分析示例数据
```

## 数据样例

`data/`目录中提供了热点分析的数据样例，可用于测试和学习系统功能。

## 依赖环境

- Python 3.8+
- geopy
- httpx
- mcp
- pandas
- numpy
- scipy
- scikit-learn
- shapely
- PyYAML

## 贡献指南

1. Fork项目仓库
2. 创建功能分支：`git checkout -b feature/xxx`
3. 提交变更：`git commit -m 'Add some feature'`
4. 推送到远程：`git push origin feature/xxx`
5. 提交Pull Request

## 许可证

本项目采用MIT许可证，详情见[LICENSE](LICENSE)文件。
