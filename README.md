## Compositor 中文版[汉化编译]

**这是自用分支工作记录而非 PR —— 作者 README 明确拒绝 localization PR。

**基于上游 robbietilton/Compositor 从纯英文界面编译为简体中文，
并编译出可用的 macOS App。
- 原作者：Robbie Tilton ｜ 许可证：MIT（原作者所有）
- 本地化性质：个人自用汉化，未向上游提交 PR（详见「为什么不提交上游」）
- 构建环境：macOS 27.0 (26A434) · M3 Max · Xcode 27.0 (27A266a)

---

## 一、为什么做
Compositor 是 2026 年 9 月出现的新项目（MIT 开源，7 MB，作者单人维护），
对 PS 工作流用户很友好，但界面全英文，工具选项条上大量专业术语（Exposure / Curves / Gradient Map / Hue-Saturation / Blend Mode 等）对中文用户有阅读成本。
作者在 README 中明确说明不接受 localization PR：
Compositor is English only for now, and translation pull requests aren't being accepted.It's maintained by one person, every new string would need translating from then on,and translations in languages I don't read can't be reviewed.
因此本工作是自用性质的独立分支，不、不、也不打算合并回上游。


---


## 二、翻译范围与术语规范
### ２.１规模
| 项                     | 数量                                         |
| ---------------------- | -------------------------------------------- |
| 唯一 UI 文案（去重后） | **423 条**                                   |
| 枚举成员本地化         | **63 个**（分布在 5 个枚举）                 |
| 改动文件               | **100**（`.swift` 99 + `project.pbxproj` 1） |
| diff 规模              | 1349 增 / 1264 删                            |

### 文案分类（按可见频率）：
| 类别               | 条数 | 说明                   |
| ------------------ | ---- | ---------------------- |
| 标签文本 `Text`    | 121  | 面板参数标签、状态提示 |
| 悬停提示 `.help()` | 121  | 长句说明，最费翻译工时 |
| 按钮 `Button`      | 88   |                        |
| 开关 `Toggle`      | 37   |                        |
| 选择器 `Picker`    | 20   |                        |
| 无障碍标签         | 20   | VoiceOver 朗读文案     |
| 菜单标题           | 5    | 主菜单                 |
| 错误/状态信息      | 7    |                        |
| 警告框             | 3    |                        |


### ２.2 术语规范：对齐 Photoshop 中文版
中文 PS 用户的肌肉记忆在 Adobe 官方译名上，**自造译名会让软件显得山寨**。
所有有 Adobe 官方译名的条目一律采用官方译名：
| 英文                                                | 采用译名                                    | 说明                               |
| --------------------------------------------------- | ------------------------------------------- | ---------------------------------- |
| Mask                                                | **蒙版**                                    | 不是「遮罩」                       |
| Selection                                           | **选区**                                    |                                    |
| Adjustment Layer                                    | **调整图层**                                |                                    |
| Feather / Contract / Expand                         | **羽化** / **收缩** / **扩展**              |                                    |
| Content-Aware Fill                                  | **内容识别填充**                            |                                    |
| Levels / Curves                                     | **色阶** / **曲线**                         | 不是「色位」「调整曲线」           |
| Hue / Saturation / Luminance                        | **色相** / **饱和度** / **明度**            | Luminance 用「明度」，不用「亮度」 |
| Copy Merged                                         | **合并拷贝**                                |                                    |
| Ungroup                                             | **取消编组**                                | 不是「解组」                       |
| Trim vs Crop                                        | **裁掉** vs **裁剪**                        | 两者是不同操作，必须区分           |
| Darken / Lighten                                    | **变暗** / **变亮**                         | 不是「加深」「减淡」               |
| Pin Light / Vivid Light                             | **点光** / **亮光**                         | 两者常被混译                       |
| Linear Dodge (Add)                                  | **线性减淡（添加）**                        | PS 保留括号补充                    |
| Divide / Subtract                                   | **划分** / **减去**                         |                                    |
| Hue / Saturation / Color / Luminosity（混合模式组） | **色相** / **饱和度** / **颜色** / **明度** |                                    |
| Nearest Neighbor                                    | **最近邻**                                  | 不是「最邻近」                     |
| Grouped / Guided（镜头校正）                        | **引导式**                                  |                                    |


