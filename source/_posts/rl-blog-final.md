---
title: '从 AlphaGo 到 LLM：强化学习的前世今生与大模型时代'
date: 2026-07-01 14:46:47
tags: [rl, reinforcement learning, 强化学习]
mathjax: true
comments: true
---

> **摘要：** 强化学习（Reinforcement Learning, RL）从上世纪 50 年代的早期构想，到 AlphaGo 的里程碑式突破，再到如今在大语言模型（LLM）中的全面渗透，走过了一条波澜壮阔的技术演进之路。本文回顾了 RL 的核心发展脉络，并重点聚焦 RL 如何与 LLM 结合——从 RLHF 到 PPO、DPO，再到 GRPO、BRPO 等最新算法，梳理其背后的技术逻辑、演进逻辑与前沿趋势。

---

## 一、强化学习：一段简史

### 1.1 萌芽与奠基（1950s–1980s）

强化学习的思想源头可以追溯到行为心理学中的**操作性条件反射**（Skinner, 1938）。20 世纪 50 年代，Arthur Samuel 在跳棋程序中提出了"强化学习"的雏形概念。1951 年，Homer Walker 在哈佛 Mark I 计算机上实现了世界上第一个强化学习代理。

真正的理论奠基来自 **Richard Bellman** 在 1957 年提出的**动态规划**和**贝尔曼方程**，它为强化学习提供了数学基础——将价值函数定义为即时奖励与未来折扣奖励之和。

### 1.2 策略梯度与理论完善（1990s）

90 年代是 RL 走向算法化的关键时期：

- **1989**，Chris Watkins 提出了 **Q-Learning**，一种无需模型（model-free）的时序差分学习方法，通过更新 Q 表来学习最优动作价值函数。该论文随后在 Watkins & Barto（1989）中正式发表，奠定了无模型 RL 的基石。
- 1996 年，**SARSA**（State-Action-Reward-State-Action）算法由 McCallum 正式提出，与 Q-Learning 的区别在于它属于 on-policy 方法，学习的是实际执行策略的价值，而非理论最优策略的价值。
- **REINFORCE 算法**由 Ronald Williams 于 1992 年正式提出，它是基于策略梯度（Policy Gradient）的蒙特卡洛方法，直接优化策略参数而非通过价值函数间接优化，为后来 LLM 中的偏好优化奠定了理论基础。

REINFORCE 的核心是**策略梯度定理**：对于策略参数 $\theta$，梯度为

$$\nabla_\theta J(\theta) = \mathbb{E}_{\tau \sim \pi_\theta} \left[ \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(a_t | s_t) \cdot Q^{\pi_\theta}(s_t, a_t) \right]$$

在 LLM 语境中，$s_t$ 对应 prompt + 已生成的前缀 $x, y_{<t}$，$a_t$ 对应下一个 token $y_t$。这个公式告诉我们：如果某个 token 带来了高累计回报，就增加其概率；反之则降低。这与 DPO 中的对数概率比本质上是相通的——都是基于回报信号调整 token 概率。

这一阶段的 RL 主要应用于小型离散控制问题（如走迷宫、平衡杆），受限于计算能力和状态空间的爆炸性问题。

### 1.3 深度学习 × RL：突破临界点（2013–2016）

2013 年，DeepMind 发布了 **DQN（Deep Q-Network）**，将深度神经网络与 Q-Learning 结合，首次实现了从原始像素输入直接学习到 Atari 游戏的玩法规则（Mnih et al., 2013）。这是 RL 历史上第一个真正的里程碑。

关键创新包括：
- **深度神经网络作为函数近似器**，替代了传统的 Q 表
- **经验回放（Experience Replay）**：将历史经验存储并随机采样，打破数据间的时序相关性
- **目标网络（Target Network）**：固定目标 Q 值一段时间，提升训练稳定性

2016 年，DeepMind 在 *Nature* 上发表了 **AlphaGo**（Silver et al., 2016），通过结合蒙特卡洛树搜索（MCTS）与深度策略网络和价值网络，击败了围棋世界冠军李世石。AlphaGo 的成功证明了 RL 在超高复杂度博弈中的能力，也引发了全球范围内的 AI 热潮。

---

## 二、RL 与大语言模型：从 RLHF 到后 RLHF 时代

### 2.0 一句话类比：RL 对齐 LLM 到底在干什么？

如果用一个生活化的比喻来理解 RL 如何对齐 LLM：

- **预训练**就像让一个学生读了整座图书馆的书，知识渊博但说话不着边际。
- **SFT（监督微调）**就像让老师给他一本"标准答案手册"，教他"好的回答应该长什么样"。
- **RL 对齐**就像考试——学生每次写完回答后，老师给他打分（奖励），他根据分数调整自己的写法，最终学会在老师期望的方向上优化。

RLHF、DPO、GRPO 等一系列算法，本质上都是在回答同一个问题：**老师的"打分"应该怎么做，才能让学生学得好、学得快、学得稳？**

### 2.1 为什么 LLM 需要 RL？

大语言模型（如 GPT、LLaMA）基于**自回归的无监督预训练**，擅长模仿训练数据的统计规律，但存在几个关键问题：

