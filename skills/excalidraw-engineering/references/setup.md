# 安装与首次验收

核对日期：2026-10-14。本文区分技能安装、编辑器使用和自动导出。手动操作步骤供用户执行；agent 只有获得安装授权后才修改环境。

## 1. 安装 Skill

1. 解压技能包，找到含有 `SKILL.md` 的 `excalidraw-engineering` 文件夹。
2. 将整个文件夹放到当前工具支持的技能目录。Codex 使用其配置的用户技能目录，通常为 `$CODEX_HOME/skills`，未设置时为 `~/.codex/skills`；其他工具使用其实际配置，不能照抄 Codex 路径。
3. 如果已有同名技能，先比较版本并备份，再更新；不要把不相关文件覆盖掉。
   已存在的 `preferences.local.md` 是个人偏好，更新时保留；共享技能包不应携带作者的个人偏好文件。
4. 按宿主方式重新加载技能或开启新聊天，要求 agent 读取并确认该技能。发现列表有名称不代表正文和参考文件均已读取。

安装后的层级应为：

```text
<技能目录>/excalidraw-engineering/SKILL.md
<技能目录>/excalidraw-engineering/references/setup.md
```

不要多嵌套一层同名目录。技能位置存放规则，业务图稿另放独立成果目录。

## 2. 选择编辑和导出方式

### A. 先手动试用：网页版

1. 在浏览器打开 [Excalidraw](https://excalidraw.com)。无需先安装桌面软件。
2. 让 agent 按本技能生成 `.excalidraw` 源文件。
3. 使用编辑器的打开/导入功能读取该文件，检查文字和连线，尝试选择、移动一个节点，确认它不是整张图片。
4. 使用导出图片功能保存 PNG 或 SVG，放回源文件所在的独立成果目录。
5. 将实际预览交给 agent 检查，或由用户完成目视检查并告知结果。菜单名称以当前界面为准。

Excalidraw 提供网页应用及本地文件/图片导出能力，参见 [官方项目](https://github.com/excalidraw/excalidraw)。涉及内部资料时遵守公司对工具的要求；已有公司部署版优先使用。仅访问网页版并不会自动赋予 agent 操作浏览器的权限。

### B. 推荐给 agent：官方 CLI 本地导出

官方 CLI 的本地 PNG 渲染无需账号或 API Key。当前要求 Node.js 22+；使用已安装的 Chrome、Edge、Chromium 或 Firefox，不代为下载浏览器。依据：[官方 CLI 安装与渲染说明](https://github.com/excalidraw/cli)。

**第一步：检查前置环境。**

```bash
node --version
npm --version
```

确认 Node.js 满足要求，电脑已有上述浏览器之一。缺少运行时可按 [Node.js 官方下载入口](https://nodejs.org/en/download) 安装受支持版本；公司有统一环境管理方式时遵循它。agent 不能为了绘图擅自升级业务项目运行时、改全局默认版本或切换包管理器。

**第二步：安装 CLI。**

用户自行安装，或向 agent 明确授权后执行：

```bash
npm install -g @excalidraw/cli
excalidraw --help
excalidraw render --help
```

如果不想长期全局安装，可以使用：

```bash
npx @excalidraw/cli --help
```

`npx` 仍可能下载包并写入缓存，不等于无安装、无网络活动。团队追求复现时固定经过验证的包版本，并记录实际版本。若全局目录权限不足，不默认使用 `sudo`；使用用户级环境或独立工具目录，避免把依赖装进业务项目。

**第三步：实际导出。**

先由 agent 在独立成果目录生成有效的 `01-flow-v01.excalidraw`，再进入该目录执行；下面的源文件必须真实存在：

```bash
excalidraw render "01-flow-v01.excalidraw" --out "01-flow-v01.png"
```

选择临时使用方式时，对应命令为：

```bash
npx @excalidraw/cli render "01-flow-v01.excalidraw" --out "01-flow-v01.png"
```

本地命令使用 `render`。不要为了导出本地文件执行 `login`、`scenes create` 等云端操作。CLI 用于自动 PNG 预览；需要手动编辑或 SVG 时使用编辑器或其他已验证工具。

**第四步：处理浏览器和字体。**

自动查找浏览器失败时，可用 `--browser-path` 指定实际可执行文件路径；先根据系统定位，不硬编码作者电脑路径。中文手绘字体可能首次下载并缓存；严格禁用字体下载时可用 `--fonts-url none`，然后检查系统字体回退是否可读。上述参数与行为见 [CLI 渲染说明](https://github.com/excalidraw/cli#render-to-png)。

不要把“本地渲染”写成“全过程绝无网络请求”。字体、首次包下载及版本检查等需按实际配置判断。

### C. 公司需要本地编辑器时

已有公司自建 Excalidraw 直接使用。需要新增本地服务时，参考 [官方本地开发与部署说明](https://github.com/excalidraw/excalidraw/blob/master/dev-docs/docs/introduction/development.mdx)，由 agent 按公司环境选择部署方式。不要仅为本地 PNG 导出默认搭建完整编辑器、协作服务或购买订阅。

## 3. 首次验收：安装成功不等于能画图

要求 agent 实际执行一个不含业务资料的小任务：

```text
请使用 excalidraw-engineering 按 UML 活动图常用子集生成中文流程图：接收请求→校验参数→成功/失败。标明判断出口条件和终点含义，并按 references/review-checklist.md 在成果 README 记录语义及视觉检查结果。
判断节点有明确分支标签，成功与失败各有终态。
在 diagrams/<实际日期>-setup-check/ 中保存 .excalidraw、PNG 和 README。
实际运行导出并查看预览，记录使用的工具和版本，以及真正完成的检查。
```

验收内容：

- agent 能读取本技能及引用文件。
- 源文件能被实际编辑器打开或被渲染器处理；打开编辑器时抽查节点、文字和箭头可编辑。
- PNG 实际生成，中文清楚，判断出口和终态正确，没有裁切与明显重叠。
- `.excalidraw`、PNG、README 同处独立成果目录，源图与预览版本一致。
- 记录工具版本、实际运行命令与尚未验证内容；没有查看预览就标为待目视检查。

若宿主只能读写文件、不能执行命令或看图，可以先交付源文件，再由用户用网页版导出和确认。不能把这种环境描述为自动导出已配置完成。

## 4. 常见问题

| 问题 | 处理 |
| --- | --- |
| Skill 已复制但没有被发现 | 检查层级和宿主实际技能目录，重新加载；不要反复安装 CLI |
| Node.js 版本太低 | 优先隔离运行时，避免影响当前业务项目 |
| npm 下载失败或目录无权限 | 检查公司网络/包源与用户级目录，不盲目提权或换未知包源 |
| 找不到浏览器 | 检查已安装浏览器和真实路径；缺失时说明需安装 |
| 中文缺字或手绘字体不可用 | 检查字体/下载条件或系统回退，再实际查看结果 |
| 导出失败 | 保留源文件和具体错误，按格式、绑定、图片资源、浏览器逐项定位 |
| 导出成功但图很小或难读 | 按工具参数调整尺寸或拆图，再查看；不要仅因退出码为零就验收 |

## 5. 给自己 agent 的安装指令

可直接复制 [README 中的完整安装指令](../README.md#先准备运行环境)，连同技能压缩包一起发送。它包含授权范围、检查步骤、中文示例验收和后续使用要求，无需用户逐条解释命令。