### ２.3 排版规范
- **可选项**用直角引号：`「基本」`、`「高级」`、`「线性」`（对应 UI 里的分段控件）
- **修饰键保留英文**：`Command` / `Option` / `Shift` / `Control` —— 符合 macOS 中文本地化惯例
- **快捷键符号**保留 macOS 原生：`⌘` `⇧` `⌥` `⌃`
- **拉丁字母缩写保留**：`X` `Y` `W` `H`（变换面板坐标/尺寸）、`sRGB`、`DPI` —— PS 中文版同样保留
- **格式名保留**：`JPEG` / `PNG` / `TIFF` / `HEIC` / `PSD`
- **产品名与专有名词保留**：`Camera Raw`、`Adobe`
- **省略号**统一用 `…`（与上游一致，上游本身不用三个点）


### ２.4 刻意不翻译的部分
翻译这些会直接导致功能损坏：
| 类别                      | 例子                                                         | 原因                                       |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------ |
| **PSD 格式键名**（45 条） | `TySh` `SoCo` `SoLd` `PxSD` `Mtrn` `GlbC` `Vrtc` `Idnt` `UntF` `RunArray` `ResourceDict` `StyleSheet(Data)` `FauxBold` … | PSD 二进制格式的 key，改了文件解析直接失败 |
| **Core Image 滤镜名**     | `CIEdgePreserveUpsampleFilter`                               | 框架 API 名                                |
| **SF Symbol 图标名**      | `eye` `chevron.down` `hand.draw` `plus.magnifyingglass`      | 系统符号标识符                             |
| **4 字符颜色通道名**      | `Alph` `Clss` `Bqm` `Athm`                                   | PSD 内定通道 ID                            |
| **偏好设置 key**          | `gridColor` `gridStyle`                                      | UserDefaults 持久化键                      |
| **调试/底层标识**         | `levelsInput` `levelsOutput` `mmapped file changed`          | 内部实现标识                               |


---


## 三、最终遗留的英文（刻意保留）
| 位置                         | 保留原因                         |
| ---------------------------- | -------------------------------- |
| `导出 PNG…` / `导出 JPEG…`   | 格式名                           |
| `Camera Raw`                 | Adobe 专有功能名，与 PS 保持一致 |
| `移到"Mr.Cody's iPad Pro 5"` | 设备名，系统生成                 |
| `Compositor 中文版帮助`      | 产品名                           |
| `X` `Y` `W` `H`              | PS 中文版同样保留拉丁缩写        |
| `sRGB` `DPI` `Gamma`         | 标准术语                         |


---


## 四、已知限制

**⚠️ 无自动更新**

这是本发行版最重要的限制。上游仍在活跃更新（发布节奏约 1–2 天一个小版本），
但本版无法自动获取新版本，需手动重新构建（见下节）。

**⚠️ 非官方签名**

App 使用 ad-hoc 签名（`Signature=adhoc`，无 Team ID）。这意味着：

- 首次打开需手动清除隔离属性（见安装说明）
- 无法通过 Gatekeeper 的公证检查
- 不适用于需要分发给他人的场景

**⚠️ 与官方版数据不互通**

`.comp` 项目文件内部按英文标识存储，因此：

- 本版创建的项目，**官方版打不开**（枚举标识对不上）
- 官方版创建的项目，本版**可以打开**（界面文案不同不影响数据读取）
- 图层混合模式、调整类型、滤镜类型均以英文标识持久化，跨版本打开安全

**⚠️ 与官方版共用偏好设置**

本版的 Bundle ID 为 `com.wonderassembly.compositor.zh`，但 macOS 的 `UserDefaults`
在某些情况下会按进程主 bundle 解析偏好域，因此**可能与官方英文版共用同一份偏好设置**。

若你同时安装了官方版，两者的以下偏好可能互相覆盖：

| 偏好                                        | 影响                     |
| ------------------------------------------- | ------------------------ |
| `NSWindow Frame editor`                     | 窗口位置与尺寸           |
| `tool.grid` / `tool.guides` / `tool.rulers` | 参考线、标尺、网格开关   |
| `layersPanelWidth` / `navigator.visible`    | 面板宽度与导航器显示状态 |

