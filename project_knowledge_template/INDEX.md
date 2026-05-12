# 项目专属知识库索引

> **项目ID**：`{PROJECT_ID}`
> **项目名称**：{PROJECT_NAME}
> **游戏类型**：{GAME_TYPE}
> **题材**：{THEME}
>
> **使用说明**：
> 1. 将 `{PROJECT_ID}`、`{PROJECT_NAME}`、`{GAME_TYPE}`、`{THEME}` 替换为实际值
> 2. 删除或补充下方文件清单中的条目
> 3. 将各占位符文件（attribute_system.md 等）替换为实际内容

---

## 知识库文件清单

| 文件 | 内容 | 适用岗位 |
|---|---|---|
| `tone_and_voice.md` | Tone & Voice 语气与文风规范 | 文案策划、系统策划 |
| `attribute_system.md` | 属性体系详解 | 数值策划、战斗策划、系统策划 |
| `character_power.md` | 角色战力计算规则 | 数值策划、系统策划 |
| `progression_systems.md` | 养成系统详解 | 数值策划、系统策划、战斗策划 |
| `combat_formulas.md` | 战斗公式（建议项） | 数值策划、战斗策划 |
| `economy_system.md` | 经济系统（建议项） | 数值策划、系统策划 |
| `level_structure.md` | 关卡结构（建议项） | 关卡策划、系统策划 |
| `scenes/` | 自定义场景目录 | 不限（根据场景配置关联角色） |

---

## 使用方式

1. **自动注入**：Skill 从 `user_state.yaml` 中读取 `project_knowledge_path`，自动加载对应项目知识包
2. **优先级**：当项目知识库与通用知识库冲突时，**以项目知识库为准**
3. **更新机制**：项目设计规则变更时，同步更新本项目知识包中的对应文件
4. **禁止假设**：Skill 在使用项目知识库时，不得擅自增删或修改已定义的规则

---

## 知识库状态

- [ ] Tone & Voice 规范（tone_and_voice.md）
- [ ] 属性体系（attribute_system.md）
- [ ] 角色战力（character_power.md）
- [ ] 养成系统（progression_systems.md）
- [ ] 战斗公式（combat_formulas.md）— 建议项
- [ ] 经济系统（economy_system.md）— 建议项
- [ ] 关卡结构（level_structure.md）— 建议项
- [ ] 自定义场景（scenes/INDEX.md）
