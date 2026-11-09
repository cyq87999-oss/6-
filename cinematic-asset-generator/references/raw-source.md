# 电影级概念艺术资产生成器 · 短剧/漫剧视觉资产生产 Skill v4.0

> 本文件为技能完整规范（v4.0）。Skin 入口见同级 `SKILL.md`。以下内容原样保留，作为 Ada 执行视觉资产生产的方法论依据。

## 核心定位

本 Skill 的核心不是"生成一张漂亮概念图"，而是解决短剧/漫剧生产中的具体问题：**故事已经成立，但人物、场景、道具、镜头各自好看，放到一起却不像同一部作品。**

它把"一句话故事/世界观"转化为一套可以持续使用的视觉生产资产：
**故事输入 → 视觉DNA → 资产矩阵 → 关键帧 → Prompt编译 → 连续性锁 → QA → 多轮增量迭代**

核心判断标准：每个资产都必须回答"它如何服务这个故事"，而不是只回答"它是否好看"。

## 核心生产链（core_pipeline）

`STORY_VISUAL_BRIEF → WORLD_BUILD → VISUAL_DNA → ASSET_MATRIX → KEYFRAME_STORYBOARD → PROMPT_COMPILER → CONTINUITY_QA → ITERATION_ROUTER`

- Asset ID 与 Visual DNA 继承关系，用于多角色、多场景、多镜头持续生产。
- C 项 L1-L5 变更路由：根据用户修改粒度决定局部重算还是世界层重构。
- 关键帧必须具有"建立→变化→钩子"叙事功能，避免概念图拼贴成 PPT。
- B 项强制增量规则：自动产出必须形成可验证的资产系统，而非单张漂亮图。

## 原创边界

不复制具体影视 IP、具体角色独有设计或整段现成 Prompt；可使用通用摄影、材质、构图和公共文化元素，并重新组织其因果关系与生产逻辑。

## 标准输出 Schema v3.001

- **01 STORY VISUAL BRIEF**：主角目标 / 核心冲突 / 情绪基调 / 视觉隐喻 / 关键视觉事件
- **02 WORLD CARD**：WORLD ID / 世界观一句话 / 时间·地域 / 世界规则 / 美术生产规则 / 视觉禁区
- **03 VISUAL DNA**：Color DNA / Light DNA / Material DNA / Camera DNA / Production Design DNA / Motion DNA
- **04 ASSET MATRIX**：Anchor·Support·Decorative / Character IDs / Location IDs / Prop IDs / Shot IDs
- **05 KEYFRAME STORYBOARD**：Shot Size / Lens / Camera Height / Movement / Composition / Action Start-End / Information Change / Hook to Next Shot / Continuity
- **06 PROMPTS**：IMAGE PROMPT / VIDEO PROMPT / NEGATIVE PROMPT
- **07 CONTINUITY LOCK**：Locked Fields / Mutable Fields / Inherited IDs
- **08 QA**：Score / Hard Fail Check / PASS·NEEDS_REVISION·REBUILD
- **09 ITERATION REPORT**：本轮修改层级 / 修改模块 / 保持锁定模块 / 受影响 ID

## 目标赛道与应用场景

默认垂直：短剧/漫剧的前期视觉策划与连续视觉资产生产。适用：短剧人物设定与角色定稿、漫剧主视觉与角色立绘、短剧场景圣经、关键道具设定、连续镜头关键帧、AI 视频前的首帧/尾帧设计、多集内容的角色与空间一致性管理。不以"电影/动画/游戏全覆盖"为主卖点。

## A/B/C 评测设计

- **A｜不用 Skill 直接 Prompt**：允许直接生成单张角色图、场景图或概念图。
- **B｜仅使用 Skill 全自动**：给出一句话故事/世界观后，不得停在"请选择方向"，必须自动完成：故事视觉任务判断 → WORLD CARD → VISUAL DNA → 默认资产矩阵（≥1角色+1场景+1道具+3关键帧）→ 图像/视频/Negative Prompt → Continuity Lock → QA 评分 → 下一步扩展建议。
  - B 项强制增量规则：相较普通 Prompt，Skill 结果至少新增三类可验证资产——世界级绑定（Visual DNA/WORLD ID）、生产级绑定（Character/Location/Prop/Shot ID）、连续性绑定（Continuity Lock + QA）。只生成一张漂亮图而无资产系统 = B 失败。
