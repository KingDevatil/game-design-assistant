# 知识库加载策略（详细规范）

> 本文档是 SKILL.md 知识库架构章节的完整参考。各角色的 `system.md` 只需简要引用，不应重复粘贴。

## 通用知识库目录结构

```
references/game_context/
├── game_types/              # 通用：游戏类型知识库（13个类型）
│   ├── side_scrolling_fighter.md
│   ├── mmorpg.md
│   ├── arpg.md
│   ├── moba.md
│   ├── open_world_rpg.md
│   ├── roguelike.md
│   ├── card_game.md
│   ├── strategy.md
│   ├── shooter.md
│   ├── idle.md
│   ├── battle_royale.md
│   ├── gacha_rpg.md
│   └── simulation.md
├── themes/                  # 通用：游戏题材知识库（9个题材）
│   ├── three_kingdoms.md
│   ├── fantasy.md
│   ├── sci_fi.md
│   ├── wuxia_xianxia.md
│   ├── post_apocalyptic.md
│   ├── cyberpunk.md
│   ├── mythology.md
│   ├── horror.md
│   └── school_life.md
├── cross_matrix/            # 通用：类型×题材交叉矩阵（按需添加）
└── systems/                 # 通用：游戏系统知识库（7大系统）
    ├── INDEX.md             # 系统知识库索引与按需加载策略
    ├── 01_core_gameplay.md  # 核心玩法：战斗、关卡、探索、PvP
    ├── 02_progression.md    # 养成系统：角色、装备、技能、羁绊、收集
    ├── 03_economy.md        # 经济系统：货币、产出、消耗、背包、交易
    ├── 04_monetization.md   # 商业化：抽卡、通行证、商城、广告
    ├── 05_social.md         # 社交系统：公会、好友、聊天、排行榜
    ├── 06_operations.md     # 运营活动：日常、限时活动、心流体验设计、赛季、版本
    └── 07_basic_functions.md # 基础功能：新手引导、UI/UX、设置、客服
```

## 项目专属知识包目录结构

```
# 用户工作区下的项目知识包（与 Skill 完全分离）
# 实际路径由 user_state.yaml 中的 knowledge_source.local.path 指定
{user_workspace}/
└── {project_name}/
    ├── INDEX.md                    # 项目知识包入口索引
    ├── 系统资料/                    # 系统策划/数值策划相关
    │   └── INDEX.md
    ├── 战斗资料/                    # 战斗策划相关（建议项）
    │   └── INDEX.md
    ├── 数据资料/                    # 数据/经济相关（建议项）
    │   └── INDEX.md
    ├── 文案资料/                    # 文案策划相关
    │   └── INDEX.md
    ├── 关卡资料/                    # 关卡策划相关（建议项）
    │   └── INDEX.md
    ├── scenes/                     # 自定义场景目录（可选）
    │   ├── INDEX.md
    │   ├── system-designer/
    │   │   ├── INDEX.md
    │   │   └── my_scene.md
    │   └── ...
    └── ...
```

## 加载优先级与切片策略

知识库按以下5层优先级组织，**高层级覆盖低层级**。为控制 token 消耗，采用**按需切片加载**，非必要不加载完整文件：

| 优先级 | 层级 | 目录 | 加载策略 |
|---|---|---|---|
| **P0** | 项目专属知识 | 由 `user_state.yaml` 中的 `knowledge_source` 配置 | **知识驱动 + 按需检索**：local 类型递归加载各子目录下的 `INDEX.md` 索引文件，按需切片加载具体段落。remote 类型使用环境中可用的检索工具（如 `search_knowledge`、`get_document`），以 `root_path` 为范围按需检索。若存在 `scenes/{current_role}/INDEX.md`，同时按角色加载自定义场景 |
| **P1** | 项目配置 | `user_state.yaml` 中的 `project_type` + `project_theme` | **摘要加载**：根据配置的游戏类型和题材标识，加载对应通用知识库的摘要 |
| **P2** | 交叉矩阵 | `cross_matrix/` | **条件加载**：仅当用户 query 同时涉及类型和题材特征时加载 |
| **P3** | 通用系统知识库 | `systems/` | **按需切片加载**：根据用户query关键词，只加载对应系统文件的对应段落 |
| **P4** | 通用类型/题材 | `game_types/`、`themes/` | **基础摘要加载**：加载当前项目类型/题材的核心特征摘要，不加载完整文件 |

### 冲突解决规则

