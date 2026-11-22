# 把性能吐出来！

[English](README.md) · 简体中文

移动 Web、PWA 和 WebView 性能排查 Skill。

原作者：**niannian** · Skill 标识：`web-performance-audit`

原作来源：[niannian0125/web-performance-audit](https://github.com/niannian0125/web-performance-audit)

自定义许可：允许个人非商业使用、分享和署名修改后再发布，禁止内嵌第三方平台。详见[使用许可](#使用许可)。

聊天页第一次打开很快，来回切换几次却越来越卡？批量导入照片时，明明每张都不大，网页还是闪退？把这个 Skill 交给能读取项目代码的编程助手，让它沿着出问题的那次操作找原因。

这份 Skill 来自开发网页“小手机”时总结的八条经验，适用于移动 Web、PWA 和 WebView。它检查图片和音频存储、图片压缩、数据库读取、上传入口、聊天渲染、懒加载与并发、毛玻璃与动效，以及页面退出后还在跑的幽灵任务。

## 放进项目

下载源码后，把 `web-performance-audit` 文件夹整个复制到你要检查的项目里。中英文共用这一个入口，直接用中文提问就会收到中文报告。完整规则也有[中文阅读版](web-performance-audit/SKILL.zh-CN.md)。

| 你使用的工具 | 复制后的入口文件 |
| --- | --- |
| Codex | `.agents/skills/web-performance-audit/SKILL.md` |
| Claude Code | `.claude/skills/web-performance-audit/SKILL.md` |

在该项目里打开编程助手，直接说出你遇到的问题。Codex 可用 `$web-performance-audit` 点名，Claude Code 可用 `/web-performance-audit`。使用其他能读项目文件的助手时，也可以直接让它读取 [SKILL.md](web-performance-audit/SKILL.md)，并给出相关代码路径。

## 试着这样问

> 用 web-performance-audit 给这个项目做一次小手机性能体检，先别改代码。告诉我最值得先修哪一处，指出代码位置、触发条件和复测方法。

> 来回切换聊天后越来越卡。帮我查旧页面的轮询有没有停，再看每次刷新为了显示最近 20 条消息，实际从数据库读了多少条。

> 帮我修批量导图的内存峰值。保留用户选图顺序和保存原图的选项，用同一批测试图片比较修复前后同时存活的位图数。

## 怎么看结果

一条可操作的发现应该讲清楚是哪次操作、哪段代码、做了多少工作。例如，页面退出时清除了计时器，但退出前已开始的查询返回后，又排了下一次轮询。可以让测试查询故意慢一点，在等待时切走，观察旧页面会不会继续发请求。

Skill 会区分源码能确认的行为、运行中复现的现象和还需要真机测量的判断。只要求体检时交付报告；明确要求修复时才按范围改代码并复测。可用的[复测场景](web-performance-audit/references/scenarios.zh-CN.md)包括大聊天记录、批量导图、缓存清理、页面切换和动效掉帧。

安装路径参考 [Codex 官方文档](https://developers.openai.com/codex/skills/) 与 [Claude Code 官方文档](https://code.claude.com/docs/en/skills)。

## 使用许可

Copyright © 2026 niannian。采用自定义[《把性能吐出来！个人非商业使用与再发布许可 1.0》](LICENSE)。Skill 文件夹内也附有完整许可，单独安装或转载时请保留。

允许自然人出于个人非商业目的使用、复制、分享、修改和重新发布。转载原版或修改版时，须保留原作者 **niannian**、原作名称 **「把性能吐出来！」**、随原作提供的来源链接和完整许可；修改版须说明改动，清楚区分原作和修改版，也可以加上修改者自己的署名。

禁止商业使用，也禁止将原版或修改版内置、预装、打包集成，或托管为第三方平台的一项功能或内置资源，无论平台免费还是收费。个人安装到 Codex、Claude Code 等工具供自己使用是允许的；以独立源码仓库或文件发布页进行非商业署名转载也是允许的。只转发来源链接同样允许。

修改版署名示例：

> 原作：《把性能吐出来！》（`web-performance-audit`）
>
> 原作者：niannian
>
> 许可：《把性能吐出来！个人非商业使用与再发布许可 1.0》，见随附 LICENSE。
>
> 修改说明：调整了图片导入场景的复测步骤。

如取得的版本带有上游来源链接，再发布时须一并保留。这是自定义限制性许可，不是 MIT、Apache 或标准 CC 协议。具体权利与限制以完整 [LICENSE](LICENSE) 为准；中英文有歧义时以中文文本为准。