1. **人类偏好对齐缺失**：模型可能生成有害、偏见或不准确的回答
2. **任务导向能力弱**：在需要多步推理或遵循特定指令的场景下表现不佳
3. **奖励信号缺失**：预训练只有语言建模损失，没有关于"这个回答好不好"的反馈

RL 的引入，本质上是为 LLM 提供**基于反馈的学习信号**，让模型学会在人类期望的方向上优化输出。

### 2.2 RLHF：人类反馈强化学习（2022）

2022 年，OpenAI 在论文 **"Training language models to follow instructions with human feedback"** 中正式提出了 **RLHF（Reinforcement Learning from Human Feedback）**，成为 LLM 对齐的标杆方法。

RLHF 分为三步：

| 步骤 | 内容 | 说明 |
|------|------|------|
| SFT | 监督微调 | 用人工标注的高质量对话数据微调 LLM |
| RM | 奖励模型训练 | 让人类对同一 prompt 的不同回答进行排序，训练一个能给出评分的奖励模型 |
| RL | 强化学习优化 | 以预训练 LLM 为初始策略，用 PPO 算法根据奖励模型的反馈进行优化 |

**核心公式：**

$$\max_{\pi} \mathbb{E}_{x \sim D, y \sim \pi(\cdot|x)} [r(x,y)] - \beta \cdot D _{KL}(\pi(\cdot|x) \| \pi _{ref}(\cdot|x))$$


其中 $r(x,y)$ 是奖励模型给出的分数，$\beta > 0$ 是超参数，控制对齐强度；$\beta \cdot D_{KL}$ 是 KL 散度惩罚项，防止优化后的策略 $\pi$ 偏离参考模型 $\pi_{ref}$ 太远。

**PPO 的完整计算流程：**

PPO（Proximal Policy Optimization，Schulman et al., 2017）是 RLHF 中实际使用的策略优化算法，其核心是**裁剪代理目标（clipped surrogate objective）**。具体步骤如下：

**步骤 1 — 采样**：对于每个 prompt $x$，从当前策略 $\pi_\theta$ 采样生成回答 $y$。自回归生成过程中，每一步的 token 序列为 $y = (y_1, y_2, \ldots, y_T)$，其概率为

$$\pi_\theta(y|x) = \prod_{t=1}^{T} \pi_\theta(y_t | x, y_{<t})$$

**步骤 2 — 计算奖励**：用训练好的奖励模型 $r_\phi(x, y)$ 给出生成 $y$ 的标量奖励 $r(x,y)$。同时计算 KL 散度惩罚项（单样本估计）：

$$d(x,y) = \frac{\pi_\theta(y|x)}{\pi_{ref}(y|x)} - \log \frac{\pi_\theta(y|x)}{\pi_{ref}(y|x)} - 1$$

这是 KL 散度 $D_{KL}(\pi_\theta(\cdot|x) \| \pi_{ref}(\cdot|x))$ 在单样本 $y$ 上的无偏估计（满足 $d(x,y) \geq 0$ 且当 $\pi_\theta = \pi_{ref}$ 时等于 0）。综合奖励为：

$$r^{\text{combined}}(x, y) = r(x, y) - \beta \cdot d(x,y)$$

**步骤 3 — 优势估计（GAE）**：用价值网络 $V_\psi$ 估计优势函数。采用广义优势估计（GAE, Generalized Advantage Estimation，Schulman et al., 2015）：

$$\delta _t = r _t + \gamma \cdot V_\psi(s _{t+1}) - V _\psi(s _t)$$

$$A^{\text{GAE}(\gamma, \lambda)} _t = \sum _{l=0}^{\infty} (\gamma \lambda)^l \cdot \delta _{t+l}$$

其中 $\gamma \in [0, 1]$ 是折扣因子，$\lambda \in [0, 1]$ 是 GAE 平滑参数。GAE 在偏差和方差之间做权衡：$\lambda = 0$ 退化为单步优势估计（低方差、高偏差），$\lambda = 1$ 为蒙特卡洛优势估计（高方差、低偏差）。

**步骤 4 — PPO 裁剪目标**：设 $r_t(\theta) = \frac{\pi_\theta(y_t | x, y_{<t})}{\pi_{\theta_{\text{old}}}(y_t | x, y_{<t})}$ 为新旧策略的概率比。PPO 的裁剪目标为：

$$L^{\text{CLIP}}(\theta) = \mathbb{E}_t \left[ \min\left( r_t(\theta) \cdot A_t, \; \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon) \cdot A_t \right) \right]$$

其中 $\epsilon$ 是裁剪超参数（通常取 0.2）。当优势 $A_t > 0$ 时，裁剪上限防止策略更新过快；当 $A_t < 0$ 时，裁剪下限防止策略过度远离旧策略。

**步骤 5 — 总损失**：

$$\mathcal{L}(\theta) = \mathbb{E}_t \left[ -L^{\text{CLIP}}(\theta) - c_1 \cdot \mathcal{L}_{\text{VF}}(\theta) + c_2 \cdot S[\pi_\theta] (x) \right]$$

