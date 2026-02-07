# 人工智能概念地图（System → Model → Algorithm → Architecture）

> 核心目的：理清「模型 / 算法 / 网络 / 学习 / 推理 / CNN / Transformer」等概念的层级关系

---

## 一、系统层（AI System Level）

关注点：整体智能系统如何运作，而不关心模型内部细节

```bash
环境 / 用户
↓
感知模块
↓
模型（学习 + 推理）
↓
决策模块
↓
执行模块
↓
反馈 / 评估
```

### 关键词

- AI System
- Agent（智能体）
- 闭环控制
- 自动 / 半自动 / 智能系统

---

## 二、模型层（Model Level）

### 定义

> **模型 = 从输入到输出的函数 + 内部状态（参数）**

模型层只关心两件事：

模型 = 学习（Learning） + 推理（Inference / Reasoning）

---

### 2.1 学习（Learning）

**解决问题：模型参数是如何得到的？**

#### 常见学习范式

学习方式
├─ 监督学习
├─ 无监督学习
├─ 自监督学习
├─ 半监督学习
└─ 强化学习

> 学习 ≠ 网络  
> 学习 = 参数更新规则

---

### 2.2 推理（Inference / Reasoning）

**解决问题：给定参数，模型如何产生输出？**

#### 推理范式分类

推理范式
├─ 符号推理（Symbolic Reasoning）
├─ 概率推理（Probabilistic Reasoning）
├─ 神经推理（Neural Reasoning）
└─ 混合推理（Neuro-Symbolic）

> 推理 ≠ 学习  
> 推理发生在“已训练模型”之上

---

## 三、算法层（Algorithm Level）

### 定义

> **算法 = 实现学习或推理的具体计算过程**

算法解决的是「怎么做」，而不是「表示什么」。

---

### 3.1 学习算法（训练阶段）

学习算法
├─ 梯度下降（SGD / Adam）
├─ 反向传播（Backpropagation）
├─ EM 算法
├─ Q-learning
├─ Policy Gradient
└─ Monte Carlo Tree Search（MCTS）
---

### 3.2 推理算法（运行阶段）

推理算法
├─ 贪心搜索（Greedy）
├─ Beam Search
├─ Viterbi
├─ 贝叶斯更新
├─ Monte Carlo Sampling
├─ Tree Search
└─ 约束求解（Constraint Solving）
> 推理算法 ≠ 推理范式  
> 推理算法是“实现手段”，推理范式是“思想层次”

---

## 四、表示 / 网络结构层（Representation / Architecture）

### 定义

> **网络结构 = 参数如何组织 + 输入如何被表示**

这是 CNN、Transformer 所在的层级。

---

### 4.1 常见网络结构

网络结构
├─ 线性模型
├─ MLP（全连接网络）
├─ CNN（卷积神经网络）
├─ RNN / LSTM / GRU
├─ Transformer
└─ GNN（图神经网络）
> 网络结构决定模型的 **归纳偏置（Inductive Bias）**

---

### 4.2 CNN / Transformer 的准确位置

| 名称 | 层级 | 说明 |
|----|----|----|
| CNN | 网络结构 | 基于局部感受野与参数共享 |
| Transformer | 网络结构 | 基于自注意力的全局建模 |
| ViT | 具体模型 | Transformer 用于视觉 |
| BERT | 具体模型 | Transformer + 预训练 |
| GPT | 具体模型 | 自回归 Transformer |

---

## 五、具体模型实例（Concrete Model Level）

### 定义
> **模型实例 = 网络结构 + 已训练参数 + 明确用途**

具体模型
├─ ResNet-50
├─ YOLOv8
├─ CLIP
├─ Stable Diffusion
└─ GPT-4 / GPT-5
这些是**可以直接加载、部署、使用的模型对象**。

---

## 六、概念快速归位表（非常重要）

| 概念 | 所属层级 |
|----|----|
| AI 系统 | 系统层 |
| Agent | 系统层 |
| 模型 | 模型层 |
| 学习 | 模型层 |
| 推理 | 模型层 |
| 算法 | 算法层 |
| SGD / Adam | 算法层 |
| 推理范式 | 模型层 |
| 推理算法 | 算法层 |
| CNN | 网络结构层 |
| Transformer | 网络结构层 |
| GPT / BERT | 具体模型 |

---

## 七、关键关系总结（强烈建议记住）

模型
├─ 用某种网络结构表示
├─ 用某种学习算法训练
└─ 用某种推理算法运行
---

## 八、压缩记忆版总结（核心结论）

> **模型是功能抽象，算法是实现过程，网络是参数结构，学习决定参数来源，推理决定参数如何被使用，而 CNN 与 Transformer 只是众多网络结构中的两类。**

---
