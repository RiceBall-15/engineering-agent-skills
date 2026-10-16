# Excalidraw 工程绘图 · 分享示例

日期：2026-10-15；图稿版本：v01；配套技能：excalidraw-engineering v1.3。

供技能介绍、技术分享和试用参考。架构与时序示例取自 Google 的开源 Online Boutique；CI 为原创通用方案。未使用任何用户当前项目的架构或业务信息。所有图为重新绘制的可编辑示例，不是官方原图，也不表示维护者背书。

直接打开 [示例展厅](index.html) 浏览。文件夹整体移动后仍可用；PNG 可直接放入帖子或幻灯片，SVG 适合缩放，`.excalidraw` 可用编辑器打开修改。**请保留图上的范围说明，并在介绍开源项目示例时保留来源链接。** SVG 内嵌字体的版权与许可见 [第三方资源说明](THIRD-PARTY-NOTICES.md)，整体分享时一并保留。

## 文件

| 图号 | 主题 | 可编辑源文件 | 实际预览 |
| --- | --- | --- | --- |
| 00 | 三种风格对照 | [源文件](00-style-comparison-v01.excalidraw) | [PNG](00-style-comparison-v01.png) · [SVG](00-style-comparison-v01.svg) |
| 01 | Online Boutique · 购物车架构 | [源文件](01-online-boutique-c4-v01.excalidraw) | [PNG](01-online-boutique-c4-v01.png) · [SVG](01-online-boutique-c4-v01.svg) |
| 02 | 加入购物车 · 成功路径 | [源文件](02-add-to-cart-sequence-v01.excalidraw) | [PNG](02-add-to-cart-sequence-v01.png) · [SVG](02-add-to-cart-sequence-v01.svg) |
| 03 | CI 检查 · 并行与条件 | [源文件](03-ci-activity-v01.excalidraw) | [PNG](03-ci-activity-v01.png) · [SVG](03-ci-activity-v01.svg) |

## 图种、风格与范围

- 00：自定义风格样张，保持内容和关系一致，只比较 roughness、填充和字体。不是 C4 架构图。
- 01：C4 容器图 C2 局部视图。主体为 Online Boutique 软件系统，展示 frontend、productcatalogservice、cartservice 及 Redis 配置下的存储关系。略去结算、广告、推荐、币种转换及部署拓扑，不表示完整系统或某个生产环境。
- 02：UML 2.5.1 时序图常用子集，展示参数有效、查询成功、存储可用时的一次加购。失败状态在图下注明；未展开失败分支、内部缓存实现细节及重定向后加载页面。消息参数为可读的业务字段摘要，不是语言级函数签名。
- 03：UML 2.5.1 活动图常用子集，原创 CI 方案。假设静态检查和测试都能产出检查结果，包括失败/超时；join 等待两项检查完成，再决定保存产物或报告失败。构建阶段本身的异常处理不在图示范围，也不表示任何现有仓库的实际 CI 配置。

除 00 中的对照列，全部采用轻手绘：roughness=1、浅色 hachure 填充。中文使用已加载的 Xiaolai 回退字体，英文使用 Excalifont；00 的简洁列使用 Helvetica/系统中文回退。细小 UML 符号、时序箭头和生命线采用规整笔触以保留辨识度。字体实际加载和中文显示已检查。

## 关键关系与来源

