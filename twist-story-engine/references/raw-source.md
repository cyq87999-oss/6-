# 反转故事发动机 V3.0 · 完整规范

## 0. 核心定位
服务抖音短视频故事、短剧、漫剧与 AI 短剧前期脚本。解决：开头无必看理由 / 中段无有效变化 / 反转靠临时揭密 / 人物为反转降智 / 结尾无回看价值 / 多轮修改漂移。
方法：观众认知状态→预期建立→误导→证据升级→反转→人物选择→认知重构→余震/追更。
原则：反转不是"多一个秘密"，而是新信息迫使观众重新解释旧信息。

## 1. 定位与场景
15–90 秒抖音短故事；1–5 分钟短剧/漫剧单集；AI 短剧前期故事核与脚本；连载单集开场/冲突/悬念/结尾。覆盖生活、情感、家庭、职场、悬疑、轻惊悚、黑色幽默、奇幻。不以复制具体影视作品、固定名场面或热点表达为目标。

## 2. 最小输入与自动补齐
一句自然语言创意即可。缺参时自动推断，不因低风险缺项反复追问。
```yaml
auto_fill:
  platform: "抖音"
  format: "60秒短故事"
  audience: "大众短视频用户"
  genre: "由故事种子自动判断"
  protagonist_count: 1
  relationship_count: 1
  twist_mode: "自适应"
  foreshadow_count: "按时长自适应"
  ending_mode: "认知重构+人物选择或追更问题"
```
顺序：内容核心→题材判断→故事功能→主角→核心关系→世界/场景→反转机制→节奏→输出格式。用户明确输入优先。

## 3. 输入冲突处理
优先级：本轮明确修改 > 早期明确设定 > 已锁定故事状态 > 自动补齐。
流程：识别冲突→判断层级→判断补充/修改/覆盖/否定→尝试内部兼容→局部重算→QA。
可见输出：本轮修改（内容+层级）/已更新/保持锁定/当前生效设定。不展示内部完整推理。

## 4. 故事状态、版本与回退
```yaml
story_state:
  story_id: ; version: ; protagonist_id:
  relationship_ids: [] ; clue_ids: [] ; twist_ids: [] ; beat_ids: []
  ending_id: ; audience_belief_states: [] ; snapshots: [] ; current_output_mode:
snapshot:
  version: "V1/V2/V3" ; reason:
  locked_elements: [] ; changed_elements: []
```
"恢复上一版结局"→恢复对应 Snapshot，不重新随机创作。

## 5. 创作状态机
INIT→IDEA_DIAGNOSIS→GENRE_STRATEGY→AUDIENCE_PROMISE→STORY_CORE→AUDIENCE_BELIEF_MAP→REVERSAL_MODE_SELECT→EXPECTATION_BUILD→CLUE_LADDER→CHARACTER_LOGIC→THREE_AXIS_RHYTHM→IRREVERSIBLE_CHOICE→ENDING_DESIGN→FAIR_PLAY_TEST→ORIGINALITY_TEST→PLATFORM_PACKAGING→QA→DELIVERY
必过：STORY_CORE / REVERSAL_MODE_SELECT / CHARACTER_LOGIC / CLUE_LADDER / FAIR_PLAY_TEST / QA。

## 6. 题材适配矩阵
悬疑→信息/因果（次：规则/关系）→回看型；情感→关系/情绪（次：人物）→余震型；家庭→关系/记忆（次：价值观）→选择型；职场→利益/身份（次：规则）→立场型；轻惊悚→认知/规则（次：时间）→余震/追更；黑色幽默→立场/因果（次：选择）→反讽型；奇幻→规则/身份（次：时间）→世界重构；生活故事→情绪/关系（次：信息）→情绪余震。
题材不明时自动判断。禁止机械套"事件→人物→认知"三层。

## 7. Audience Promise
第一步明确"观众为什么继续看"。候选：异常事实 / 即将发生的危险 / 无法解释的行为 / 违反常识的规则 / 关系秘密 / 结果先行 / 必须回答的问题。
前段优先：异常+问题 / 结果+秘密 / 关系+危机。禁用"炸裂、震惊、天价"等空词。

## 8. Story Core
模板：【主角】为了【欲望】，必须面对【障碍】，却逐渐发现【隐藏事实】，最终必须在【选择A】与【选择B】之间付出代价。
隐藏事实必须对人物选择产生实际影响；删除后故事仍成立→反转依赖不足，回退加深。

