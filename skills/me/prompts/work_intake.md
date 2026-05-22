# 工作日志采录引导

通过 AskUserQuestion 逐项采集工作日志的每一列。一次问一列，用户可以跳过。

**所有提问必须使用 AskUserQuestion 工具。**

## Step 1: 概括

```json
{
  "questions": [{
    "question": "来，聊聊 {date} 的工作。一句话概括就行。",
    "header": "概括",
    "options": [
      {"label": "今天就摸鱼了", "description": "没什么实质性工作"}
    ],
    "multiSelect": false
  }]
}
```

提取用户回答作为 `summary`。

## Step 2: 项目

```json
{
  "questions": [{
    "question": "哪个项目？",
    "header": "项目",
    "options": [
      {"label": "跳过", "description": "不填这个"}
    ],
    "multiSelect": false
  }]
}
```

## Step 3: 类型

```json
{
  "questions": [{
    "question": "工作类型是？",
    "header": "类型",
    "options": [
      {"label": "开发", "description": "写代码、改bug、做功能"},
      {"label": "会议", "description": "开会、讨论、评审会"},
      {"label": "调研", "description": "调研方案、看文档、学习"},
      {"label": "其他", "description": "评审/规划/运维/沟通/文档"}
    ],
    "multiSelect": false
  }]
}
```

用户选"其他"时通过 Other 输入具体类型。

## Step 4: 重要度

```json
{
  "questions": [{
    "question": "今天工作的重要程度？",
    "header": "重要度",
    "options": [
      {"label": "1-2 普通", "description": "日常事务，没什么特别的"},
      {"label": "3 有推进", "description": "推进了项目进度"},
      {"label": "4-5 关键", "description": "完成里程碑、解决关键问题"}
    ],
    "multiSelect": false
  }]
}
```

## Step 5: 关键产出

```json
{
  "questions": [{
    "question": "有没有具体的产出或成果？",
    "header": "产出",
    "options": [
      {"label": "没有", "description": "跳过"}
    ],
    "multiSelect": false
  }]
}
```

## Step 6: 心情

```json
{
  "questions": [{
    "question": "今天工作的心情怎么样？",
    "header": "心情",
    "options": [
      {"label": "不错", "description": "开心、有成就感"},
      {"label": "一般", "description": "平平淡淡"},
      {"label": "有点累", "description": "疲惫、烦躁、压力大"},
      {"label": "跳过", "description": "不想填"}
    ],
    "multiSelect": false
  }]
}
```

## Step 7: 确认写入

汇总所有已填的列，展示给用户确认：

```json
{
  "questions": [{
    "question": "整理好了，确认写入？\n\n📋 {summary}\n🏗️ 项目：{project}\n📂 类型：{category}\n⭐ 重要度：{impact}/5\n🎯 产出：{key_result}\n💼 心情：{mood}\n🏷️ 标签：{tags}",
    "header": "确认",
    "options": [
      {"label": "确认写入", "description": "没问题"},
      {"label": "要改一下", "description": "选择后说明要改什么"}
    ],
    "multiSelect": false
  }]
}
```

## 规则

- tags 从对话内容自动提取，不单独问
- 用户选"跳过"的列留空
- 所有提问用 AskUserQuestion
