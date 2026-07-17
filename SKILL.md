---
name: ai-short-ad
description: AI ad creation for Chinese social media (Douyin/Kuaishou). Use when user says "写剧本", "出新剧本", "来几个剧本", "写脚本", "短视频广告", "广告制作", or similar. Supports 15-20s short ad scripts (15s default, extendable to 18-20s when dialogue load requires). Product info provided at conversation time. For AI short drama, use ai-short-drama skill instead.
name_cn: AI广告制作
description_cn: AI广告制作，走量快产出，审核风控合规，产品口播位设计
create_source: super-agent-skill-creator
---

# AI广告制作

## 定位

走量、快产出、卖货转化。一天多条，角色不复用，台词走即梦原生生成。

## Boundaries

- Use this for: 走量卖货型 15-20s 短视频广告，一天多条，角色不复用
- Do not use for: AI 短剧（角色复用、音频驱动口型、多镜头叙事）— use ai-short-drama instead
- Do not use for: 品牌大片/情感微电影（需要叙事弧线和多场景切换）
- Default build tool: 即梦 Seedance 720p + Topaz 放大

## Leading Words

本技能使用以下锚定词，正文中出现时指向此处定义：

- **合规闸**: 逐句台词对照合规表，全部落在合规范畴内才放行
- **降级链**: 台词超载时按优先级处理：压缩 → 拆条 → 延长
- **特征污染**: 提示词重复描述参考图已锁死的维度，文本特征和图片特征抢权重
- **单场域**: 全部动作在固定空间、角色不移动位置 → 触发一镜到底模式
- **抽卡流**: 720p 直出3次 → 选最佳 → Topaz 放大到 4K

## Workflow

1. **Collect context** — Ask user for: product name, core selling points, target audience, any style preferences. If user already provided these, skip.
   - Complete when: product name、core selling points、target audience 均已明确；或用户明确说「用默认值」
2. **Apply audit rules** — Read `references/audit-full-catalog.md` before writing. 通过**合规闸**：每句台词逐条对照通用规则 + 对应品类专项规则。
   - Complete when: 每句台词已对照通用规则 + 品类专项规则，全部落在合规范畴内
3. **Write scripts** — 默认2-3条；用户指定数量则按指定数（最小1条）。每条剧本产出三件套：分镜表 + 即梦提示词 + 字幕文本.
   - Complete when: N条剧本已产出；每条含完整三件套；字数公式检查通过（台词字数 ÷ 4.5 ≤ 可用时长）
4. **Number scripts** — Format: `YYYYMMDD-NN` (e.g. 20260711-01). Auto-increment within the day.
   - Complete when: 每条剧本编号唯一且格式正确
5. **Post** — Ask if user wants to add to the script library web page.
   - Complete when: 用户已回复是否入库

## Audit Compliance (Mandatory)

> 完整品类专项清单在 `references/audit-full-catalog.md`。写台词前先 Read 该文件。本段为核心速查。

### 通用规则（适用所有品类）

| 类目 | 错误表述 | 正确表述 |
| --- | --- | --- |
| 功效宣称 | 直接宣称治愈/治疗/解决某问题 | 陪伴、全面营养、好口感等生活方式表述 |
| 效果对比 | 使用前后对比、量化承诺（数字/时间） | 生活场景片段，呈现自然使用状态 |
| 医疗用语 | 处方、治疗、临床、诊断等（台词中作为玩笑可以） | 日常用语，如"每天一杯""营养补充" |
| 绝对保证 | "一定能""保证""100%"等绝对化表述 | "陪伴每一步""营养支持" |
| 品牌滥用 | 用品牌名暗示必然效果 | 品牌名仅作品牌标识出现 |
| 绝对化定性 | "解决XX""根治XX""告别XX" | 改善、助力、搭配等柔性动词 |

### 品类专项规则

根据实际产品品类，叠加对应专项规则：

**儿童奶粉/营养品**:
| 错误表述 | 正确表述 |
| --- | --- |
| 直接说"长高""增高" | 陪伴成长、全面营养 |
| 身高前后对比、量身高画面 | 日常生活片段 |
| "解决挑食""治挑食" | 好喝、孩子爱喝 |

