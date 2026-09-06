# my-knowledge-cards

一个供 AI 助手加载的纯指令 Skill，将粘贴文章或本地 Markdown/TXT 文件整理为便于理解和复习的知识卡片。

## 解决什么问题

文章内容较长、知识点分散或重复时，手动整理复习材料需要反复筛选和改写。本 Skill 指导助手提取与主旨相关的重要知识，整理成每张只讲一个知识点、附带自测问题的卡片，减少重复整理的工作。

## 主要功能

- **支持两种输入**：粘贴的文章正文，或本地 `.md`、`.markdown`、`.txt` 文件。
- **按重要性提炼并去重**：通常生成 5–8 张；信息不足时少于 5 张，没有可提炼知识时输出 0 张并说明原因。
- **统一卡片结构**：标题、核心知识、简明解释、例子、自测问题。
- **忠于原文**：要求保留关键条件、不确定性和作者观点归属，不添加外部知识；原文没有例子时写“原文未提供例子”。
- **输出 Markdown**：默认使用用户的语言，在对话中输出；明确要求保存时才写入指定位置，不覆盖原文。

当前不支持网页抓取、PDF、Anki 或图形界面。项目不包含独立转换程序、命令行工具或额外运行脚本；内容由加载 Skill 的 AI 助手生成，仍需核对是否准确。

## 安装方法

需要能够加载 Skill 的 AI 助手；处理本地文件时，助手还需具有相应文件的读取权限。日常使用本 Skill 无需安装 Python 或 Node.js 依赖。

以 Codex 本地安装为例：

1. 将本项目下载或复制到本地。
2. 将整个 `skills/my-knowledge-cards` 文件夹复制到以下一个位置：当前工作目录的 `.agents/skills/`（项目内使用），或用户主目录的 `.agents/skills/`（个人跨项目使用）。
3. 确认目标结构如下，避免多套一层目录：

```text
.agents/skills/my-knowledge-cards/
├── SKILL.md
└── agents/
    └── openai.yaml
```

Codex 会自动检测 Skill 变化；若未出现，重启 Codex。安装目录与加载行为依据 [OpenAI 官方 Skill 文档](https://learn.chatgpt.com/docs/build-skills)。其他助手请遵循其 Skill 安装说明。

本项目的 `skills/` 是源码存放位置，不等同于自动发现目录。暂不安装时，也可以在当前项目中明确要求助手读取入口：

```text
请读取 skills/my-knowledge-cards/SKILL.md，并按其中的规则把以下正文转换成知识卡片：
（粘贴文章正文）
```

## 使用方法

安装并加载后，在对话中通过 `$my-knowledge-cards` 指定 Skill。

**粘贴正文：**

```text
请使用 $my-knowledge-cards 将以下文章整理为知识卡片：
（粘贴文章正文）
```

**读取本地文件：** 在本项目根目录打开任务后，可直接使用随附测试文章：

```text
请使用 $my-knowledge-cards 读取 tests/knowledge-cards/procedure.md，生成知识卡片。
```

自己的文件可以使用相对于当前工作目录的路径，或完整的绝对路径。文件无法读取、乱码或内容不完整时，助手应说明问题，不猜测缺失内容。

如需保存，可在请求中补充“请保存到 knowledge-cards.md”。默认只在对话中展示结果。

## 输入输出示例

下面的短文仅包含一个独立知识点，因此只生成一张卡片，展示“信息不足时不凑数量”的行为。

**输入：**

```text
请使用 $my-knowledge-cards 把以下文章整理为知识卡片：
复习时先尝试回忆，再查看笔记。例如先默写定义，再对照笔记检查。
```

**输出：**

```markdown
来源：粘贴正文，共 1 张卡片；原文只有一个独立知识点，不凑数量。

### 卡片 1｜先回忆再查看笔记
- **核心知识**：复习时先尝试回忆，再查看笔记。
- **简明解释**：先回忆内容，然后对照笔记检查。
- **例子**：先默写定义，再对照笔记检查。
- **自测问题**：按照原文，默写定义和查看笔记的顺序是什么？
```

更多完整示例见 [本地测试结果](tests/knowledge-cards/results.md)：操作说明生成 6 张、研究摘要生成 3 张、重复观点短文生成 2 张。具体措辞可能随模型变化。

## 测试与验证

本地测试覆盖操作说明、研究摘要、重复观点短文，以及无知识正文和网址输入，详见 [`tests/knowledge-cards/results.md`](tests/knowledge-cards/results.md)。这些是助手执行并复核的行为样例，不是自动化模型评测。

本 Skill 使用官方 `skill-creator/scripts/init_skill.py` 初始化。安装官方 skill-creator 后，可运行其结构验证器（实际文件名为 `quick_validate.py`）：

```text
python <skill-creator目录>/scripts/quick_validate.py skills/my-knowledge-cards
```

中文环境若验证器默认使用非 UTF-8 编码，可设置 `PYTHONUTF8=1` 后重试。结构检查不保证每次生成内容的质量。

## 许可

本项目原创 Skill、文档和测试材料以 [MIT License](LICENSE) 发布。使用者提供的文章不属于此许可范围。