其中 $\mathcal{L} _{\text{VF}} = \|r _t \cdot A _t - V _\psi(s _t)\|^2$ 是价值函数的 MSE 损失（ $c_1$ 为权重系数），$S[\pi _\theta] (x) = \mathbb{E} _{y \sim \pi _\theta(\cdot|x)} [-\log \pi _\theta(y|x)]$ 是策略的熵（ $c _2$ 为权重系数，鼓励探索）， $c _1, c _2$ 是超参数。注意最小化损失等价于最大化 PPO 目标（因此在 $L^{\text{CLIP}}$ 前加负号）。

**优点：** 能显著改善模型的对齐质量和指令遵循能力。

**痛点：**
- 训练流程极长（三步串行，每步都要单独训练和评估）
- PPO 算法在 LLM 上训练不稳定，需要精细调参（包括学习率、裁剪参数、价值网络超参数等）
- 奖励模型可能存在偏差（reward hacking / reward mis-specification）
- 需要额外维护价值网络（critic）和奖励模型（reward model），显存和计算开销大

### 2.3 从 RLHF 到 DPO：直接偏好优化（2023）

2023 年，Stanford 的 **DPO（Direct Preference Optimization）** 论文（Rafailov et al., 2023）提出了一个关键洞察：**不需要显式的奖励模型和 PPO 训练**。

DPO 的核心思想是将 RLHF 中的最优性条件反向求解出来，得到一个解析解形式的偏好优化目标。简单来说，给定一对回答 $(y_w, y_l)$（一个被偏好，一个被拒绝），DPO 直接最大化：

$$\mathcal{L} _{\text{DPO}}(\pi _\theta; \pi _{\text{ref}}) = -\mathbb{E} _{(x, y _w, y _l) \sim \mathcal{D}} \left[ \log \sigma \left( \beta \log \frac{\pi _\theta(y _w|x)}{\pi _{\text{ref}}(y _w|x)} - \beta \log \frac{\pi _\theta(y _l|x)}{\pi _{\text{ref}}(y _l|x)} \right) \right]$$

**DPO 公式的推导逻辑：**

DPO 的精妙之处在于从 RLHF 的优化目标反向求解。在 RLHF 的 KL 约束优化框架下，最优策略的解析解为：

$$\pi^*(y|x) = \frac{1}{Z(x)} \pi_{ref}(y|x) \exp\left(\frac{r^*(x,y)}{\beta}\right)$$

其中 $Z(x) = \sum_y \pi_{ref}(y|x) \exp\left(\frac{r^*(x,y)}{\beta}\right)$ 是归一化常数（配分函数）。反过来，最优奖励可以表示为：

$$r^*(x,y) = \beta \log \frac{\pi^*(y|x)}{\pi_{ref}(y|x)} + \beta \cdot \log Z(x)$$

注意到 $Z(x)$ 与 $y$ 无关，在偏好对比中会被消去。将 $r^*$ 代入 DPO 的偏好损失（即最大化偏好回答的奖励减去拒绝回答的奖励），就得到了上面的损失函数。

**DPO 的完整计算过程：**

1. **输入**：偏好对数据 $(x, y_w, y_l)$，其中 $y_w$ 是被偏好的回答，$y_l$ 是被拒绝的回答，数据集记为 $\mathcal{D}$
2. **计算对数概率比**：对每个偏好对，分别计算偏好回答和拒绝回答相对于参考模型的对数概率比：

$$\Delta(x, y_w, y_l) = \log \frac{\pi_\theta(y_w|x)}{\pi_{ref}(y_w|x)} - \log \frac{\pi_\theta(y_l|x)}{\pi_{ref}(y_l|x)}$$

其中自回归的对数概率为 $\log \pi_\theta(y|x) = \sum_{t=1}^{T} \log \pi_\theta(y_t | x, y_{<t})$，$T$ 为回答长度。
3. **Sigmoid 映射与损失**：将 $\beta \cdot \Delta$ 输入 sigmoid 函数，得到偏好概率：

$$\sigma(\beta \cdot \Delta) = \frac{1}{1 + \exp(-\beta \cdot \Delta)}$$

当 $\Delta > 0$ 时，说明偏好回答的对数概率比大于拒绝回答，$\sigma(\beta \cdot \Delta) > 0.5$，损失 $-\log \sigma(\beta \cdot \Delta)$ 较小；反之损失较大。
4. **梯度更新**：

$$\nabla_\theta \mathcal{L}_{\text{DPO}} = -\beta \cdot \left(1 - \sigma(\beta \cdot \Delta)\right) \cdot \nabla_\theta \Delta$$

当 $\Delta$ 很小（模型难以区分偏好和拒绝）时，$1 - \sigma \approx 0.5$，梯度较大，推动模型加大区分；当 $\Delta$ 很大（模型已能很好地区分）时，$1 - \sigma \approx 0$，梯度趋近于零，自动停止更新。

**意义：** DPO 将三步流程压缩为一步（直接用偏好数据微调），训练更简单、更稳定，且效果与 RLHF 相当甚至更好。

DPO 引发了后续一系列**偏好优化算法**的爆发，以下逐一介绍其公式细节：

#### IPO：Identity Preference Optimization