**建议**：不要与官方版同时使用，或在切换后重置 Compositor 偏好：

```bash
defaults delete com.wonderassembly.compositor
defaults delete com.wonderassembly.compositor.zh
```

> 说明：本条为已知的不确定项。实测中清空偏好域后运行本版并未重新写入，
> 但完整的人机交互流程（真正拖动面板、开关参考线）未能在自动化环境复现，
> 故无法完全排除共用情形。如遇异常，以上面的 `defaults delete` 解决即可。

**其他**

- 仅支持 Apple Silicon，Intel Mac 无法运行（与上游一致）
- 需要 macOS 26.0 或更高版本

---

## 五、安装

### 前置条件

- macOS 26.0 或更高版本
- Apple Silicon（M 系列）Mac

### 步骤

```bash
# 1. 复制到应用程序文件夹
cp -R Compositor-中文版-1.4.8.app /Applications/Compositor.app

# 2. 清除隔离属性（自签名 App 必需）
xattr -dr com.apple.quarantine /Applications/Compositor.app

# 3. 打开
open /Applications/Compositor.app
```

若提示「无法验证开发者」，右键点击 App 图标选择「打开」，然后在对话框中再次确认「打开」。

### 卸载

```bash
rm -rf /Applications/Compositor.app

# 清理偏好设置（如有）
defaults delete com.wonderassembly.compositor.zh 2>/dev/null
```

本版不创建 `~/Library` 下的独立配置目录（偏好存于 `UserDefaults`），
删除 App 本身即完成卸载。


---


## 六、为什么不提交上游
作者的 README 已明确拒绝 localization PR，理由是单人维护、每加一个功能就要重翻一遍、且无法 review 不懂的语言。本工作因此定位为**自用分支**，没有向上游开 PR。
如果将来上游愿意接受，本地化的完整改动可通过以下方式提供：

```bash
# 补丁（约 420 KB，含全部改动）
git apply Compositor-zh.patch
```

---

## 七、后续维护
上游仍在更新，本地化会根据上游需要定期维护跟进：
**主要维护成本**：上游每加一个功能就要补翻该功能的 UI 文案。
由于没有引入 `.lproj` 资源文件体系（那样需要改动 230 个 Swift 文件的调用方式），
本工作选择了直接改字面体的轻量路线，代价是每次同步都要重新跑抽取与替换。


---


## 附：产物清单
| 文件                                               | 说明                                                     |
| -------------------------------------------------- | -------------------------------------------------------- |
| `Compositor-zh.patch`                              | 全部改动（100 文件），可 `git apply` 到任意干净 clone    |
| `_src/`                                            | 已中文化的完整源码                                       |
| `build_zh.sh`                                      | 一键构建 + 组装 + 注入 + 签名                            |
| `Compositor-中文本地化-翻译对照表.xlsx`            | 三页签：翻译对照表（423 条）/ 按文件归组（带行号）/ 说明 |
| `extract.py`                                       | 从源码抽取 UI 字符串                                     |
| `apply_zh.py` / `apply_extra.py` / `apply_map3.py` | 三批替换（控件 / 三元拼接 / 菜单面板）                   |
| `dump_menus.py`                                    | 运行时抓菜单树，验证本地化                               |
| `merge_map.py`                                     | 译文映射合并与校验                                       |
| `zh_batch1-3.txt` / `zh_map.json`                  | 573 条译文数据                                           |
| `remove_sparkle.py`                                | 摘除 Sparkle SPM 依赖                                    |


---



## 致谢
- **Robbie Tilton** —— Compositor 作者，MIT 开源。本工作建立在作者的开源代码之上，所有逻辑与架构均来自上游，汉化仅涉及界面文字。
- **Adobe** —— 混合模式、滤镜、调整图层等术语译名以 Photoshop 中文版为基准，这些译名是 Adobe 官方资产，此处仅作界面文案参照使用。


---

## 中文界面预览
<img width="2057" height="1260" alt="Capture_26-10-09_172610" src="https://github.com/user-attachments/assets/61879d5a-6eaa-4bb6-98bf-86b73feb2aa4" />
