# 补录引导

多日补录模式。**逐天进行，每天逐列确认。**

所有提问必须使用 AskUserQuestion 工具。用户通过选项或 Other 输入自由文本回答。

补录模式比单日模式更轻量——合并部分列，减少提问次数。

## 流程

对每一天重复以下步骤，处理完一天再进入下一天：

### Step 1: 概括

```json
{
  "questions": [{
    "question": "{date}（{weekday}）那天工作上最值得记的是什么？",
    "header": "{date}",
    "options": [
      {"label": "让我说说...", "description": "选择后输入内容"},
      {"label": "没啥特别的", "description": "普通的一天"},
      {"label": "不太记得了", "description": "回忆不起来"}
    ],
    "multiSelect": false
  }]
}
```

用户选 "没啥特别的" 或 "不太记得了"，跳到 Step 4 直接确认。

### Step 2: 类型 + 重要度（合并一问）

```json
{
  "questions": [{
    "question": "工作类型和重要程度？",
    "header": "类型",
    "options": [
      {"label": "开发 普通", "description": "日常写代码，没什么特别的"},
      {"label": "开发 有推进", "description": "推动了项目进度"},
      {"label": "会议", "description": "主要在开会"},
      {"label": "其他", "description": "选择后说明类型和重要度"}
    ],
    "multiSelect": false
  }]
}
```

从用户选择中提取 category 和 impact。

### Step 3: 项目（可选追问）

```json
{
  "questions": [{
    "question": "哪个项目？",
    "header": "项目",
    "options": [
      {"label": "让我输入...", "description": "输入项目名称"},
      {"label": "跳过", "description": "不填"}
    ],
    "multiSelect": false
  }]
}
```

### Step 4: 确认当天

```json
{
  "questions": [{
    "question": "{date} 的记录：\n📋 {summary}\n🏗️ 项目：{project}  📂 类型：{category}  ⭐ 重要度：{impact}/5  🏷️ {tags}\n\n确认没问题就写入。",
    "header": "确认",
    "options": [
      {"label": "确认写入", "description": "记录没问题"},
      {"label": "要改一下", "description": "选择后说明要改什么"}
    ],
    "multiSelect": false
  }]
}
```

用户确认后，立即调用 csv_manager 写入 CSV，然后进入下一天的 Step 1。

## 规则

- 所有提问用 AskUserQuestion
- 一天一天来，不批量问多天
- 补录模式精简提问：合并类型+重要度，跳过心情和产出
- tags 自动推断
- "没啥特别的" 也要记录
