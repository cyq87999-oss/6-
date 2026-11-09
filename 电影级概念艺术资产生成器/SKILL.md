---
name: 电影级概念艺术资产生成器
description: >-
  电影级概念艺术资产生成器 · 短剧/漫剧视觉资产生产 Skill v4.0。把一句话故事/世界观转化为一套可复用的视觉生产资产：
  STORY VISUAL BRIEF → WORLD CARD → VISUAL DNA → ASSET MATRIX → KEYFRAME → PROMPT 编译 → CONTINUITY LOCK → QA。
  解决多角色/多场景/多镜头"各自好看却不像同一部作品"的连续性漂移问题。支持最小一句输入自动补齐、A/B/C 三模式评测、
  L1-L5 变更路由、动态负向系统、资产依赖图与版本管理、HARD/SOFT/OPEN 三级锁定、参考图继承、失败恢复、标准输出 Schema v3.0。
  当用户说「概念图/角色设定/场景圣经/主视觉/定妆图/关键帧/视觉DNA/资产ID/World ID/连续性锁/短剧漫剧视觉资产/
  参考图继承/视觉资产胶囊/一句话出一套视觉系统」时调用。
---

# 电影级概念艺术资产生成器 · 短剧 / 漫剧视觉资产生产 Skill v4.0

## 0. 定位

本 Skill 的核心不是"生成一张漂亮概念图"，而是解决短剧 / 漫剧生产中的一个具体问题：

> 故事已经成立，但人物、场景、道具、镜头各自好看，放到一起却不像同一部作品。

它把"一句话故事 / 世界观"转化为一套可以持续使用的视觉生产资产：

```
故事输入 → 视觉DNA → 资产矩阵 → 关键帧 → Prompt编译 → 连续性锁 → QA → 多轮增量迭代
```

核心判断标准：每个资产都必须回答"它如何服务这个故事"，而不是只回答"它是否好看"。

## 1. 目标赛道与应用场景

默认垂直于：短剧 / 漫剧的前期视觉策划与连续视觉资产生产。

适用场景：短剧人物设定与角色定稿、漫剧主视觉与角色立绘、短剧场景圣经、关键道具设定、连续镜头关键帧、AI 视频前的首帧 / 尾帧设计、多集内容的角色与空间一致性管理。

不把"电影、动画、游戏全部覆盖"作为主要卖点；这些可以作为可迁移能力，而不是参赛主定位。

## 2. A / B / C 评测设计

**A｜不用 Skill 直接 Prompt** — 允许用户直接生成单张角色图、场景图或概念图。

**B｜仅使用 Skill 全自动** — 给出一句话故事 / 世界观后，不得停在"请选择方向"，必须自动完成：故事视觉任务判断 → WORLD CARD → VISUAL DNA → 默认资产矩阵 → 至少 1 角色 + 1 场景 + 1 道具 + 3 关键帧 → 图像 Prompt + 视频 Prompt + Negative Prompt → Continuity Lock → QA 评分 → 下一步可扩展资产建议。

B 项强制增量规则（相较普通 Prompt 至少新增三类可验证资产）：
- 世界级绑定：Visual DNA / WORLD ID
- 生产级绑定：Character / Location / Prop / Shot ID
- 连续性绑定：Continuity Lock + QA
如果只生成一张漂亮图片而没有资产系统，视为 B 失败。

**C｜三轮对话** — 必须维护同一 WORLD ID，并根据用户修改判断影响层级，只重算受影响模块。禁止每一轮从头生成完整世界而导致人物、服装、空间和色彩漂移。

## 3. 输入协议

### 3.1 最小输入
支持只有一句自然语言，如"一个记忆被删除的东方末世女侦探。"最低输入只需要包含一个可识别的创作核心；不强制用户填写人物、时代、场景、风格、画幅或模型。

### 3.2 自动补齐原则
信息缺失时，Skill 必须主动补齐，不得因为低影响缺项而停止、反复追问或要求用户选择。
自动补齐顺序：内容核心 → 故事功能 → 世界设定 → 主角设定 → 核心场景 → 核心道具 → Visual DNA → 关键帧 → Prompt 参数。补齐完成后直接进入生产，不要求二次确认。

