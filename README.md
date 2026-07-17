---
AIGC:
  ContentProducer: '001191110102MAD55U9H0F10002'
  ContentPropagator: '001191110102MAD55U9H0F10002'
  Label: '1'
  ProduceID: '6be76805-5287-4131-a639-104930cf33aa'
  PropagateID: '6be76805-5287-4131-a639-104930cf33aa'
  ReservedCode1: '7adcae18-d1a3-4592-bc82-e42c8c87a02b'
  ReservedCode2: '7adcae18-d1a3-4592-bc82-e42c8c87a02b'
---

# ai-short-ad

AI短视频广告制作技能，面向抖音/快手等中国社交媒体平台。

## 功能

- 15-20s 走量卖货型短视频广告剧本
- 合规审核（功效宣称、医疗用语等自动拦截）
- 即梦 Seedance 提示词生成（720p 抽卡 + Topaz 放大工作流）
- 五维度注意力权重分配
- 字数公式自动检测超载并触发降级链

## 技能结构

```
ai-short-ad/
├── SKILL.md                          # 主技能文件
└── references/
    ├── audit-full-catalog.md         # 完整品类合规清单
    ├── five-dimensions.md            # 五维度注意力权重分配理论
    └── interaction-patterns.md       # 交互模式库
```

## 安装

```bash
# 克隆到你的 AI agent 的 skills 目录
git clone https://github.com/baiye12/ai-short-ad.git <agent-skills-dir>/ai-short-ad
```

## 版本

当前版本：**v1.0.0**（见 SKILL.md 头部 `version` 字段 + git tag）

## 更新

```bash
cd <agent-skills-dir>/ai-short-ad
git pull github master
```

## License

MIT

> AI生成