- **C｜三轮对话**：必须维护同一 WORLD ID，根据用户修改判断影响层级，只重算受影响模块。禁止每轮从头生成完整世界导致人物/服装/空间/色彩漂移。

## 输入协议

### 3.1 最小输入
支持只有一句自然语言（如"一个记忆被删除的东方末世女侦探"）。最低输入只需一个可识别创作核心；不强制人物、时代、场景、风格、画幅或模型。

### 3.2 自动补齐原则
信息缺失时主动补齐，不得因低影响缺项停止、反复追问或要求选择。顺序：内容核心 → 故事功能 → 世界设定 → 主角设定 → 核心场景 → 核心道具 → Visual DNA → 关键帧 → Prompt 参数。补齐后直接进入生产，不要求二次确认。

### 3.3 自动补齐规则
- A 内容类型：偏人物→角色资产；偏空间→场景资产；偏事件→关键帧；人物+事件→默认短剧/漫剧视觉资产。未明确默认短剧/漫剧视觉资产。
- B 角色：自动补齐年龄段、外貌、体态、服装、职业/身份、核心道具、情绪基调；所有补齐必须服务故事核心，禁止随机堆砌"机甲/霓虹/皮夹克"等泛化视觉词。
- C 世界：自动补齐时间、地域、社会环境、建筑环境、天气/空气、世界规则与人物相关视觉背景。
- D 场景与道具：默认 1 核心场景 + 1 核心道具；道具必须能影响人物选择、信息或剧情。
- E 视觉：未指定时默认电影级写实概念艺术、低饱和、明确冷暖关系、真实材质、统一摄影语言、剧情服务型光影。不得机械套用"黑金/霓虹/赛博朋克/体积雾"。
- F 画幅：未指定默认 16:9；识别短视频/抖音/手机竖屏/漫剧竖版时自动切 9:16。
- G 模型：未指定用 Generic Prompt；指定 Seedance/Anycool 等仅改变表达语法，不改变 WORLD、Visual DNA、资产锚点与连续性规则。

### 3.4 自动补齐优先级
用户明确输入 > 本轮明确修改 > 已锁定资产 > 自动推断默认值。先保证故事功能，再保证视觉合理，最后补齐风格参数。任何自动补齐不得覆盖用户明确提供的信息。

### 3.5 自动补齐锁定
内部建立 `auto_fill`（input_core / content_type / story_function / protagonist / world / location / prop / visual_style / aspect_ratio / target_model / confidence），并生成 WORLD / CHARACTER / LOCATION / PROP / SHOT ID。

### 3.6 最小输入首轮交付要求（B 测试）
一次性完成：一句话创作判断 → WORLD CARD → Visual DNA → 1主角 → 1核心场景 → 1核心道具 → 3连续关键帧 → 图像 Prompt → 视频 Prompt → 动态 Negative Prompt → Continuity Lock → QA 分数。不得输出"请补充设定/请选择风格/请选择模型"作为首轮阻塞。

### 3.7 明确冲突信息处理规则
识别 → 分层 → 优先级判断 → 兼容尝试 → 局部更新/整体回退。优先级默认：本轮用户明确修改 > 用户早期明确设定 > 已锁定资产 > 自动推断。同层冲突以最新明确修改为准；不同层冲突优先保证：故事逻辑 > 人物逻辑 > 世界逻辑 > 视觉表现 > 风格偏好。可兼容时优先用世界观解释同时成立；无法兼容时不随机拼接，采用高优先级信息。

### 3.8 冲突处理输出模板
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
轻微冲突不阻塞；仅当冲突导致核心人物身份、世界规则或故事目标无法确定时，才允许提出一个最小澄清问题。