### 3.3 自动补齐规则
- **A｜内容类型**：偏人物→角色资产生产；偏空间→场景资产生产；偏事件→关键帧生产；人物 + 事件→短剧 / 漫剧视觉资产生产；未明确时默认短剧 / 漫剧视觉资产生产。
- **B｜角色**：自动补齐年龄段、外貌、体态、服装、职业 / 身份、核心道具、情绪基调；所有补齐必须服务故事核心，禁止随机堆砌"机甲 / 霓虹 / 皮夹克"等泛化视觉词。
- **C｜世界**：自动补齐时间、地域、社会环境、建筑环境、天气 / 空气、世界规则与人物相关视觉背景。
- **D｜场景与道具**：默认 1 个核心场景 + 1 个核心道具。道具必须能影响人物选择、信息或剧情。
- **E｜视觉**：未指定时默认电影级写实概念艺术、低饱和、明确冷暖关系、真实材质、统一摄影语言、剧情服务型光影；不得机械套用固定"黑金 / 霓虹 / 赛博朋克 / 体积雾"。
- **F｜画幅**：未指定默认 16:9；识别出短视频、抖音、手机竖屏、漫剧竖版时自动切换 9:16。
- **G｜模型**：未指定使用 Generic Prompt；指定 Seedance / Anycool 等时仅改变表达语法，不改变 WORLD、Visual DNA、资产锚点与连续性规则。

### 3.4 自动补齐优先级
用户明确输入 > 本轮明确修改 > 已锁定资产 > 自动推断默认值。先保证故事功能，再保证视觉合理，最后补齐风格参数。任何自动补齐不得覆盖用户明确提供的信息。

### 3.5 自动补齐锁定（内部建立）
```
auto_fill:
  input_core:
  content_type:
  story_function:
  protagonist:
  world:
  location:
  prop:
  visual_style:
  aspect_ratio:
  target_model:
  confidence:
```
并生成 WORLD / CHARACTER / LOCATION / PROP / SHOT ID。

### 3.6 最小输入的"首轮交付"要求（B 测试）
对于只有一句输入的 B 测试，必须一次性完成：一句话创作判断、WORLD CARD、Visual DNA、1 主角、1 核心场景、1 核心道具、3 个连续关键帧、图像 Prompt、视频 Prompt、动态 Negative Prompt、Continuity Lock、QA 分数。不得输出"请补充设定""请选择风格""请选择模型"作为首轮阻塞。

### 3.7 明确冲突信息处理规则
当用户输入出现冲突，先执行：识别 → 分层 → 优先级判断 → 兼容尝试 → 局部更新 / 整体回退。
优先级默认：本轮用户明确修改 > 用户早期明确设定 > 已锁定资产 > 自动推断。同层冲突以最新明确修改为准；不同层冲突优先保证：故事逻辑 > 人物逻辑 > 世界逻辑 > 视觉表现 > 风格偏好。可兼容时优先用世界观解释同时成立；无法兼容时不随机拼接，采用高优先级信息。

### 3.8 冲突处理输出模板
检测到冲突时使用简明格式：
```
【冲突提示】
原设定：{原信息}
新设定：{新信息}

【处理结果】
已采用：{生效信息}
处理原则：{本轮修改/已锁定继承/故事逻辑优先/视觉逻辑优先}

【影响范围】
已更新：{模块/ID}
保持锁定：{模块/ID}

【当前生效设定】
{1—3句话}
```
轻微冲突不阻塞；仅当冲突会导致核心人物身份、世界规则或故事目标无法确定时，才允许提出一个最小澄清问题。

### 3.9 连续对话变更语义
默认识别："增加"＝补充；"调整"＝局部修改；"改成"＝覆盖；"不要 / 去掉"＝删除；"保留其他不变"＝仅修改指定模块。如果用户未明确要求重置，不得重置其他已锁定设定。

## 4. 创作流程

- **Stage 01｜STORY VISUAL BRIEF**：提取主角目标、核心冲突、情绪基调、世界规则、视觉隐喻、关键视觉事件。
- **Stage 02｜WORLD BUILD**：生成 WORLD ID，确定时间 / 地域、空间逻辑、美术生产规则、视觉禁区。
- **Stage 03｜VISUAL DNA**：建立 Color / Light / Material / Camera / Production Design / Motion DNA（详见 references/01）。
- **Stage 04｜ASSET MATRIX**：自动决定最少可交付资产 Character / Location / Prop / Shot Keyframe。
- **Stage 05｜ASSET DEPTH**：按故事功能分级 Anchor（必须锁定）/ Support（服务剧情）/ Decorative（只在必要时生成）；禁止无故事功能的资产堆砌。
- **Stage 06｜KEYFRAME STORYBOARD**：至少 3 个关键帧（进入场 / 信息·动作变化 / 下一镜头钩子）。
- **Stage 07｜PROMPT COMPILER**：输出图像、视频、负向 Prompt，不同模型只改变执行格式，不改变视觉核心。
- **Stage 08｜CONTINUITY QA**：检查人物、服装、道具、空间、光线、时间、动作、视线、镜头轴线。
- **Stage 09｜DELIVERY**：优先交付可直接使用的结果，再给简要方法说明；避免把内部工程过程全部倾倒给用户。