项目：[GoogleCloudPlatform/microservices-demo](https://github.com/GoogleCloudPlatform/microservices-demo)，在线核对日期 2026-10-15，来源提交 `38e7348eb289eb5b87c0c6e8cb19ced0449dc389`。仅阅读公开文档与源码，未运行或部署该项目。

| 关系/行为 | 依据 |
| --- | --- |
| 应用名称、职责和实现语言 | [官方项目说明](https://github.com/GoogleCloudPlatform/microservices-demo/blob/38e7348eb289eb5b87c0c6e8cb19ced0449dc389/README.md) |
| frontend 校验参数、查询商品、加购、返回 302 | [handlers.go / addToCartHandler](https://github.com/GoogleCloudPlatform/microservices-demo/blob/38e7348eb289eb5b87c0c6e8cb19ced0449dc389/src/frontend/handlers.go#L194) |
| 商品查询、购物车读写使用 gRPC | [rpc.go / getProduct、getCart、insertCart、emptyCart](https://github.com/GoogleCloudPlatform/microservices-demo/blob/38e7348eb289eb5b87c0c6e8cb19ced0449dc389/src/frontend/rpc.go#L47) |
| cartservice 接收 AddItem 并等待存储操作完成 | [CartService.cs / AddItem](https://github.com/GoogleCloudPlatform/microservices-demo/blob/38e7348eb289eb5b87c0c6e8cb19ced0449dc389/src/cartservice/src/services/CartService.cs#L31) |
| Redis 配置下读取已有购物车、合并条目、写回 | [RedisCartStore.cs / AddItemAsync](https://github.com/GoogleCloudPlatform/microservices-demo/blob/38e7348eb289eb5b87c0c6e8cb19ced0449dc389/src/cartservice/src/cartstore/RedisCartStore.cs#L30) |

图 02 中的 C# `async/await` 不等于单向异步消息：此处客户端等待 gRPC 结果，因此采用 UML 请求与回复记法。Redis 读写是存储交互概括，没有把缓存包装器方法误写成 Redis 原生命令。

## 检查记录

运行环境：Excalidraw JS 库 0.18.0 的 `exportToSvg`，已有 Playwright/Chromium 本地渲染后输出 PNG（2 倍像素）。使用远程加载的运行库和字体，图稿没有上传到协作服务。SVG 嵌入本次使用的字体；源文件在其他环境打开时仍可能需要字体或回退字体。

| 图号 / 规则 | 结果 | 依据 |
| --- | --- | --- |
| 全部 / S1–S3 | 通过 | 图头说明用途/图种与范围；每页有图例；01/02 来源见上表，03 明示为原创方案 |
| 01 / A1–A3 | 通过 | 单一系统边界；应用与存储同属容器层主体；节点有名称/类型/技术/职责；关系标动作与协议 |
| 02 / B2 | 通过 | 五条生命线；请求使用实心箭头，回复使用虚线开放箭头；商品查询在 AddItem 之前 |
| 03 / B1 | 通过 | fork/join 粗条表达并行；菱形上标判断、出口有条件；两个活动终点明确 |
| 00 / D1 | 通过 | 明示风格示意，三列内容一致；未冒称架构标准 |
| 全部 / V1–V3 | 通过 | 实际查看全部预览，中文可读；修正 01 中靠近边界的标签后重新导出；未发现明显重叠或裁切 |
| 全部 / F1 | 通过（已渲染范围） | 147 个原生元素；JSON、唯一 ID、有效坐标、文字/箭头反向绑定检查通过；实际导出成功 |
| 全部 / F2 | 通过 | 四套同版本源文件、SVG 与 PNG；所有正式成果在本目录 |
| 其他图种规则 | 不适用 | 本包无状态图、BPMN、部署图；00/02/03 不采用 C4 |

限制：未逐个在交互编辑器中拖动验证。时序消息及连接并行条的部分箭头依赖布局定位，移动生命线或并行条后需检查并调整连线；所有形状、文字和箭头仍为独立可编辑元素。图稿是按所述范围的示例，不能据此声称已完成开源系统的运行验证或全量架构审计。

## 分享时可以这样介绍

> 这套技能先根据问题选择 C4/UML 等表达方式，再按用户偏好采用手绘、简洁工程或白板草图。图稿保留可编辑源文件，实际导出预览，并附来源与检查说明。这里的架构和时序示例来自公开的 Online Boutique，CI 流程为独立原创示意。

修改 `.excalidraw` 后，重新导出对应 SVG/PNG 并更新版本。正式修订使用 v02，保留本次 v01。