IPO（Wang et al., 2023）的核心洞察是：DPO 损失本质上是希望 $\Delta = \log \frac{\pi_\theta(y_w|x)}{\pi_{ref}(y_w|x)} - \log \frac{\pi_\theta(y_l|x)}{\pi_{ref}(y_l|x)}$ 为正，而 DPO 用 sigmoid 将其映射到 (0,1)。IPO 直接对这个差值做平方损失：

$$\mathcal{L}_{\text{IPO}}(\theta) = \mathbb{E} \left[ \left( \frac{1}{2} - \beta \cdot \Delta(x, y_w, y_l) \right)^2 \right]$$

IPO 对标签噪声（即偏好标注错误的情况）比 DPO 更鲁棒，因为平方损失对极端值的敏感度低于交叉熵。

#### KTO：Kahneman-Tversky Optimization

KTO（Ethayarajh et al., 2024）基于前景理论（Prospect Theory），为每个回答分配一个"期望效用"而非依赖成对比较。给定一个目标偏好分数 $r_{\text{target}}$（通常取 0.5），KTO 的损失为：

$$\mathcal{L} _{\text{KTO}}(\theta) = \mathbb{E} _{(x,y,w) \sim \mathcal{D}} \left[ \begin{array}{l} w \cdot \max\left(0, -\log \sigma(\beta \cdot \log \frac{\pi _\theta(y|x)}{\pi _{ref}(y|x)} - r _{\text{target}})\right) \\ + (1-w) \cdot \max\left(0, -\log \sigma(\beta \cdot \log \frac{\pi _{ref}(y|x)}{\pi _\theta(y|x)} + r _{\text{target}})\right) \end{array} \right]$$

其中 $w \in \{0, 1\}$ 表示该回答是否被偏好。KTO 不需要成对的偏好数据，每个样本独立优化。

#### ORPO：Odds-Ratio Preference Optimization

ORPO 将 SFT 的交叉熵损失和 DPO 的偏好损失合并为单步：

$$\mathcal{L} _{\text{ORPO}}(\theta) = -\mathbb{E} \left[ \log \sigma\left( \beta \cdot \log \frac{\pi _\theta(y _w|x)}{\pi _\theta(y _l|x)} \cdot \frac{\pi _{ref}(y _l|x)}{\pi _{ref}(y _w|x)} \right) \right] - \lambda \cdot \mathcal{L} _{\text{SFT}}(\theta)$$

其中第二项是标准 SFT 损失（最大化偏好回答的似然，即负对数似然）。ORPO 将偏好比值 $\frac{\pi_\theta(y_w|x)}{\pi_\theta(y_l|x)}$ 与参考模型的比值 $\frac{\pi_{ref}(y_l|x)}{\pi_{ref}(y_w|x)}$ 做对比，本质上是在 SFT 的同时做偏好优化。

#### SimPO：Simple Preference Optimization

SimPO（Tian et al., 2024）的洞察是：不需要 KL 散度到参考模型，直接用序列的 log-prob 差作为奖励信号：

$$\mathcal{L}_{\text{SimPO}}(\theta) = -\mathbb{E} \left[ \log \sigma\left( \frac{\beta}{|y_w|} \log \pi_\theta(y_w|x) - \frac{\beta}{|y_l|} \log \pi_\theta(y_l|x) - \gamma \right) \right]$$

其中 $|y_w|$、$|y_l|$ 分别是回答 $y_w$、$y_l$ 的 token 数（即序列长度），$\gamma$ 是目标 margin 超参数。SimPO 通过长度归一化消除了长度偏好问题，且不需要参考模型 $\pi_{ref}$。

#### CPO：Contrastive Preference Optimization

CPO 关注正负样本之间的相对距离，使用对比式损失（InfoNCE 风格）。设对于每个 prompt $x$，有一个偏好回答 $y_w$ 和 $K$ 个拒绝回答 $y_1, \ldots, y_K$。定义每个回答的"偏好分数"为 $s(y) = \beta \cdot \log \frac{\pi_\theta(y|x)}{\pi_{ref}(y|x)}$，则 CPO 损失为：

$$\mathcal{L}_{\text{CPO}}(\theta) = -\mathbb{E} \left[ \log \frac{\exp(s(y_w))}{\sum_{j=0}^{K} \exp(s(y_j))} \right]$$

其中 $y_0 = y_w$，$y_1, \ldots, y_K$ 为拒绝回答。CPO 将偏好优化视为一个 $K+1$ 类的对比分类问题（InfoNCE 风格）。

这些算法的共同特点是**去掉了 PPO 训练环节**，将 RL 问题转化为一个监督学习问题。

### 2.4 GRPO：组相对策略优化（2024）

2024 年 11 月，来自 DeepSeek 的 **GRPO（Group Relative Policy Optimization）** 论文（DeepSeek-AI et al., 2024）引起了广泛关注。GRPO 的核心创新在于**用组内相对排名替代了价值网络（critic）**。

**GRPO 的关键设计：**

1. **去掉 Critic 模型**：传统 PPO 需要单独训练一个价值函数模型来估计状态价值。GRPO 通过让 LLM 为同一个 prompt 生成多个回答（一个 group），然后用组内的相对奖励（group reward）来计算优势函数（advantage）。

