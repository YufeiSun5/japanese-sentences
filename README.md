# 🇯🇵 日语句子学习系统 - 项目企划

> 每天一句，积少成多。以句子为核心单元，关联单词库，借助 AI 实现智能复习。

---

## 🎯 项目目标

打造一个个人日语学习工具，核心理念：
- **以句子为主**，不死记单词，在语境中记忆
- **单词库自动积累**，幂等校验，不重复添加
- **AI 辅助**，接入 DeepSeek 分析句子、生成新句子
- **发音支持**，后期接入 TTS 接口朗读日语句子

---

## 📁 目录结构

```
japanese-sentences/
├── README.md                  # 项目企划（本文件）
├── sentences/                 # 每日一句
│   └── YYYY-MM-DD.json        # 每天一个文件
└── words/
    └── vocabulary.json        # 全局单词库（幂等）
```

---

## 📐 数据结构设计

### 句子 sentences/YYYY-MM-DD.json

```json
{
  "date": "YYYY-MM-DD",
  "sentences": [
    {
      "id": "s_YYYYMMDD_001",
      "japanese": "日文原句",
      "reading": "平假名读音",
      "chinese": "中文翻译",
      "english": "English translation",
      "source": "manual 或 ai",
      "scene": "使用场景描述",
      "jlpt_target": "N3",
      "ai_generated": false,
      "word_ids": ["w_単語1", "w_単語2"],
      "notes": "备注"
    }
  ]
}
```

### 单词库 words/vocabulary.json

```json
{
  "meta": {
    "total": 0,
    "last_updated": "YYYY-MM-DD"
  },
  "words": [
    {
      "id": "w_単語",
      "kanji": "単語",
      "hiragana": "たんご",
      "meaning": "Word",
      "meaning_zh": "单词",
      "jp_chunk": "日语语块（含平假名标注）",
      "jp_chunk_zh": "语块中文意思",
      "en_chunk": "English chunk",
      "en_chunk_zh": "英语语块中文意思",
      "dev_scene": "关联编程/工业/生活场景",
      "note": "学习备注",
      "frequency": 5,
      "priority": "Must Know",
      "jlpt": "N3",
      "learned_date": "YYYY-MM-DD",
      "frequency_in_my_study": 1,
      "sentence_ids": ["s_YYYYMMDD_001"]
    }
  ]
}
```

---

## 🔒 幂等校验规则

单词唯一键：`id = "w_" + kanji`

```
输入句子
  → DeepSeek 分析，返回单词列表
  → 用户勾选要保存的单词
  → 对每个单词：
      IF w_单词 已存在
        → 仅追加 sentence_id，frequency_in_my_study + 1
      ELSE
        → 创建新条目，learned_date = 今天
```

---

## 🚀 开发路线

### 阶段 1（当前）：纯 JSON 手动记录
- [x] 设计数据结构
- [ ] 每天手动新建 sentences/YYYY-MM-DD.json
- [ ] 手动维护 words/vocabulary.json

### 阶段 2：脚本自动化
- [ ] 输入句子 → 调用 DeepSeek API 分析单词
- [ ] 自动生成 JSON，幂等写入单词库
- [ ] 支持 AI 按场景生成新句子

### 阶段 3：微信小程序
- [ ] 界面化展示句子 + 单词
- [ ] 接入 TTS（腾讯云 / Azure ja-JP-NanamiNeural）实现朗读
- [ ] 复习功能（按 JLPT 等级筛选、词频排行）
- [ ] 个人词库统计面板

---

## 🔊 TTS 发音方案（阶段3）

| 方案 | 费用 | 音质 | 推荐 |
|------|------|------|------|
| 腾讯云 TTS | 免费500次/月 | ⭐⭐⭐⭐ | 小程序首选 |
| Azure NanamiNeural | 免费50万字/月 | ⭐⭐⭐⭐⭐ | 音质最佳 |
| 平假名展示 | 完全免费 | 无声音 | 阶段1过渡 |

---

## 💡 技术栈规划

- **存储**：JSON 文件 → GitHub 仓库
- **AI**：DeepSeek API（句子分析 + 生成）
- **前端**：微信小程序
- **TTS**：腾讯云 或 Azure TTS
- **后端**：轻量 Python/Node.js 脚本（阶段2）