### 3.9 连续对话变更语义
"增加"=补充；"调整"=局部修改；"改成"=覆盖；"不要/去掉"=删除；"保留其他不变"=仅修改指定模块。用户未明确要求重置，不得重置其他已锁定设定。

## 创作流程（Stage 01-09）

- **Stage 01 STORY VISUAL BRIEF**：提取主角目标、核心冲突、情绪基调、世界规则、视觉隐喻、关键视觉事件。
- **Stage 02 WORLD BUILD**：生成 WORLD ID，确定时间/地域、空间逻辑、美术生产规则、视觉禁区。
- **Stage 03 VISUAL DNA**：建立 Color / Light / Material / Camera / Production Design / Motion DNA。
- **Stage 04 ASSET MATRIX**：自动决定最少可交付资产（Character/Location/Prop/Shot Keyframe）。
- **Stage 05 ASSET DEPTH**：Anchor（必须锁定）/ Support（服务剧情）/ Decorative（必要时）分级。禁止无故事功能的资产堆砌。
- **Stage 06 KEYFRAME STORYBOARD**：至少 3 关键帧（进入场 / 信息·动作变化 / 下一镜头钩子）。
- **Stage 07 PROMPT COMPILER**：输出图像、视频、负向 Prompt，不同模型只改执行格式不改视觉核心。
- **Stage 08 CONTINUITY QA**：检查人物、服装、道具、空间、光线、时间、动作、视线、镜头轴线。
- **Stage 09 DELIVERY**：优先交付可直接使用结果，再给简要方法说明，避免倾倒内部工程过程。

## Visual DNA 设计规则

- 5.1 色彩：主色/辅色/点缀色/阴影色/高光色。
- 5.2 光影：主光方向、光质、对比度、空气感、反射逻辑。
- 5.3 材质：至少 3 类具有故事功能的材质，不允许只写"高级/电影感"。
- 5.4 摄影：根据叙事目的选择景别、焦段、机位和运动。
- 5.5 Motion DNA：人物运动节奏、环境运动、动态光源或可视化能量运动。

## 资产工程规则

- WORLD ID：`WORLD-001`
- CHARACTER ID：`WORLD-001-CHAR-001`
- LOCATION ID：`WORLD-001-LOC-001`
- PROP ID：`WORLD-001-PROP-001`
- SHOT ID：`WORLD-001-SHOT-001`
- **Asset Inheritance**：每个子资产必须继承 Color/Light/Material/Camera/Production Design DNA。允许变化：动作、表情、镜头、天气、叙事时间；不允许无理由改变核心身份锚点。

## 短剧/漫剧专项规则

概念艺术必须与剧情直接连接：
- 每个角色回答：他/她是谁？要什么？怕什么？哪件视觉物件代表其冲突？
- 每个场景回答：这里发生什么？观众应该先看哪里？下一场能从哪里接？
- 每个道具回答：它如何改变人物选择或剧情信息？
- 每个关键帧回答：这一镜比上一镜多了什么信息？下一镜为什么必须发生？

## C 项三轮变更路由（L1-L5）

- **L1 视觉表层变更**：颜色、光线、镜头、构图、天气 → 只更新对应 Visual DNA 子字段。
- **L2 资产层变更**：服装、发型、道具、场景布局 → 更新对应 Asset ID，继承 WORLD DNA。
- **L3 人物层变更**：身份、年龄段、性格、动机、关系 → 重算角色及受影响镜头，不重置世界观。
- **L4 故事层变更**：冲突、关键事件、结局、时间线 → 重算 Story Visual Brief 与相关 Shot；保留不受影响资产。
- **L5 世界层变更**：时代、地域、核心规则、文明设定 → WORLD ID 升级版本；重新运行 Visual DNA，其余资产标记需复核。
输出时明确："本轮修改了什么 / 哪些资产保持锁定 / 哪些资产需要重生成"。

## 连续关键帧导演规则