2. **组内归一化**：对于一个 prompt 生成的 $G$ 个回答，先计算每个回答的奖励 $r_1, r_2, \ldots, r_G$，然后做组内归一化：

$$\hat{A} _i = \frac{r _i - \text{mean}(\{r _j\} _{j=1}^G)}{\text{std}(\{r _j\} _{j=1}^G)}$$

3. **简化训练流程**：不需要额外的奖励模型和价值模型，只需要一个基础模型就能完成整个 RL 训练。

**GRPO 的完整计算过程：**

设对于 $N$ 个 prompt $\{x_1, x_2, \ldots, x_N\}$，当前策略 $\pi_\theta$ 为每个 prompt 生成 $G$ 个回答。

**步骤 1 — 组内采样**：对每个 prompt $x_i$，从 $\pi_\theta$ 采样 $G$ 个回答：

$$\{y_{i,1}, y_{i,2}, \ldots, y_{i,G}\} \sim \pi_\theta(\cdot|x_i)$$

**步骤 2 — 计算每个回答的奖励**：
- 对于可验证任务（数学、代码）：使用精确奖励，正确答案为 1，错误为 0：

$$r_{i,j} = \mathbb{I}(\text{verify}(y_{i,j}) = \text{true})$$

- 对于不可验证任务：使用奖励模型 $r_\phi(x_i, y_{i,j})$ 给出标量评分。

**步骤 3 — 组内优势估计**：计算组内奖励的均值和标准差，然后归一化得到优势：

$$\hat{A} _{i,j} = \frac{r _{i,j} - \mu _i}{\sigma _i + \epsilon}$$

其中 $\mu_i = \frac{1}{G}\sum_{k=1}^{G} r_{i,k}$，$\sigma_i = \sqrt{\frac{1}{G}\sum_{k=1}^{G}(r_{i,k} - \mu_i)^2 + \epsilon}$，$\epsilon$ 为数值稳定性项（通常取 $10^{-8}$）。

**步骤 4 — GRPO 裁剪目标**：与 PPO 类似，使用裁剪策略目标，但优势函数用组内归一化的 $\hat{A}_{i,j}$ 替代：

$$L^{\text{GRPO}}(\theta) = \frac{1}{NG} \sum _{i=1}^{N} \sum _{j=1}^{G} \min\left( \frac{\pi _\theta(y _{i,j}|x _i)}{\pi _{\theta _{\text{old}}}(y _{i,j}|x _i)} \cdot \hat{A} _{i,j}, \; \text{clip}\left(\frac{\pi _\theta(y _{i,j}|x _i)}{\pi _{\theta _{\text{old}}}(y _{i,j}|x _i)}, 1-\epsilon, 1+\epsilon\right) \cdot \hat{A} _{i,j} \right)$$

**步骤 5 — KL 惩罚项**：为防止策略偏离参考模型太远，加入 KL 惩罚：

$$\mathcal{L} _{\text{GRPO}}(\theta) = L^{\text{GRPO}}(\theta) + \lambda \cdot \frac{1}{NG} \sum _{i=1}^{N} \sum _{j=1}^{G} D _{KL}(\pi _\theta(\cdot|x _i) \| \pi _{ref}(\cdot|x _i))$$

**步骤 6 — 梯度更新**：对总损失进行反向传播更新策略参数 $\theta$。

**与 PPO 的关键差异：**

| 对比项 | PPO (RLHF) | GRPO |
|--------|-----------|------|
| 优势估计 | 需要价值网络 $V_\psi$ 用 GAE 计算 | 组内奖励归一化，无需价值网络 |
| 奖励模型 | 需要单独训练的 $r_\phi$ | 可省略（可验证任务用精确奖励）或复用基础模型 |
| Critic 模型 | 需要，额外参数开销 | 不需要 |
| 采样策略 | 每个 prompt 采样 1 个回答 | 每个 prompt 采样 $G$ 个回答（$G$ 通常为 4–8）|

**GRPO 的优势：**
- **训练效率更高**：省去了奖励模型训练和 PPO 中 critic 的更新
- **显存占用更低**：不需要存储 critic 模型的参数
- **实现更简洁**：整个流程可以集成在一个训练循环中
- **组内相对比较更鲁棒**：组内归一化天然消除了不同 prompt 之间奖励尺度的差异

### 2.5 BRPO：引导式相对策略优化（2025）

2025 年 6 月，**Writing-Zero** 论文（Jia et al., arXiv: 2506.00103）提出了 **BRPO（Bootstrapped Relative Policy Optimization）**，专门解决 RLVR 在**非可验证任务**（如创意写作、开放对话）中的困境。

**背景问题：** RLVR 在数学推理、代码生成等可验证任务上取得了巨大成功，但在创意写作等主观评价任务上，传统的标量奖励模型（scalar reward model）存在明显缺陷：泛化能力差、容易 reward hacking（过度解释、长度偏好等）。

**BRPO 的核心设计：**

1. **生成式奖励模型（GenRM）：** 不使用标量评分，而是训练一个 pair-wise 的生成式奖励模型，基于写作原则进行自我批判式评估（self-principled critique），将主观判断转化为可验证的奖励信号。

