# Styles Library

## 放在了哪里

- 统一可用入口在 `styles/library/`
- 合并后的 25 个可用风格清单在 `styles/library/manifest.json`
- 本地原始版本保存在 `styles/local/`
- DrawPPT 来源档案保存在 `styles/drawppt/`
- 22 张 DrawPPT 预览图可本地保存在 `styles/drawppt/previews/`，不随代码提交
- DrawPPT 原始来源清单和去重结果保存在 `styles/drawppt/manifest.json`

## 下载来源

- 来源页面：`https://www.drawppt.com/zh-CN/templates`
- 抓取日期：`2026-03-18`

## 分类结果

- `styles/library/business/`：3 个风格
- `styles/library/education/`：7 个风格
- `styles/library/creative/`：6 个风格
- `styles/library/tech/`：3 个风格
- `styles/library/editorial/`：2 个风格
- `styles/library/fun/`：4 个风格

说明：
- `styles/library/` 一共收纳了 25 个去重后的可用风格
- 其中 20 个来自 DrawPPT，5 个来自本地已有整理
- DrawPPT 原始来源仍然保留 22 条记录；预览图作为本地可选素材，不纳入代码提交
- DrawPPT 里重复的 2 个风格没有进入统一入口，而是映射到了更好的本地版本

## 重复评审

1. `claude-style` 对应本地的 `styles/local/C-warm-academic-humanism.md`
本地版本更好。原因是它保留了同一组核心视觉约束，同时额外提供了页面类型模板、技术参数、中文说明和来源注释，复用时更稳定。

2. `claymation` 对应本地的 `styles/local/D-clay-mimicry.md`
本地版本更好。原因是它对材质、光照、构图、页面模板和使用场景的定义明显更完整，而 DrawPPT 原版只是一个较短的风格提示词。

## 使用建议

- 想直接挑风格复用：优先从 `styles/library/<category>/` 里选
- 想看合并后的完整清单：先看 `styles/library/manifest.json`
- 想追溯 DrawPPT 原始来源：看 `styles/drawppt/manifest.json`
- 想看视觉参考：可在本地放置对应预览图到 `styles/drawppt/previews/`