**零食/食品**: 聚焦口感、原料、食用场景
**日用品/家电**: 功能表述贴检测报告原文
**美妆/护肤**: 聚焦肤感、使用体验、妆容效果

**交付前正向核对**: 每句台词都落在"生活方式/陪伴/口感/营养支持/使用体验"范畴内即合规。

## Script Format

每个剧本输出三件套：分镜表、即梦提示词、字幕文本。

### Part 1: 分镜表

用于整体把控节奏和产品口播位。

```
### YYYYMMDD-NN《Title》

**Core contradiction**: [What makes it funny/surprising — the gap between expectation and reality]

| Time | Visual | Audio |
|------|--------|-------|
| 0-Ns | [shot description] | [dialogue/narration] |
| ... | ... | ... |
| N-15s | [ending + PRODUCT PLACEMENT] | [voiceover/tagline] |
```

### Part 2: 即梦提示词

即梦 Seedance 图生视频用的提示词。**台词直接写进提示词**，即梦会原生生成对应声音。

**分镜表 → 提示词转换约定**：
- 时间戳去掉（Shot序号保留）
- 动作只留关键动态，外貌信息交给参考图
- 台词原样保留
- 旁白改为最后一个Shot的「旁白：」标注

```
主体：[场景图中出现的角色简述]
场景：[场景简述]（上传场景图）
音乐：只生成对应音效

Shot 01 | [景别] [动作描述] [台词]
Shot 02 | [景别] [动作描述] [台词]
Shot 03 | [景别] [动作描述] [台词]
Shot 04 | [景别] [动作描述] [台词]
Shot 05 | [景别] [动作描述] [旁白]
```

**格式规则**：

- 每个 Shot 用 `|` 分隔景别、动作、台词
- 景别：中景/近景/特写/远景/低机位广角 等
- 动作只写关键动态；外貌服装交给参考图承载
- 台词直接写在动作后面，即梦原生生成声音
- 最后一个 Shot 的旁白用「旁白：」标注
- 按Shot序号描述节奏（Shot数量影响时长分配）
- 即梦阶段台词由原生声音输出；字幕单独在Part 3处理

### Part 3: 字幕文本

后期加字幕用的纯文本，逐条列出。最后一行为产品口播tagline，与分镜表末尾旁白一致。

```
- 台词一
- 台词二
- 台词三
- XX产品，口播tagline
```

### 一镜到底模式

当场景适合**全景固定机位、镜头全程固定**时（如双人对话、柜台咨询、办公室场景），使用一镜到底模式：

- 提示词开头标注 `机位：全景固定机位一镜到底，镜头全程不动`
- 每个 Shot 的景别统一写 `全景`，仅描述角色动作和台词变化
- 动作靠角色肢体驱动节奏（转头、前倾、回头看），镜头全程固定不动
- 适合台词密集、情绪反差强的场景，比多 Shot 切换更自然连贯

**判断标准**：**单场域**场景（全部动作在固定空间、角色不移动位置）→ 优先一镜到底。
**默认行为**：判断不确定时默认走多Shot模式。



## Style Guidelines

- **Tone**: Absurdist, contradictory, unexpected. 追求反常识开场。
- **Characters**: Child acting like adult (role reversal), formal language from kid, power dynamic flip.
- **Reversal patterns**: bait-and-switch, 先行后奏 (act first, ask later), form mismatch.
- **Product placement**: 产品摆在画面显眼处，自然可见。用 **\[产品口播位\]** 标注所在行
  - 饮品类：罐装+倒好的杯装并排
  - 零食类：包装+打开的状态展示
  - 家电类：产品特写+使用场景
  - 美妆类：产品+使用效果对比场景
- **Placement row**: Last \~5 seconds. Visual focuses on product on table. Audio is voiceover tagline.
- **Length**: 默认 15 秒，4-5 shots。
- **台词超载处理**: 触发**降级链** — ① 压缩台词（删冗余词、合并相近句意，信息量不丢）→ ② 拆两条（自然断点处拆为 2 条独立广告，各自 15s）→ ③ 延长直出（18-20s，牺牲部分完播率）。判断标准：台词字数 ÷ 4.5字/秒 > 15s → 触发。检查时机：写完分镜表后、出提示词前先跑一遍字数公式。

