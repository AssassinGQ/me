# 生活日志采录引导

通过 AskUserQuestion 逐项采集生活日志的每一列。一次问一列，用户可以跳过。

**所有提问必须使用 AskUserQuestion 工具。**

## Step 1: 概括

```json
{
  "questions": [{
    "question": "嘿，{date} 过得怎么样？随便说两句就行。",
    "header": "概括",
    "options": [
      {"label": "没啥特别的", "description": "普通的一天"},
      {"label": "今天有点丧", "description": "心情不太好"}
    ],
    "multiSelect": false
  }]
}
```

提取用户回答作为 `summary`。

## Step 2: 心情

```json
{
  "questions": [{
    "question": "心情怎么样？",
    "header": "心情",
    "options": [
      {"label": "开心", "description": "心情很好"},
      {"label": "一般", "description": "平平淡淡"},
      {"label": "有点丧", "description": "心情不太好"},
      {"label": "跳过", "description": "不想填"}
    ],
    "multiSelect": false
  }]
}
```

## Step 3: 亮点

```json
{
  "questions": [{
    "question": "今天有没有什么亮点？",
    "header": "亮点",
    "options": [
      {"label": "没有", "description": "跳过"}
    ],
    "multiSelect": false
  }]
}
```

## Step 4: 低谷

```json
{
  "questions": [{
    "question": "有没有什么不开心的事？",
    "header": "低谷",
    "options": [
      {"label": "没有", "description": "跳过"}
    ],
    "multiSelect": false
  }]
}
```

## Step 5: 一起的人

```json
{
  "questions": [{
    "question": "今天和谁一起？",
    "header": "人物",
    "options": [
      {"label": "一个人", "description": "独处"},
      {"label": "跳过", "description": "不填"}
    ],
    "multiSelect": false
  }]
}
```

## Step 6: 地点

```json
{
  "questions": [{
    "question": "今天主要在哪？",
    "header": "地点",
    "options": [
      {"label": "在家", "description": "宅家"},
      {"label": "公司", "description": "在公司"},
      {"label": "跳过", "description": "不填"}
    ],
    "multiSelect": false
  }]
}
```

## Step 7: 确认写入

汇总所有已填的列：

```json
{
  "questions": [{
    "question": "帮你记下来了，确认写入？\n\n📝 {summary}\n😊 心情：{mood}\n⭐ 亮点：{high_point}\n😔 低谷：{low_point}\n👥 一起的人：{people}\n📍 在哪：{location}\n🏷️ 标签：{tags}",
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
