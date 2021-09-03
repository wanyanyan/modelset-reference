# 概述

> 本规范定义了大场景三维模型数据集的组织结构，在三维模型数据生产时应该根据该规范创建模型的配置文件，开发人员根据该规范进行三维模型的数据调度与可视化。


## 总体结构

```json
{
  "name": "汇博苑",
  "bbox": {
    "min": {
      "x": -10,
      "y": 0,
      "z": 0
    },
    "max": {
      "x": 50,
      "y": 150,
      "z": 90
    }
  },
  "origin": [114.3241, 30.8652],
  "des": "这是一个位于华中科技大学东边的高档小区，建于2015年，总建筑面积2.34万㎡。",
  "metadata": {},
  "models": [...]
}
```

以下是对每个属性的详细说明：

**name**

这个三维数据集的名称

**bbox**  *（可选）*

三维数据集的包围盒，即坐标范围。该字段为可选字段，主要用于优化可视化效果

**origin**

该数据集原点的地理坐标，用经纬度表示

**des**  *（可选）*

该数据集的描述信息。可选字段

**medadata**   *（可选）*

元数据信息，可以放任何必要的属性信息

**models**

该数据集包含的模型列表

## 模型列表

*models*定义了该数据集的模型列表，模型列表中支持三种类型的模型：实体模型、虚拟模型、模型子集

- 实体模型

```json
{
  "id": "hhh01",
  "type": "model",
  "name": "汇博苑1号楼",
  "format": "gltf",
  "model": "hby-01.glb",
  "properties": {
    "area": 1200,
    "floors": 32
  }
}
```

- 虚拟模型

虚拟模型是通过实例化的方式对实体模型进行引用，本身不存在实体文件。

```json
{
  "id": "hhh02",
  "type": "virtual_model",
  "name": "汇博苑2号楼",
  "ref": "hhh01",
  "location": [100, 32, 109],
  "properties": {
    "area": 1200,
    "floors": 32
  }
}
```

- 模型子集

模型子集不是一个具体的模型，是一系列模型的集合，相当于一个子数据集，例如一个门可以包含门板和门把手两个子模型。

模型子集中的模型列表同样支持以上这三种模型类型，即模型子集中也可以有实体模型、虚拟模型和模型子集。

```json
{
  "id": "hhh02",
  "type": "group",
  "name": "汇博苑3号楼",
  "properties": {
    "area": 1200,
    "floors": 32
  },
  "models": [...]
}
```

相关字段说明：

**id**

每个模型的唯一编码，可以是任意字符串

**type**

模型的类型，包括三种：实体模型（`model`）、虚拟模型（`virtual_model`）、模型子集（`group`）

**name**

模型的名称

**format**

*只有实体模型需要该字段*

模型的格式，支持以下几种格式：`gltf`、`obj`、`drc`、`fbx`、`ifc`。
需要注意的是，当格式为gltf时，模型可以是gltf或glb格式。当格式为drc或obj时，需要有同名的mtl纹理文件与模型在同一个位置。

**model**

*只有实体模型需要该字段*

模型的文件名，包含拓展名一起

**rel**

*只有虚拟模型需要该字段*

引用的实体模型或模型子集的id

**location**

*只有虚拟模型需要该字段*

虚拟模型的位置，分别是[x, y, z]

**models**

*只有模型子集需要该字段*

模型子集包含的模型列表

**properties**

模型挂接的外部属性

## 一个完整的配置文件示例

```json
{
  "name": "汇博苑-1号楼",
  "bbox": {
    "min": {
      "x": -10,
      "y": 0,
      "z": 0
    },
    "max": {
      "x": 50,
      "y": 150,
      "z": 90
    }
  },
  "origin": [114.3241, 30.8652],
  "des": "这是一个位于华中科技大学东边的高档小区，建于2015年，总建筑面积2.34万㎡。",
  "metadata": {},
  "models": [
    {
      "id": "f1",
      "type": "group",
      "name": "1楼",
      "properties": {
        "floorHeight": 3
      },
      "models": [
        {
          "id": "f11",
          "type": "model",
          "name": "架空层",
          "format": "gltf",
          "model": "HBY_1D_01F_LOD3_001.glb",
          "properties": {
            "单元号": "1",
            "类型": "公共空间",
            "房号": "架空"
          }
        },
        {
          "id": "f12",
          "type": "model",
          "name": "地下室用管井",
          "format": "gltf",
          "model": "HBY_1D_01F_LOD3_003.glb",
          "properties": {
            "单元号": "1",
            "类型": "公共空间",
            "房号": "管井"
          }
        }
      ]
    },
    {
      "id": "f2",
      "type": "group",
      "name": "2楼",
      "properties": {
        "floorHeight": 3
      },
      "models": [
        {
          "id": "f21",
          "type": "model",
          "name": "1号房",
          "format": "gltf",
          "model": "HBY_1D_02F_LOD3_001.glb",
          "properties": {
            "单元号": "1",
            "类型": "房间",
            "房号": "1"
          }
        },
        {
          "id": "f22",
          "type": "model",
          "name": "2号房",
          "format": "gltf",
          "model": "HBY_1D_02F_LOD3_003.glb",
          "properties": {
            "单元号": "1",
            "类型": "房间",
            "房号": "2"
          }
        },
        {
          "id": "f23",
          "type": "model",
          "name": "电梯间",
          "format": "gltf",
          "model": "HBY_1D_02F_LOD3_002.glb",
          "properties": {
            "单元号": "1",
            "类型": "公共空间",
            "房号": "电梯间"
          }
        }
      ]
    },
    {
      "id": "f3",
      "type": "virtual_model",
      "name": "3楼",
      "ref": "f2",
      "location": [0, 9, 0],
      "properties": {
        "floorHeight": 3
      }
    },
    {
      "id": "f4",
      "type": "virtual_model",
      "name": "4楼",
      "ref": "f2",
      "location": [0, 12, 0],
      "properties": {
        "floorHeight": 3
      }
    },
    {
      "id": "ff",
      "type": "model",
      "name": "顶楼",
      "format": "gltf",
      "model": "HBY_1D_31F_LOD3_001.glb",
      "properties": {
        "floorHeight": 2
      }
    }
  ]
}
```