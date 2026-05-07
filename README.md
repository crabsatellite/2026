# 2026 — works repository

本仓库存放 2026 年的设计文件 (图纸、模型、模拟数据、文本)。它是
[dorawolf.com](https://dorawolf.com) **作品档案页 (`/archive/`) 的数据源**——
网站构建时从这里抓取每个项目的封面图和文件下载链接，所以只要按下面的格式
摆放文件，新作品会**自动**出现在网站上，不需要写一行代码。

---

## 1. 一个项目长什么样

每个项目是仓库根目录下的一个文件夹，文件夹名格式：

```
YYYY-MM-DD_<type>_<slug>
```

| 段位 | 规则 | 例子 |
|------|------|------|
| `YYYY-MM-DD` | 这件作品的代表日期 (开题日 / 提交日 / 验收日，自己定) | `2026-04-29` |
| `<type>` | 三选一：`thesis` (长期研究) · `project` (项目) · `coursework` (短期研究) | `thesis` |
| `<slug>` | 小写英文 + 连字符。**不要拼音首字母**，让人能看懂 | `fallout-shelter` |

完整例子：`2026-04-29_thesis_fallout-shelter/`

---

## 2. 项目内部文件夹结构

```
2026-04-29_thesis_fallout-shelter/
├── images/                 ← 图纸、效果图、可对外展示的图片
│   ├── 1.png  2.png  3.png    ↑ 数字命名，越小越靠前 (1.png 自动当封面)
│   └── _raw/                  ↑ 微信截图、临时图等，不上网站画廊
│       └── 微信图片_*.png
├── models/
│   ├── rhino/         *.3dm
│   ├── sketchup/      *.skp *.skb
│   ├── grasshopper/   *.gh *.ghx
│   └── cad/           *.dwg *.dxf
├── docs/               ← *.pdf  *.docx  *.pptx  *.md (任务书、文本汇报)
├── data/               ← *.xlsx *.csv  *.json (分析与计算表格)
├── simulation/         ← Ladybug / EnergyPlus / Octopus 输入输出 (.epw .idf .gh-octopus 等)
└── archives/           ← *.rar *.zip (大型依赖包，气象数据库等)
```

**只有 `images/` 根目录** (不含 `_raw/`) 里的图片会被网站当作画廊和封面。
`images/_raw/` 里的文件仍然安全保存，只是不会出现在公开页面上。

---

## 3. 网站怎么读取这里

1. 网站构建时跑一个脚本 (`crabsatellite/dorawolf_portfolio/tools/organize.py`)
2. 脚本扫描本仓库根目录下符合 `YYYY-MM-DD_<type>_<slug>/` 命名的文件夹
3. 为每个项目自动产出 `manifest.json` 条目，包含：
   - 标题、日期、类型 (从文件夹名解析)
   - 封面 URL (= `images/` 下数字最小的图)
   - 所有文件的 `raw.githubusercontent.com` 直链
4. 网站直接 `<img src>` 这些 URL —— **网页本身一字节图片都不存**

> 这意味着：你在这里 push 一张新图，网站下次 build 就能拿到。

---

## 4. 添加一个新作品 — 三步

```text
① 新建文件夹：2026-MM-DD_<type>_<slug>/
② 把图片放进 images/，模型放进 models/<软件>/，文本放进 docs/
③ git add . && git commit -m "add <slug>: <短描述>" && git push
```

完成。网站下次 build 就会显示这件作品。

---

## 5. 想给作品加文字介绍？

文件夹结构本身**不需要写文字**——网站会根据日期 + 类型自动展示。

如果想给某件作品配中英文标题、简介、tag (像 `2026-04-29_thesis_fallout-shelter`
那样有"呼吸之间——长沙岳麓区防空洞公共空间改造"的描述)，让 crabsatellite
在 `crabsatellite/dorawolf_portfolio/tools/organize.py` 里的 `PROJECT_COPY`
字典加一条就行——本仓库不需要任何配置。

---

## 6. 命名细节

- **日期填什么**：建议用最能代表这件作品的日期。本科毕设填答辩日，
  课程作业填提交日，长期项目填关键里程碑日。后续 push 不要改日期 (一改就
  改了文件夹名，URL 失效)。
- **slug 要不要改**：尽量不改，因为它是 URL 的一部分。如果一定要改，
  通知一下 crabsatellite，他在 site 端一并更新对应的 `PROJECT_COPY` 字典 key。
- **重名怎么办**：日期 + slug 一起做 ID，所以同一天不要两件同名作品。
  如果真有，slug 后面加 `-v2` / `-baicao-variant` 之类的。

---

## 7. 不确定的就 ping crabsatellite

任何关于命名、文件放哪、网站显示效果的问题，问 crabsatellite。
他维护 `crabsatellite/dorawolf_portfolio` 这个网站仓库，从这里读数据。