2. **Bootstrapped 引导机制：** BRPO 在训练过程中，从同组 rollout 中动态采样一个引导响应（bootstrapped response）作为临时参考，实现**动态、无参考的成对比较（dynamic reference-free pairwise comparison）**。这与 GRPO 的组内归一化思想一脉相承，但引入了 bootstrapped 参考的概念。

**BRPO 的关键创新：**
- **无需监督微调（SFT-free）：** 直接在预训练模型上训练写作能力
- **强抗 reward hacking：** 通过 pair-wise 比较而非标量评分，避免模型迎合单一奖励维度
- **统一 RLVR 范式：** 尝试将基于规则、基于参考和基于无参考的奖励建模统一到 RLVR 框架下

**BRPO 的完整计算过程：**

设对于 $N$ 个 prompt $\{x_1, x_2, \ldots, x_N\}$，当前策略 $\pi_\theta$ 为每个 prompt 生成 $G$ 个回答 $\{y_1, y_2, \ldots, y_G\}$。

**步骤 1 — 组内采样**：同 GRPO，对每个 prompt $x_i$ 采样 $G$ 个回答。

**步骤 2 — 生成式奖励（GenRM）评估**：训练一个 pair-wise 的生成式奖励模型 $M_{\text{GenRM}}$，对每对回答 $(y_a, y_b)$ 进行自我批判式评估。模型基于写作原则（如结构、连贯性、创意性）输出比较结果而非标量评分：

$$\text{critique} _{a \to b} = M _{\text{GenRM}}(x, y _a, y _b)$$

该 critique 是一个自然语言文本，解释为什么 $y_a$ 优于 $y_b$（或反之）。这种 pair-wise 比较避免了标量评分的单一维度问题。

**步骤 3 — Bootstrapped 引导采样**：从同组 rollout 中动态采样一个引导响应 $y_{\text{boot}}$ 作为临时参考。具体而言，从组内 $\{y_1, \ldots, y_G\}$ 中按概率分布 $p_j \propto \exp(\text{critique}(y_j))$ 采样：

$$y_{\text{boot}} \sim p_{\text{boot}} = \text{Softmax}\left(\frac{\text{critique}(y_1), \ldots, \text{critique}(y_G)}{\tau}\right)$$

其中 $\tau$ 是温度参数，控制采样的"锐度"。

**步骤 4 — 动态参考比较**：将当前回答 $y_j$ 与 bootstrapped 参考 $y_{\text{boot}}$ 进行成对比较，得到成对优势：

$$\hat{A} _{i,j} = \text{critique} _{y _{i,j} \to y _{i,\text{boot}}} - \text{critique} _{y _{i,\text{boot}} \to y _{i,j}}$$

这种对称的成对比较消除了单个 critic 的主观偏差。

**步骤 5 — BRPO 策略更新**：与 GRPO 类似，使用裁剪策略目标，但优势函数用 bootstrapped 成对比较的 $\hat{A}_{i,j}$ 替代：

$$\mathcal{L} _{\text{BRPO}}(\theta) = L^{\text{clip}}(\theta; \hat{A}) + \lambda \cdot D _{KL}(\pi _{ref} \| \pi _\theta)$$

**与 GRPO 的核心差异：**

| 对比项 | GRPO | BRPO |
|--------|------|------|
| 奖励形式 | 标量奖励 $r \in \mathbb{R}$ | 生成式 critique 文本 |
| 优势估计 | 组内归一化 $\frac{r_i - \mu}{\sigma}$ | bootstrapped 成对比较 |
| 参考模型 | 不需要（除 KL 惩罚项） | 动态 bootstrapped 采样作为参考 |
| 适用任务 | 可验证任务（数学、代码） | 非可验证任务（写作、对话） |
| SFT 依赖 | 通常需要 SFT 初始化 | SFT-free，可直接从预训练模型开始 |

**BRPO 与 GRPO 的关系：**
BRPO 和 GRPO 都属于「去 critic」的轻量级偏好优化路线，但侧重点不同：GRPO 侧重于推理类可验证任务中的高效训练，BRPO 侧重于主观任务中如何用生成式奖励和 bootstrapped 参考来克服奖励模型的局限性。两者在技术思路上有相通之处（组内比较、动态参考），可以看作是同一方向上的不同分支。

### 2.6 GRPO 的变体与延伸（2024–2025）

GRPO 提出后，迅速催生了多个改进版本。

| 算法 | 全称 | 改进点 |
|------|------|--------|
| **GRPO** | Group Relative Policy Optimization | 组内相对优势，去 critic |
| **Dr. GRPO** | Diverse Reward GRPO | 引入多样性奖励，鼓励生成多样化的高质量回答 |
| **RLVR** | Reinforcement Learning with Verifiable Rewards | 针对数学推理，使用可验证的正确答案作为奖励信号 |
| **RLOO** | Reinforcement Learning with Leave-One-Out | 改进 GRPO 的优势估计，用 leave-one-out 方式计算 |
| **BRPO** | Bootstrapped Relative Policy Optimization | 引导式相对策略优化，用于非可验证任务（Writing-Zero, 2025） |
| **PPO-Ref** | PPO with Reference Model | 保留 PPO 框架但优化了参考模型的策略 |
| **DPO++** | DPO Enhanced | 在 DPO 基础上引入更多负样本和对比机制 |
| **REINFORCE++** | — | 将 REINFORCE 算法引入 LLM 对齐，探索梯度估计的改进 |
| **SLiC / SLiF** | Sequence Likelihood Calibration / Finetuning | 通过校准序列似然来对齐偏好 |