| 场景 | 规则 | 示例 |
|---|---|---|
| **P0 vs 其他** | P0绝对优先 | 项目专属定义了"暴击率上限50%"，通用系统说"通常50-80%"，以项目专属为准 |
| **P1 vs P4** | P1覆盖P4 | 项目配置是"科幻题材"，通用题材文件是"奇幻"，以项目配置为准 |
| **P2 vs P4** | P2覆盖P4 | 交叉矩阵有"ARPG×科幻"专门设计，优先于单独的ARPG或科幻文件 |
| **P3 vs P4** | P3覆盖P4 | 某类型说"经济系统简单"，但systems/中有完整经济设计，以systems/为准 |
| **同层级** | 不冲突，可共存 | 同时加载`game_types/arpg.md`和`themes/fantasy.md` |
| **跨层级补充** | 低层级作为补充 | P0定义了具体数值，P3提供设计方法论，两者互补 |

**注意**：Skill不得擅自修改或增删P0项目专属知识库中已定义的项目规则。

### 切片加载示例

- 用户问"抽卡概率怎么设计" → 加载 `systems/04_monetization.md` 中的"抽卡/祈愿系统"段落 + 检索项目知识中的"稀有度定义"
- 用户问"装备强化" → 加载 `systems/02_progression.md` 中的"装备/圣遗物系统"段落
- 用户问"公会战" → 加载 `systems/05_social.md` 中的"公会/社团系统"段落
- 用户问"世界观设定" → 加载 `themes/{project_theme}.md` 摘要
- 用户问"关卡难度" → 加载 `systems/01_core_gameplay.md` 中的"关卡/副本系统"段落

## 项目知识绑定与初始化

**绑定流程**：
1. Skill 启动时读取 `user_state.yaml` 中的 `knowledge_source` 配置
2. 根据 `knowledge_source.type` 执行对应逻辑：
   - `local`：检查 `knowledge_source.local.path` 是否有效
   - `remote`：Agent 使用可用检索工具探索 `knowledge_source.remote.root_path`
   - `none`：跳过项目知识加载
3. 若类型为 `local` 且路径无效 → 引导用户初始化：
   - **选项A**：指定已有知识包目录的绝对路径
   - **选项B**：从模板创建新知识包（复制 `template/` 到用户指定路径）
   - **选项C**：切换为 `remote` 或 `none` 类型
   - **选项D**：暂不绑定（P0 层为空，仅使用 P1-P4 通用知识库）

**项目切换**：用户可通过修改 `user_state.yaml` 中的 `knowledge_source` 配置切换知识来源。切换后 Skill 重新加载对应 P0 层内容。

**remote 类型初始化**：Agent 初始化时自动检索 `root_path` 下的目录结构和文档列表，了解项目资料全貌，后续按需精确检索。不需要用户手动指定任何 MCP 工具名或服务地址。

## 自定义场景（项目专属）

用户可在项目知识包中定义**自定义场景**，扩展 Skill 的能力覆盖到项目特有的工作场景。自定义场景不写入 Skill 本体，完全外挂在项目知识包中。

**目录位置**：
```
{project_knowledge_path}/
└── scenes/
    ├── INDEX.md                    # 全局索引：说明按角色分类规则
    ├── system-designer/            # 系统策划自定义场景
    │   ├── INDEX.md                #   该角色场景索引
    │   └── {scene_id}.md           #   场景定义文件
    ├── writer/                     # 文案策划
    ├── balance-designer/           # 数值策划
    ├── level-designer/             # 关卡策划
    ├── combat-designer/            # 战斗策划
    ├── event-designer/             # 活动策划
    └── data-analyst/               # 数据分析师
```

**角色子目录索引格式**（每个 `scenes/{role}/INDEX.md`）：

| 场景ID | 名称 | 触发关键词 | 描述 |
|--------|------|-----------|------|
| `{id}` | {名称} | {关键词} | {描述} |

> 索引中不再需要"关联角色"字段——目录名已标识角色归属。

**加载规则**：
1. Skill 绑定项目知识包时，检查 `{project_knowledge_path}/scenes/` 下是否存在角色子目录
2. 遍历所有角色子目录，读取其中的 `INDEX.md`，加载所有已声明的自定义场景文件
3. 仅将**当前角色**的自定义场景加入场景匹配候选集（按目录角色过滤）
4. **优先级**：自定义场景与内置场景 ID 冲突时，**以自定义场景为准**（P0 级覆盖）

**使用限制**：
- 自定义场景不修改 Skill 本体中的任何文件
- 每个自定义场景文件需遵循与内置场景相同的结构规范（Scene ID / 目的 / 输入模板 / 处理逻辑 / 输出规范 / 自检清单）
- 场景 ID 建议使用有意义的英文标识（如 `CBT-boss_design`），避免与内置场景 ID 混淆