## 9. Audience Belief Map
```yaml
belief_state:
  at_time: ; audience_belief: ; evidence:
  uncertainty: ; new_question: ; forced_reinterpretation:
```
默认路径：我以为A→发现异常→暂信B→新证据→B被修正→接近C→C也不完整→真相D。
每主要 beat 至少改变"判断/风险/关系/目标/选择"之一。

## 10. 反转机制
A 信息（事实与理解不同）；B 立场（好坏对错被重释）；C 因果（因果倒置）；D 时间（发生时间不同）；E 关系（真实关系重定义）；F 规则（世界真正规则不同）；G 选择（答案是主角最后选什么）；H 情绪（情感目的被重释）。可混 1-3 种，必须有主反转，禁止机械堆叠。

## 11. 自适应反转深度
15s=1 强反转；30s=1 主+局部预期修正；45-60s=1 主+1 次人物/认知重解释；60-90s=2 层复合优先；90s+=3 层或多阶段重构。铁律：少而准，不为层数硬加。

## 12. 伏笔系统
```yaml
foreshadow:
  id: ; planted_at: ; surface_meaning: ; hidden_meaning:
  payoff_at: ; payoff_type: ; reread_value: ; importance:
```
密度：15s 1-2；30s 2-3；45s 2-4；60s 3-4；90s 3-5；2min+ 4-7。服务剧情，禁止硬塞。至少一个重要伏笔在真相后产生新意义。

## 13. Character Logic
```yaml
character:
  want: ; fear: ; secret: ; false_belief: ; self_justification:
  evidence_for_belief: ; blind_spot: ; relationship:
  irreversible_choice: ; emotional_cost:
```
主角必须相信关键行动"有理由"；禁止为反转突做前文无法解释的行为。
优秀结构：错误信念→合理证据→合理行动→新真相→旧行动获新意义。

## 14. 三轴节奏
```yaml
beat:
  time: ; information_change: ; risk_change: ; emotion_change:
  visual_change: ; dialogue_function: ; audience_question: ; next_action:
```
掉速判定：仅当连续两个 beat 出现"信息+风险+情绪+关系/选择"全不变才算真掉速；纯情绪表演可持续。

## 15. 信息预算（软约束）
15s 2-3 · 30s 3-5 · 60s 5-8 · 90s 8-12 次有效变化，须影响事实/风险/关系/目标/判断/选择/情绪之一。

## 16. 不可逆选择
主角知道部分真相→仍必须选择→产生代价→暴露价值观。结尾优先"真相+选择"，非"真相+作者宣布答案"。

## 17. Fair Play 测试
F1 证据存在；F2 不可轻易猜中；F3 回看成立；F4 不靠临时信息；F5 人物仍合理。不满足两项以上→重写反转架构。

## 18. 原创度 / 换皮
"仅换名字/职业/地点是否仍成立？"是→模板化。须加：独特动机/关系机制/世界规则/关键道具/因果链/选择成本。禁止连续默认"失踪+身份反转+监控录像+原来是你"。

## 19. 抖音短内容适配
只优化可控目标，不承诺流量。前段：异常+具体问题+行动。中段：新证据/新误判/新代价/新关系/新选择。结尾：认知重构/人物选择/情绪余震/可追问新问题/自然讨论点。不以"点赞关注"为唯一钩子。

## 20. 观点冲突与评论
```yaml
stance:
  side_a: ; side_b: ; core_value_conflict: ; question:
```
无自然冲突不强行制造争吵。可生成：讨论问题 2 / 站队问题 1 / 结局争议点 1。

## 21. 连载引擎
```yaml
series_state:
  season_core: ; protagonist_arc:
  unresolved_clues: [] ; resolved_clues: []
  recurring_relationships: [] ; episode_history: [] ; next_episode_hook:
```
单集：一个局部闭环 + 一个高价值未解决问题。禁止每集同一种突然反转。

## 22. 三轮对话路由
L1 表层→局部改；L2 人物→重算人物及受影响 beat；L3 故事→重算依赖链；L4 世界→重算 Story Core 与下游；L5 输出→只改表达保留状态；L6 组合→先列变更再判依赖，只重算受影响模块。

## 23. Snapshot / Rollback
"恢复上一版结局""还是用第二轮人物设定"→恢复最近符合要求的 Snapshot，禁止随机重写近似版本。

## 24. 故事依赖图
STORY_CORE ├ GENRE_STRATEGY ├ CHARACTER└RELATIONSHIP ├ WORLD_RULE ├ FORESHADOW ├ REVERSAL ├ EMOTION └ ENDING。
上游变化→检查全部下游；下游变化→不污染无关上游。

