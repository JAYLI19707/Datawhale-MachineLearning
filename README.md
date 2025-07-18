# 新用户预测 Baseline

本项目提供了一系列解决新用户预测问题的 Baseline 方案，通过逐步的特征工程和模型优化，展示了从 0.63 到 0.97 的分数提升过程。

*截至目前：2025年07月18日 17:50*

## 思路过程文字描述

### 整体解题思路

这个新用户预测项目的核心挑战在于如何从用户的历史行为数据中识别出新用户的模式。我的解题思路经历了从简单到复杂、从通用到针对性的演进过程：

**1. 数据理解阶段**
- 首先深入分析数据结构，发现用户行为数据包含时间序列信息、用户标识(did)、以及复杂的udmap特征
- 通过数据探索发现一个关键规律：约93%的用户是老用户，这为后续的策略制定提供了重要依据

**2. 特征工程演进路径**
- **基础阶段**：从Datawhale官方Baseline(0.63)开始，学习基本的数据预处理和模型训练流程
- **用户画像阶段**：意识到用户个体特征的重要性，开始构建基于用户ID(did)的聚合特征，包括行为频次、时间分布等
- **RFM建模阶段**：引入经典的RFM(最近性、频率、金额)模型，将用户行为量化为可解释的商业指标，实现了显著的分数提升(0.63→0.86)
- **参数优化阶段**：通过网格搜索等超参数调优技术，挖掘模型的最大潜力(0.86→0.94)

**3. 模型可靠性保障**
- 发现并修复了特征构造过程中的数据泄露问题，确保模型的泛化能力
- 虽然修复后分数略有下降，但这是为了保证模型在真实场景下的可靠性

**4. 深度特征挖掘**
- 深入解析udmap特征列，提取更细粒度的信息
- 探索特征间的交叉组合效应，发现隐藏的数据模式(0.94→0.95)

**5. 竞赛策略应用**
- 基于数据分布的先验知识，将用户ID直接作为特征，让模型学习到强规则
- 这是一种针对比赛场景的策略性做法，最终实现了0.95→0.97的提升

### 核心技术洞察

1. **特征工程是王道**：每一次显著的分数提升都来自于更好的特征构造，而不是复杂的模型架构
2. **用户行为的时序性很重要**：RFM模型之所以有效，是因为它捕捉了用户行为的时间模式
3. **数据泄露防范至关重要**：看似简单的时间划分问题，实际上可能导致严重的过拟合
4. **领域知识的价值**：了解业务背景(93%老用户)可以为特征工程提供重要指导
5. **平衡可解释性与性能**：在追求高分的同时，也要考虑模型的实际应用价值

这个项目展现了机器学习项目的典型演进过程：从数据探索到特征工程，从模型调优到问题修复，最后到策略性优化。每个阶段都有其独特的价值和学习意义。

## 项目展示

![项目截图1](屏幕截图%202025-07-18%20174706.png)

![项目截图2](屏幕截图%202025-07-18%20174753.png)

## 各方案介绍

### 1. Datawhale 夏令营 Baseline (0.63)
本项目始于 Datawhale 夏令营提供的官方 Baseline，分数为 **0.63**。后续的优化工作都是在此基础上展开。

### 2. `Baseline_RFM.ipynb`：用户层建模 (0.63 -> 0.86)
该方案的核心是对用户（`did`）进行深度特征工程，从用户行为中提取有价值的信息。
- **主要工作**：
  - 对每个 `did` 进行特征聚合。
  - 构建了行为频次、时间分布以及经典的 RFM (Recency, Frequency, Monetary) 特征。
- **成果**：通过精细的用户特征构建，模型分数从 0.63 显著提升至 **0.86**。

### 3. `Baseline_grid.ipynb`：网格搜索调优 (0.86 -> 0.94)
在 RFM 特征的基础上，此方案重点在于通过超参数搜索，寻找模型的最佳性能点。
- **主要工作**：
  - 使用网格搜索（Grid Search）方法。
  - 调整并优化了模型的参数搜索空间。
- **成果**：精调后的模型将分数从 0.86 进一步提升至 **0.94**。

### 4. `Baseline_grid_fixed.ipynb`：修复数据泄露问题
此版本是对 `Baseline_RFM.ipynb` 中一个数据泄露问题的修复。
已经是在`Baseline_grid.ipynb`的基础上修改。
- **主要工作**：
  - 修正了在构造 RFM 特征时，因数据划分不当导致的信息泄露。
- **说明**：修复后分数略有下降，这是正常现象，因为它保证了模型在未知数据上具有更可靠的泛化能力。

### 5. `Baseline_enhanced_features.ipynb`：深化特征工程 (0.94 -> 0.95)
此方案在现有特征基础上，进一步挖掘 `udmap` 列的潜力，并探索特征间的组合效应。
- **主要工作**：
  - 解析 `udmap` 特征列，提取更细粒度的信息。
  - 创建了新的交叉组合特征。
- **成果**: 分数从 0.94 轻微提升至 **0.95**。

### 6. `rule_bug.ipynb`：利用先验知识刷分(0.95->0.97)
这是一个利用数据分布特性进行针对性提分的策略。
- **主要工作**：
  - 基于一个重要的先验知识：数据集中约 93% 的用户是老用户。
  - 将用户ID（`did`）直接作为一个特征送入模型，让模型学习到这个强规则。
- **目的**：在比赛场景下，利用数据规律获得更高的分数。

## 数据集
- `train.csv` - 训练数据（文件较大，未上传到GitHub）
- `testA_data.csv` - 测试数据（文件较大，未上传到GitHub）
- `submit.csv` - 提交格式样例

**数据下载地址**：https://challenge.xfyun.cn/topic/info?type=subscriber-addition-2025&option=stsj

**注意**：由于数据文件较大（train.csv 约283MB，testA_data.csv 约92MB），超过了GitHub的文件大小限制，因此未包含在此仓库中。请通过上述链接下载数据文件。

## 项目博客

详细的技术分享和思路过程请访问我的博客：[https://jayli19707.github.io/](https://jayli19707.github.io/)

## 免责声明

本仓库单纯分享学习过程，本人属于机器学习初入门水平，很多底层类的原理不是太理解，很多做法也是通过AI辅助，本仓库记录学习思路的过程，希望能帮到也在学习的你。

## 致谢

感谢Datawhale夏令营，让自己对机器学习有个初步的认识，大致了解特征工程的过程，还有了解如何处理数据集，需要修改那些参数提高模型准确度。感谢机器学习11群的同学们分享的思路，指引我的思考方向。

MIT License

Copyright (c) 2025 JAYLI19707

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

