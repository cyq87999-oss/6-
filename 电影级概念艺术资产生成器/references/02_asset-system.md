# 资产工程规则 · 依赖图 · 版本管理 · 参考图继承（对应 §6 / §12 / §13）

## 6. 资产工程规则

### ID 体系（全局唯一、层级派生）

```
WORLD ID      WORLD-001
CHARACTER ID  WORLD-001-CHAR-001
LOCATION ID   WORLD-001-LOC-001
PROP ID       WORLD-001-PROP-001
SHOT ID       WORLD-001-SHOT-001
```

### Asset Inheritance（继承关系）

每个子资产必须继承：Color DNA、Light DNA、Material DNA、Camera DNA、Production Design DNA。
允许变化：动作、表情、镜头、天气、叙事时间；不允许无理由改变核心身份锚点。

## 12. 资产依赖图与版本管理

### 12.1 依赖关系

```
WORLD
 ├─ Visual DNA
 │   ├─ Character
 │   ├─ Location
 │   ├─ Prop
 │   └─ Shot
 └─ Story Visual Brief
     └─ Shot / Prop / Character dependencies
```

每次修改先计算受影响节点，优先做局部重算，只有世界层变化才允许大范围重构。

### 12.2 版本

- `WORLD-001 v1.0`：初始世界
- `WORLD-001 v1.1`：不改变世界规则的视觉修订
- `WORLD-001 v2.0`：世界规则发生实质变化

角色、场景、道具和镜头在需要重算时递增版本；未受影响资产继续继承。

### 12.3 锁定等级

- **HARD LOCK**：身份、核心服装、关键道具功能、世界规则。只有用户明确要求时，HARD LOCK 才可解除。
- **SOFT LOCK**：光线、天气、动作、镜头、构图。可随叙事微调，但需记录。
- **OPEN**：临时背景细节、装饰性元素。最自由，不影响一致性。

## 13. 参考图继承规则

用户提供参考图时，先判断其功能：

- **CHARACTER REF**：继承人物身份锚点
- **LOCATION REF**：继承空间骨架
- **PROP REF**：继承道具形态与功能
- **STYLE REF**：仅继承视觉语言，不复制具体作品的独特表达

参考图不是绝对复刻指令；必须优先服从当前故事与已锁定世界规则。
多张参考图冲突时，按：用户明确指定用途 > 主参考图 > 次参考图 > 自动补齐。