**RLOO 的公式细节：**

RLOO（Zhao et al., 2024）的核心改进是替代 GRPO 的组内归一化优势估计。在 GRPO 中，$\hat{A} _{i,j} = \frac{r _{i,j} - \mu _i}{\sigma _i}$ 中每个样本 $j$ 都参与了均值和方差的计算，因此优势估计中混入了自身信息。RLOO 采用 leave-one-out 方式，将样本 $j$ 自身排除在统计量计算之外：

$$\hat{A}^{\text{RLOO}} _{i,j} = \frac{r _{i,j} - \frac{1}{G-1}\sum _{k \neq j} r _{i,k}}{\sqrt{\frac{1}{G-1}\sum _{k \neq j}(r _{i,k} - \bar{r} _{i}^{\neg j})^2 + \epsilon}}$$

其中 $\bar{r} _{i}^{\neg j} = \frac{1}{G-1}\sum _{k \neq j} r _{i,k}$ 是排除样本 $j$ 后的组内均值。RLOO 的优势估计更"无偏"，因为每个样本的优势只依赖其他样本的信息。

**Dr. GRPO 的核心思想：**

Dr. GRPO 在 GRPO 的奖励中加入多样性项。除了原始奖励 $r_{i,j}$，额外计算组内回答的多样性惩罚/奖励：

$$r^{\text{Dr}} _{i,j} = r _{i,j} + \lambda _{\text{div}} \cdot \frac{1}{G-1} \sum _{k \neq j} \text{diversity}(y _{i,j}, y _{i,k})$$

其中 $\text{diversity}(y_a, y_b)$ 可以基于 NLSD（Normalized Longest Common Substring Distance）等文本相似度指标计算。这鼓励模型在同一 prompt 下探索多样化的推理路径，避免多回答退化到同一个模式。

此外还有一些值得关注的方向：**BPO（Budget Preference Optimization）**，在偏好优化中引入计算预算约束，在推理质量和效率之间做权衡；以及 **RLAIF（Reinforcement Learning from AI Feedback）**，用 AI 生成的反馈替代人类标注，大幅降低对齐成本。

### 2.7 当前前沿：RL for 大模型科研

2024 年底到 2025 年，RL 在 LLM 中的应用进入了一个新阶段。几个关键趋势：

**（1）RL for Science（科学发现）**

研究人员正在用 RL 驱动 LLM 在科学发现中扮演主动角色——不是让 LLM 回答问题，而是让 LLM 作为**科研代理**，通过"提出假设 → 设计实验 → 分析结果 → 修正假设"的闭环，自主推进科学研究。

**（2）推理能力的 RL 增强**

DeepSeek-R1 和 QwQ 等模型的突破性成果，很大程度上归功于 RL 对推理能力的增强。通过 GRPO 等算法，让模型在数学推理、代码生成等任务上学会"思考过程"的优化，而不仅仅是"答案的正确性"。

**（3）多智能体 RL**

将 LLM 作为多智能体系统中的一个 agent，通过 RL 让多个模型之间协作、竞争、协商，完成更复杂的任务。这在自动化测试、软件工程和仿真系统中展现出巨大潜力。

**（4）RL 与 RAG 的结合**

检索增强生成（RAG）与 RL 的结合——用 RL 优化检索策略、重排算法和最终的答案生成，使系统在信息检索和生成之间形成端到端的优化闭环。

---

## 三、技术对比：主流偏好优化算法一览

偏好优化算法的设计在多个维度上存在权衡。下表从算法所需的**外部模型**、**训练范式**、**数据需求**和**适用场景**四个维度进行对比，帮助读者理解不同算法的定位和取舍。

| 算法 | 需训练奖励模型？ | 需训练价值模型？ | 优化范式 | 训练步骤 | 数据需求 | 适用场景 |
|------|:---:|:---:|:---:|:---:|:---:|:---:|
| **RLHF (PPO)** | ✅ 需要 | ✅ 需要 | 策略梯度 | 3 步（SFT→RM→RL） | 偏好排序（多选项排序） | 通用对话、指令遵循，需要精细对齐的场景 |
| **DPO** | ❌ | ❌ | 直接偏好优化 | 1 步（SFT 后微调） | 偏好对（$y_w, y_l$） | 通用偏好对齐，标注数据充足 |
| **IPO** | ❌ | ❌ | 平方损失偏好优化 | 1 步（SFT 后微调） | 偏好对 | 偏好对含标签噪声的场景 |
| **KTO** | ❌ | ❌ | 直接偏好优化 | 1 步（SFT 后微调） | 独立样本 + 效用标签（无需成对） | 无偏好对标注、仅有独立评分的场景 |
| **ORPO** | ❌ | ❌ | SFT + 偏好联合优化 | 1 步（端到端） | 偏好对 | 需要兼顾生成质量与偏好对齐 |
| **SimPO** | ❌ | ❌ | log-prob 差直接优化 | 1 步（SFT 后微调） | 偏好对 | 需消除长度偏好的场景 |
| **CPO** | ❌ | ❌ | 对比式偏好优化 | 1 步（SFT 后微调） | 偏好对 + 多个负样本 | 多负样本对比场景 |
| **GRPO** | ❌ | ❌ | PPO 裁剪策略（去 critic） | 1 步（可直接预训练后训练） | 无需偏好标注，仅需可验证奖励 | 可验证任务（数学推理、代码生成） |
| **RLOO** | ❌ | ❌ | PPO 裁剪策略（去 critic） | 1 步 | 无需偏好标注，仅需可验证奖励 | 需无偏优势估计的可验证任务 |
| **BRPO** | ❌ | ❌ | PPO 裁剪策略（去 critic） | 1 步（SFT-free） | 无需偏好标注，仅需 GenRM 成对比较 | 非可验证任务（写作、对话） |

