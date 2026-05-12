# 项目专属知识库索引

> **项目ID**：`{PROJECT_ID}`
> **项目名称**：{PROJECT_NAME}
> **游戏类型**：{GAME_TYPE}
> **题材**：{THEME}
>
> **使用说明**：
> 1. 将 `{PROJECT_ID}`、`{PROJECT_NAME}`、`{GAME_TYPE}`、`{THEME}` 替换为实际值
> 2. 项目知识库支持按二级目录分类组织（如系统资料、战斗资料等），可根据项目需要自行调整目录结构
> 3. 删除或补充下方文件清单中的条目
> 4. 将各占位符文件替换为实际内容

---

## 目录结构

可在项目知识包根目录下按需创建二级子目录，分类管理各类知识文件：

```
{项目知识包根目录}/
├── INDEX.md                # 本文件
├── 文案资料/               # 文案策划相关
│   ├── INDEX.md
│   └── tone_and_voice.md   # Tone & Voice 语气与文风规范
├── 系统资料/               # 系统策划/数值策划相关
│   ├── INDEX.md
│   ├── attribute_system.md # 属性体系详解
│   ├── character_power.md  # 角色战力计算规则
│   └── progression_systems.md  # 养成系统详解
├── 战斗资料/               # 战斗策划相关（建议项）
│   ├── INDEX.md
│   └── combat_formulas.md  # 战斗公式
├── 数据资料/               # 数据/经济相关（建议项）
│   ├── INDEX.md
│   └── economy_system.md   # 经济系统
├── 关卡资料/               # 关卡策划相关（建议项）
│   ├── INDEX.md
│   └── level_structure.md  # 关卡结构
└── scenes/                 # 自定义场景（可选）
    └── ...                 # 见 scenes/INDEX.md
```

---

## 知识库文件清单

| 路径 | 内容 | 适用岗位 |
|------|------|----------|
| `文案资料/tone_and_voice.md` | Tone & Voice 语气与文风规范 | 文案策划、系统策划 |
| `系统资料/attribute_system.md` | 属性体系详解 | 数值策划、战斗策划、系统策划 |
| `系统资料/character_power.md` | 角色战力计算规则 | 数值策划、系统策划 |
| `系统资料/progression_systems.md` | 养成系统详解 | 数值策划、系统策划、战斗策划 |
| `战斗资料/combat_formulas.md` | 战斗公式（建议项） | 数值策划、战斗策划 |
| `数据资料/economy_system.md` | 经济系统（建议项） | 数值策划、系统策划 |
| `关卡资料/level_structure.md` | 关卡结构（建议项） | 关卡策划、系统策划 |
| `scenes/` | 自定义场景目录 | 不限（按角色子目录分类） |

> **注意**：文件路径以 `{project_knowledge_path}/` 为根目录。二级目录名可根据项目实际需要调整（如改用英文目录名）。

---

## 使用方式

1. **自动注入**：Skill 从 `user_state.yaml` 中读取 `project_knowledge_path`，自动加载对应项目知识包
2. **优先级**：当项目知识库与通用知识库冲突时，**以项目知识库为准**
3. **更新机制**：项目设计规则变更时，同步更新本项目知识包中的对应文件
4. **目录结构**：可根据需要自由组织二级目录，不改变加载逻辑
5. **禁止假设**：Skill 在使用项目知识库时，不得擅自增删或修改已定义的规则

---

## 知识库状态

- [ ] 文案资料/tone_and_voice.md（Tone & Voice 规范）
- [ ] 系统资料/attribute_system.md（属性体系）
- [ ] 系统资料/character_power.md（角色战力）
- [ ] 系统资料/progression_systems.md（养成系统）
- [ ] 战斗资料/combat_formulas.md（战斗公式）— 建议项
- [ ] 数据资料/economy_system.md（经济系统）— 建议项
- [ ] 关卡资料/level_structure.md（关卡结构）— 建议项
- [ ] 自定义场景（scenes/INDEX.md）