## 25. 输出模式
A 全自动（默认成品优先）：标题/01 一句话故事核/02 前3秒钩子/03 故事正文/04 结尾钩子/05 讨论问题 + 简版结构摘要（反转类型/伏笔数/QA分数/扩展方向）。
B 完整结构：Audience Promise / Belief Map / Story Core / Genre Strategy / Reversal Architecture / Foreshadow Table / Character Logic / Three-Axis Rhythm / Ending / QA。
C 60秒脚本：时间/场景/景别/动作/台词/音效/BGM/信息变化/情绪/伏笔/钩子。
D 漫剧脚本：格数/画面/动作/台词/构图/镜头/连续性/伏笔。
E 大纲：故事核/三幕/关键事件/人物/反转/结局。

## 26. 自动失败恢复
反转太弱→REVERSAL_MODE_SELECT/FAIR_PLAY_TEST；反转过多→降层；伏笔过密→CLUE_LADDER；伏笔无法回收→STORY_CORE/REVERSAL；人物工具人→CHARACTER_LOGIC/SELF_JUSTIFICATION；中段掉速→THREE_AXIS_RHYTHM/INFORMATION_BUDGET；结尾硬拗→IRREVERSIBLE_CHOICE/ENDING_DESIGN；三轮后漂移→STORY_STATE/SNAPSHOT/DEPENDENCY_GRAPH；模板同质化→GENRE_STRATEGY/ORIGINALITY_TEST。不得局部失败就全量随机重写。

## 27. 质量评分（100）
前段钩子15 / 信息·风险·情绪节奏15 / 反转架构20 / 伏笔与公平性15 / 人物逻辑与选择15 / 结尾价值10 / 原创度去模板5 / 平台表达5。
90-100 SHOWCASE READY；80-89 PASS；70-79 局部返工；<70 回退 STORY_CORE/REVERSAL。
必败项：反转全靠临时信息；主角关键行为无动机；核心伏笔不回收；开头与主体无关；连续两个以上 beat 三轴同时不变；结尾与人物选择无关；换皮后完全一样；为讨论虚构重大现实事件。

## 28. 平台包装
信息型标题1 / 冲突型标题1 / 悬念型标题1 / 封面短文案2 / 评论问题2 / 搜索友好标签5-8 / 下一集钩子1。标题文案不得虚构现实事实。

## 29. 参赛模式
展示重点：同一句输入，普通 Prompt 输出一个故事；Skill 输出一套可解释、可修改、可持续生产的故事结构。推荐 Showcase：生活反转 / 悬疑反转 / 情感反转，三个案例必须用不同反转机制。

## 30. 最终交付原则
内部复杂、外部简单；先给成品，再给结构。用户能直接：看故事/改故事/转短剧/转漫剧/继续连载/恢复上一版。

---

## 附：Quality Rubric V3.0（必检）
开头是否建立具体观众问题；题材是否匹配反转机制；每主要 beat 是否至少改变信息/风险/情绪/关系/选择之一；核心伏笔是否有证据且回收；人物关键行为是否符合自我解释；反转是否通过 Fair Play；换皮后是否仍成立；结尾是否重释前文或形成不可逆选择；三轮修改后状态是否稳定；输出是否先成品后分析。

## 附：回归测试
- Case 1 最小输入全自动："一个记忆被删除的东方末世女侦探。"→自动补齐不停在澄清。
- Case 2 人物→故事→回退："普通女孩地铁上发现所有人都没有影子的60秒悬疑"→改人物（地铁检修员+母女关系）→改结局（母亲是第一批没有影子的人）→"恢复上一版结局"用 Snapshot。通过：人物修改不破坏世界规则；故事修改重算受影响伏笔与结局；回退不随机重写；无关模块锁定。
- Case 3 格式切换：大纲→60秒脚本→10格漫剧，故事核/人物/反转/伏笔/结局因果一致，只改表达。
- Case 4 反转公平性：真相有前文证据；开头不锁死答案；揭示后旧场景获新意义；不依赖临时新人/新规则。
- A/B 对比：A 普通 Prompt 直出；B 必须有 Genre Strategy / Audience Promise / Belief Map / 自适应反转 / 按时长伏笔 / self_justification / 三轴 / Fair Play / 换皮测试 / 不可逆选择 / 结尾重构价值 / 自然讨论点——不是"更长"，而是因果与结构可验证增量。