> **维度说明：**
> - **需训练奖励模型**：算法是否在训练过程中需要单独训练一个奖励模型（Reward Model）
> - **需训练价值模型**：算法是否需要单独训练一个价值网络（Critic / Value Network）
> - **优化范式**：算法采用的优化策略类型
> - **训练步骤**：从预训练/SFT 到最终模型需要经历的训练阶段数
> - **数据需求**：算法训练所需的标注数据类型（偏好标注 vs 可验证奖励 vs 独立评分）
> - **适用场景**：算法最适合的任务类型
>
> 总体而言，**RLHF (PPO)** 在通用对齐任务上效果最成熟但复杂度最高；**DPO 系列**（DPO / IPO / KTO / ORPO / SimPO / CPO）通过简化训练流程降低了门槛，但依赖偏好标注数据；**RL 衍生系列**（GRPO / RLOO / BRPO）则完全免除了偏好标注需求，通过组内比较或生成式奖励实现对齐，但各有特定的适用场景。

---

## 四、总结与展望

强化学习从 AlphaGo 到 GRPO 的演进，本质上反映了 AI 研究的一个趋势：**从"用复杂系统解决复杂问题"走向"用更简洁的机制实现相同甚至更好的效果"**。

RLHF 用三步流程证明了 LLM 对齐的可行性，DPO 用一步优化证明了它更简洁，而 GRPO 进一步去掉了价值模型，让 RL 训练真正变得轻量化和高效化。

展望未来，随着 RL 技术的持续演进，我们可能会看到：

- **RL 成为 LLM 训练的标配环节**，如同预训练和 SFT 一样自然
- **RL 与 Agent 框架深度融合**，让大模型具备自主决策和行动能力
- **RL for Science 的范式创新**，推动科学发现的自动化和加速化
- **多模态 RL**，在视觉、语音、文本等多模态场景下实现对齐和优化

从 AlphaGo 的棋盘到 LLM 的文本，从离散的动作空间到连续的语义空间，强化学习正在重新定义 AI 的能力边界。而这一切，才刚刚开始。

---

## 参考文献

1. Mnih, V. et al. "Human-level control through deep reinforcement learning." *Nature* **518**, 529–533 (2015).
2. Silver, D. et al. "Mastering the game of Go with deep neural networks and tree search." *Nature* **529**, 484–489 (2016).
3. Ouyang, L. et al. "Training language models to follow instructions with human feedback." *NeurIPS* 2022.
4. Rafailov, R. et al. "Direct preference optimization: Your language model is secretly a reward model." *NeurIPS* 2023.
5. DeepSeek-AI et al. "DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via RL." 2024.（GRPO 论文）
6. Jia, R. et al. "Writing-Zero: Bridge the Gap Between Non-verifiable Tasks and Verifiable Rewards." *arXiv:2506.00103*, 2025.（BRPO 论文）
7. Schulman, J. et al. "Proximal Policy Optimization Algorithms." *arXiv:1707.06347*, 2017.
8. Schulman, J. et al. "High-Dimensional Continuous Control Using Generalized Advantage Estimation." *arXiv:1506.02438*, 2015.
9. Wang, T. et al. "IPO: Exit the Saddle — An Exact Solution to the Logistic Regression Preference Learning Problem." *NeurIPS* 2023.
10. Ethayarajh, K. et al. "KTO: Model Alignment as Prospect Theoretic Optimization." *arXiv:2402.01306*, 2024.
11. Tian, Y. et al. "SimPO: Simple Preference Optimization with a Reference-Free Reward." *NeurIPS* 2024.
12. Zhao, H. et al. "RLOO: Reinforcement Learning with Leave-One-Out Advantage Estimation." *arXiv:2412.18899*, 2024.
13. Williams, R. J. "Simple statistical gradient-following algorithms for connectionist reinforcement learning." *Machine Learning* **8**(3), 229–256 (1992).
14. Watkins, C. J. C. H. & Barto, A. G. "Q-learning." *Machine Learning* **8**(3–4), 279–292 (1992).

---

*本文基于公开文献整理，内容截至 2026 年 6 月。*