每组至少三镜：Shot A 建立（交代人物、空间和视觉关系）；Shot B 变化（动作/信息/光线/空间至少一变）；Shot C 钩子（动作未完成/信息未揭示/视线指向/空间入口/新视觉焦点）。禁止三镜只是"同一画面不同角度"。

## Prompt 差异化编译

- 图像 Prompt 结构：`WORLD + SUBJECT + DESIGN + ENVIRONMENT + MATERIAL + LIGHT + CAMERA + COMPOSITION + COLOR + CONSISTENCY + NEGATIVE`
- 视频 Prompt 额外加入：动作起点 + 动作终点 + 环境运动 + 摄影机运动 + 节奏 + 衔接点
- 模型适配：Seedance/Anycool/Generic 三种模式只调整表达语法与限制，不得改变世界观核心 + 角色锚点 + 场景骨架 + 色彩/材质/摄影 DNA。

## 动态负向系统

禁止固定一长串负向词。根据当前资产动态生成：身份污染 / 时代污染 / 空间污染 / 摄影污染 / 材质污染 / 连续性污染 / 安全·审美风险。负向词不能误伤用户要求保留的核心科幻元素。

## 资产依赖图与版本管理

- 12.1 依赖关系：`WORLD → Visual DNA → (Character/Location/Prop/Shot)`；`WORLD → Story Visual Brief → Shot/Prop/Character`。每次修改先计算受影响节点，优先局部重算，只有世界层变化才允许大范围重构。
- 12.2 版本：WORLD-001 v1.0 初始；v1.1 不改变世界规则的视觉修订；v2.0 世界规则实质变化。角色/场景/道具/镜头需重算时递增版本；未受影响资产继续继承。
- 12.3 锁定等级：HARD LOCK（身份、核心服装、关键道具功能、世界规则）；SOFT LOCK（光线、天气、动作、镜头、构图）；OPEN（临时背景细节、装饰性元素）。只有用户明确要求时 HARD LOCK 才可解除。

## 参考图继承规则

用户提供参考图时判断功能：CHARACTER REF（继承身份锚点）/ LOCATION REF（继承空间骨架）/ PROP REF（继承形态与功能）/ STYLE REF（仅继承视觉语言，不复制具体作品独特表达）。参考图不是绝对复刻指令，必须优先服从当前故事与已锁定世界规则。多张冲突时按：用户明确指定用途 > 主参考图 > 次参考图 > 自动补齐。

## 失败恢复机制

QA 低于阈值时不默认整包重做，定位失败层：故事功能弱→回退 STORY VISUAL BRIEF；世界风格泛化→回退 WORLD/VISUAL DNA；人物漂移→回退 CHARACTER HARD LOCK；场景变化过大→回退 LOCATION 骨架；道具失去剧情功能→回退 PROP 依赖；三镜断裂→回退 SHOT STORYBOARD；Prompt 不可执行→回退 PROMPT COMPILER；多轮漂移→回退 CONTINUITY LOCK + 资产版本。必须说明：失败层、修复动作、保持锁定内容。

## 输出层级与信息密度

三层输出：L1 直接可用结果（角色/场景/道具/关键帧与可复制 Prompt）；L2 工程锁定（WORLD ID、资产 ID、Visual DNA 摘要、Continuity Lock）；L3 验证摘要（QA、变更范围、需重生成资产）。内部推理过程不得全部倾倒给用户。

## A/B/C 回归测试

- Case A 直出：允许只生成一张概念图。
- Case B Skill 全自动：同一输入必须自动形成 WORLD + Visual DNA + Character + Location + Prop + 3 Shots + Prompts + Continuity Lock + QA。
- Case C 三轮对话：R1 原始最小输入；R2 仅修改人物；R3 修改道具或结局。通过条件：WORLD ID 不无故重置；未修改资产一致；受影响资产正确重算；冲突有明确处理；最终保持视觉与故事连续性。

## 反模板与增量测试

至少两个风格差异极大输入测试。若仍重复"黑金/霓虹/赛博/皮夹克/雾/高楼"，判定 Visual DNA 失败。B 与 A 的差异必须可观察；若 B 只比 A 更长但无新增资产系统/连续性/故事服务/可复用结构，判定增量不足。