## Defaults

| 参数 | 默认值 | 可覆盖 |
|------|--------|--------|
| 时长 | 15s | 18-20s（台词超载时自动） |
| 画幅 | 9:16（抖音/快手） | 16:9（B站/视频号） |
| 分辨率 | 720p | — |
| 抽卡次数 | 3 | — |
| 剧本数 | 2-3 条 | 用户指定 |
| 景别 | 中近景（人物占画面 ~70%） | — |

## Output Contract

每条剧本交付时必须同时包含：

1. 分镜表（含 Core contradiction + 时间-画面-声音三列表格）
2. 即梦提示词（按 Shot 序号，景别+动作+台词格式）
3. 字幕文本（逐条台词 + 末行产品口播 tagline）
4. 合规确认（每条台词的合规状态标注）
5. 字数公式结果（实际字数 ÷ 4.5 vs 可用时长的比值）

## Failure Modes

- **Shallow audit**（浅审核）: Agent 扫一眼合规表就过，未逐句核对
  - 修法：Apply audit rules 步骤的 Complete when 强制每句台词核对
- **Feature pollution**（特征污染）: 提示词重复描述参考图已锁死的维度
  - 修法：5维度扫描法——先识别锁死维度，只对空白维度下刀
- **Shot homogeneity**（镜头同质化）: 多个 Shot 动作描述雷同，即梦生成一模一样画面
  - 修法：每个 Shot 必须有至少一个可区分的动作差异（点头/目光下移/前倾等微变化）
- **Overrun**（超时）: 台词量超出 15s 容量，未触发降级链
  - 修法：写完分镜表后、出提示词前强制跑字数公式
- **No-op sediment**（指令沉积）: 随时间积累的"注意XX""确保XX"类指令，模型默认就会遵守
  - 修法：逐句做 no-op 测试——删掉后行为无差异则删除

## 即梦 Seedance 生产工作流（广告版）

### 素材准备

**用户已提供参考图** → 直接使用，跳到核心原则

**用户未提供参考图** → 按以下规则生成首帧图：
- 画幅：竖屏9:16（抖音/快手），横屏16:9（B站/视频号）
- 风格：真实生活感，规避AI塑料感
- 角色比例：中近景人物占画面约70%
- 品牌名处理：用虚构代号（MN/XX牌/YY），出现真实品牌名时一律替换
- 生成张数：2张供用户选；或1张直出快速推进
- 质量门槛：评分≥8/10才可用，否则重抽

### 核心原则

| 素材 | 用途 | 内容边界 |
| --- | --- | --- |
| 首帧图/场景参考图 | 锁定画面外观 | 主体外貌、服装、场景、比例 |
| 即梦提示词 | 控制动作+镜头+台词 | 谁动了、怎么动、说什么 |

### 生成参数建议

> 参数默认值见 Defaults 段。此处补充原因说明。

| 参数 | 推荐原因 |
| --- | --- |
| 720p | 对提示词响应最好，1080p反而效果差 |
| 全能参考 | 主体+场景同时生效 |
| 2\~3s/Shot | 15s直出得5个连贯镜头 |

### 正确做法

1. **走首帧图直出** — 首帧图/场景图直接喂给模型更流畅
2. **提示词只写动作台词** — 外貌由参考图承载，认真交代动作和台词即可
3. **按Shot序号控节奏** — Shot数量决定时长分配，时间戳省略
4. **720p优先** — 720p响应更好，清晰度靠Topaz后期放大

### Gotchas

- **模糊原图 → 被误判为艺术风格**: 模糊的原图会被AI视觉编码器误判为特殊艺术风格，并在后续每帧死锁这个错误特征。先 Topaz 提升清晰度再喂模型
- **1080p 降智**: 720p 对提示词响应最好，1080p 反而效果差。清晰度靠后期 Topaz 解决
- **Shot 动作雷同**: 多个 Shot 动作描述雷同时，即梦生成一模一样画面。每个 Shot 必须有至少一个可区分的动作差异（点头/目光下移/前倾等微变化）
- **品牌名泄露**: 使用真实品牌名会触发平台审核。一律用虚构代号（MN/XX牌/YY）替换
- **模糊底图 + 文字描述 = 特征污染**: 已锁死的维度再写文字描述，文本特征和图片特征抢权重，原画细节被破坏

