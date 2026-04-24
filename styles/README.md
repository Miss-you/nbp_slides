# Styles Library

## 可用风格

`styles/library/` 下共 25 个可用风格，按 6 个分类组织：

- `business/`：3 个
- `education/`：7 个
- `creative/`：6 个
- `tech/`：3 个
- `editorial/`：2 个
- `fun/`：4 个

完整清单见 `styles/library/manifest.json`。每个风格文件包含风格描述、基础提示词模板、页面类型模板和使用示例。

## 参考来源

本项目部分风格模板的灵感参考了互联网公开渠道，并在此基础上进行了二次整理、扩展和标准化。各风格的具体来源如下：

- `clay-mimicry`（黏土拟物风格）：参考优设网 NotebookLM + Nano Banana Pro 实践案例及 Claymorphism UI 设计趋势
- `ink-wash-wuxia`（水墨武侠黑板风格）：用户自定义，融合中国水墨画传统与课堂黑板质感
- `warm-academic-humanism`（暖色学术人文风格）：参考 awesome-nanobanana-prompts PPT 示例（Humanities PPT），Anthropic/Claude 式设计语言
- 其余 22 个风格：参考互联网公开渠道整理而成

## 重复评审记录

1. `claude-style` → 合并为 `warm-academic-humanism`（本地版本更完整）
2. `claymation` → 合并为 `clay-mimicry`（本地版本更完整）

## 使用建议

- 想直接挑风格复用：从 `styles/library/<category>/<style-id>.md` 读取完整定义
- 想看完整清单：先看 `styles/library/manifest.json`