## 15. 输出层级与信息密度
默认采用三层输出：
- **L1｜直接可用结果**：用户最先看到角色 / 场景 / 道具 / 关键帧与可复制 Prompt。
- **L2｜工程锁定**：展示 WORLD ID、资产 ID、Visual DNA 摘要、Continuity Lock。
- **L3｜验证摘要**：展示 QA、变更范围、需要重生成的资产。
内部推理过程不得全部倾倒给用户。

## 19. 自动扩展能力
当用户只要求"做一个角色"，系统默认先提供最小可用资产胶囊，并告诉用户可以继续扩展：同角色三视图、表情包、日常 / 任务双形态、核心场景、关键道具、三镜关键帧。但不强行生成无关资产。

## 20. 参赛模式输出
用户说"参赛展示"时，按：输入 → AI 自动解析 → Visual DNA → 资产矩阵 → 关键帧 → Prompt 编译 → 连续性前后对比 → QA → 规模化扩展。展示重点：同一 Skill 如何从一句话稳定地产生一套可用于短剧 / 漫剧继续生产的视觉资产。

## 22. 最终交付原则
先交付结果，再交付工程摘要。最小可交付不是"一个 Prompt"，而是一套有世界观、资产 ID、视觉 DNA、关键帧和连续性约束的可复用视觉资产胶囊。

## 原创性说明
- **核心原创**：STORY VISUAL BRIEF（先判断视觉如何服务故事，再生成视觉资产）；WORLD → VISUAL DNA → ASSET MATRIX → KEYFRAME → PROMPT → QA 的一体化生产链；Asset ID 与 Visual DNA 继承关系（用于多角色、多场景、多镜头持续生产）；C 项 L1–L5 变更路由（根据用户修改粒度决定局部重算还是世界层重构）；关键帧必须具有"建立→变化→钩子"叙事功能，避免概念图拼贴成 PPT；B 项强制增量规则（自动产出必须形成可验证的资产系统，而非单张漂亮图）。
- **原创边界**：不复制具体影视 IP、具体角色独有设计或整段现成 Prompt；可使用通用摄影、材质、构图和公共文化元素，并重新组织其因果关系与生产逻辑。

## 标准输出 Schema v3.0
```
01 STORY VISUAL BRIEF   主角目标 / 核心冲突 / 情绪基调 / 视觉隐喻 / 关键视觉事件
02 WORLD CARD           WORLD ID / 世界观一句话 / 时间·地域 / 世界规则 / 美术生产规则 / 视觉禁区
03 VISUAL DNA           Color / Light / Material / Camera / Production Design / Motion DNA
04 ASSET MATRIX         Anchor/Support/Decorative + Character/Location/Prop/Shot IDs
05 KEYFRAME STORYBOARD  Shot Size / Lens / Camera Height / Movement / Composition /
                        Action Start-End / Information Change / Hook to Next Shot / Continuity
06 PROMPTS              IMAGE PROMPT / VIDEO PROMPT / NEGATIVE PROMPT
07 CONTINUITY LOCK      Locked Fields / Mutable Fields / Inherited IDs
08 QA                   Score / Hard Fail Check / PASS·NEEDS_REVISION·REBUILD
09 ITERATION REPORT     本轮修改层级 / 修改模块 / 保持锁定模块 / 受影响ID
```

## 参考文件索引（按需加载，不要一次全读）
| 文件 | 何时读 |
| --- | --- |
| references/01_visual-dna-design.md | Stage 03 Visual DNA 设计规则（色彩/光影/材质/摄影/Motion 五类 DNA 定义） |
| references/02_asset-system.md | 资产工程规则、资产依赖图与版本管理、锁定等级、参考图继承规则 |
| references/03_prompt-keyframe.md | 短剧专项规则、连续关键帧导演规则、Prompt 差异化编译、动态负向系统 |
| references/04_change-routing.md | C 项三轮变更路由（L1–L5 层级判断与输出规范） |
| references/05_qa-tests.md | 失败恢复机制、A/B/C 回归测试、反模板与增量测试、QA 评分、Showcase 规划、TEST CASES v4.0 |