### 提示词科学：五维度注意力权重分配

> 完整理论与坐标系在 `references/five-dimensions.md`。处理非标准场景前先 Read 该文件。本段为广告场景下的速查版。

核心原理：AI视频从随机噪声逐步去噪生成，提示词 = 刻刀方向。描述越多刀越乱（特征污染），结构化锚点 = 精准赋值（去噪路径聚焦）。**顶级提示词不拼字数，拼谁能最精准地分配模型的交叉注意力权重。**

#### 五维度控制坐标系

| 维度 | 含义 | 参考图能否锁死 |
|------|------|----------------|
| 1. 绝对主体与物理动势 | 画面中的人物/物体及运动状态 | 有主体参考图时锁死 |
| 2. 环境场与情绪光影 | 场景、光调、氛围 | 有场景参考图时锁死 |
| 3. 光学与摄影机调度 | 镜头运动方式 | **参考图永远无法提供** |
| 4. 时间轴与状态演变 | 变身/生长/爆炸等时间变化 | 静态图 = 锁死为静止 |
| 5. 美学介质与底层渲染参数 | 画风、色调、渲染风格 | 有风格参考图时锁死 |

#### 实操心法：5秒5维度扫描法

拿到任何画面需求，5秒内用5维度扫描，识别哪些参数已被素材锁死、哪些还是空白，**只对空白维度精准下刀**：

- **做减法场景**（信息熵拉满的底图）：极致克制，只写摄影机调度指令，一个多余形容词都不加。锁死的维度再写 = 文本特征和图片特征抢权重 → 特征污染 → 原画细节被破坏
- **做加法场景**（需变身/动作/状态演变）：按维度展开，每帧物理状态都写清楚，用结构化锚点（面部/服装/场景/时间分段）精准分配注意力

#### 前置条件：素材清晰度

详见 Gotchas 段「模糊原图」条目。

#### 广告场景下的维度映射

广告生产中，首帧图/场景参考图已锁死维度1（主体外貌+动势）和维度2（环境+光影），维度4通常为静止（单场景15s），维度5由参考图风格决定。**提示词只需要补全维度3（镜头调度）+ 维度4中需要的动作变化**——这就是为什么广告提示词只写景别+动作+台词，外貌由参考图承载。

### 后期放大

定稿后用 Topaz 放大到 4K。把省出来的时间用来构思分镜和故事。

## Interaction Patterns

> 完整模式库在 `references/interaction-patterns.md`。选模式前先 Read 该文件。以下为常用模式速查。

### 角色反转类（适合儿童产品）

| Pattern | Description | Example |
| --- | --- | --- |
| Role reversal | Kid acts as authority figure, parent is helpless | Kid as "fitness coach" scolding dad's form |
| Bait-and-switch | Setup suggests concern, real motive is selfish | Kid "diagnoses" dad, actually wants the milk |
| Form mismatch | Formal/serious framing for trivial content | Business negotiation over a snack trade |
| First-move advantage | Act before getting permission | Drink the milk first, then ask about the chips |
| Fake expertise | Kid uses jargon confidently but absurdly | "Your developmental window has closed, Old Li" |

### 成人对话类（适合全品类）

| Pattern | Description | Example |
| --- | --- | --- |
| 情绪反差 | 一方急一方淡，反差越大越好笑 | 家长急追问，营养师头都不抬淡定回答 |
| 权威碾压 | 专业人士一句话堵死外行，越平静越有压迫感 | 营养师："可以啊，到时候跟不上别打他就行" |
| 冷幽默面瘫 | 用最平淡的语气说最炸的话 | 客服面无表情："这产品确实不适合您，建议您继续受着" |
| 误读升级 | 双方理解不在一个频道，越解释越离谱 | 家长问添加剂，营养师以为是问加价 |
| 反向推销 | 销售/客服拼命劝退，越劝退越想买 | "一天一条就够了，多了浪费"→ 家长反而更信 |

## Output Language

All scripts output in Chinese. Section headers can be in Chinese or English, but dialogue, descriptions, and voiceovers must be in natural spoken Chinese.