## QA 评分（100 分）

- 故事视觉转译 15 / Visual DNA 完整度 15 / 角色·场景·道具一致性 15 / 关键帧连续性 15 / 摄影语言 10 / 材质·生产设计 10 / Prompt 可执行性 10 / 资产复用性 5 / 原创·合规 5
- 阈值：≥90 SHOWCASE READY；85–89 PASS；70–84 NEEDS_REVISION；<70 REBUILD
- 硬失败直接返工：角色身份漂移 / 核心场景骨架变化 / 关键道具消失·变形且无剧情理由 / 三镜无法连续 / B 模式未形成资产系统增量

## 自动扩展能力

用户只要求"做一个角色"，默认先提供最小可用资产胶囊，并告知可继续扩展：同角色三视图、表情包、日常/任务双形态、核心场景、关键道具、三镜关键帧。但不强行生成无关资产。

## 参赛模式输出

用户说"参赛展示"时，按：输入 → AI 自动解析 → Visual DNA → 资产矩阵 → 关键帧 → Prompt 编译 → 连续性前后对比 → QA → 规模化扩展。展示重点：同一 Skill 如何从一句话稳定产生一套可用于短剧/漫剧继续生产的视觉资产。

## 反模板测试

连续输入两个完全不同故事，若输出仍大量重复黑金/霓虹/赛博/皮夹克/雾/城市高楼，则判定 Visual DNA 失败，必须重新计算。同一故事修改角色后，世界观和场景骨架应保持稳定。

## 最终交付原则

先交付结果，再交付工程摘要。最小可交付不是"一个 Prompt"，而是：**一套有世界观、资产 ID、视觉 DNA、关键帧和连续性约束的可复用视觉资产胶囊**。

## Showcase 规划（示例）

- **Case A｜东方末世悬疑短剧**：输入"一个失去部分记忆的女侦探，在废弃东方城市寻找最后一座地下档案馆"。自动产出 WORLD CARD、冷灰青/旧铜/暗红视觉 DNA、女侦探 CHAR-001、地下档案馆 LOC-001、记忆存储器 PROP-001、3 镜关键帧、图像/视频 Prompt、Continuity QA。
- **Case B｜都市奇幻漫剧**：输入"一个普通外卖员发现城市夜间的所有订单，其实都来自不存在的街区"。验证：输出必须重新计算视觉 DNA，不得复制 Case A 黑金末世模板。
- **Case C｜同世界多资产扩展**：在 Case A 保持 WORLD ID 不变前提下，连续生成配角 2 名、新地点 2 个、道具 3 件、新镜头 6 个；要求角色、建筑骨架、光线规则、材质体系、镜头语言稳定继承。
- **C 项三轮展示**：R1 生成主角+档案馆；R2 只改主角服装与情绪→世界和场景不漂移；R3 把"档案馆"改为移动式地下列车空间→只重算 L4 故事/场景相关模块。

## TEST CASES v4.0

- **Case 01｜最小输入**：输入"一个记忆被删除的东方末世女侦探"。预期：不追问，自动补齐并完成 B 全自动输出。
- **Case 02｜人物局部修改**：R1 用 Case 01；R2 把女主改成 42 岁普通女性。预期：只重算 CHARACTER 与相关 SHOT，WORLD/LOCATION/PROP 继承。
- **Case 03｜资产冲突**：R3 把记忆终端改成女儿留下的旧玉佩。预期：触发资产层冲突处理；更新 PROP 与受影响 SHOT，不重置世界。
- **Case 04｜世界层修改**：输入把东方末世城市改为宋代未来水城。预期：进入 L5；WORLD 与 Visual DNA 重新计算，其余资产标记待复核。
- **Case 05｜反模板**：输入 A 东方末世女侦探；输入 B 温暖海岛上的少年园丁。预期：两组 Visual DNA 明显不同，不得机械复用黑金、霓虹、雾等模板。
