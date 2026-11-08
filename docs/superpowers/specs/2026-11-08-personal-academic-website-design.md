# Danning Li 个人学术网站 — 设计规格

日期：2026-11-08
状态：已获用户批准（方案 A）

## 1. 目标

将现有 Academic Pages (Jekyll) 模板改造为 Danning Li 的简约英文学术个人网站，完整覆盖：

- 个人简介与当前身份
- 专业与研究方向
- 教育背景
- 研究/实习经历
- 论文列表及链接（链接待用户最后提供）

部署方式遵循仓库 `README.md`：推送到 `origin/master` 后由 GitHub Pages 自动构建。

## 2. 非目标

- 不更换静态站点生成器，不迁移到其他主题
- 不添加博客、项目集、教学、演讲等栏目
- 不做中英双语
- 不添加复杂动效、背景图或第三方前端框架
- 不在本次提供真实头像（保留占位图，用户以后同名覆盖）

## 3. 方案选择

评估了三种方案：

1. **在现有 Academic Pages 主题上做减法（采纳）** — 保留 Jekyll/GitHub Pages 原生支持、学术排版、SEO 与论文模板；主要工作是清理模板残留、修复配置、重构内容。
2. 纯 HTML/CSS 重写 — 视觉完全可控，但需自行实现响应式、SEO、论文页，维护成本高，且偏离 README 流程。
3. 迁移到 al-folio 等其他主题 — 功能更丰富，但迁移量和依赖大，与现有内容和部署方式不一致。

## 4. 信息架构

导航栏仅保留：**About / Publications / CV**。

### 4.1 首页 `/`（About）

- 标题：About Me
- 三段英文简介（基于现有 `_pages/about.md` 润色）：
  1. McGill University 统计学本科，计算机科学辅修，2025 年毕业
  2. HKUST(GZ) Red Bird MPhil Student，金融科技学域，导师 Dr. Chao Zhang；研究 Robust AI for Asset Pricing；同时担任 Research Assistant
  3. 与四川大学华西医院 Ting Yin、Yuhao Yi 合作，参与乳腺病理临床病例分析大模型研究
- **Research Interests** 小节，列出：AI + X、Robust AI for Asset Pricing、Medical Vision-Language Models、World Models
- 页尾入口：`View my CV →`、`Publications →`
- 侧栏信息：占位头像、姓名、pronouns (he/him)、简短 bio、Guangzhou、HKUST(GZ)、Email、GitHub；移除指向本站自身的 "Website" 链接

### 4.2 论文页 `/publications/`

- 分类：Conference Papers
- 三篇论文，按年份倒序：
  - GeoWorld: Geometric World Models — CVPR 2026
  - FlashMo: Geometric Interpolants and Frequency-Aware Sparsity for Scalable Efficient Motion Generation — NeurIPS 2025
  - Infant Cry Detection Using Causal Temporal Representation — ICASSP 2025
- 列表项展示：标题、作者、会议、年份、一句话摘要、可用链接
- 每篇有独立详情页：标题、作者、会议、摘要、Paper/arXiv/DOI、Code、Poster 链接
- 链接字段当前留空，待用户最后提供后填入；不得编造链接或作者顺序

### 4.3 CV 页 `/cv/`

版块顺序：

1. **Education**
   - MPhil in Financial Technology, HKUST (Guangzhou), 2028 (Expected)；Thrust of Financial Technology；Supervisor: Dr. Chao Zhang；Research focus: Robust AI for Asset Pricing
   - B.A. in Statistics (Minor: Computer Science), McGill University, 2025
2. **Research Experience**（沿用用户确认的现有 4 段，标题使用 "Research Experience"）
   - Spring 2026：Research Assistant, HKUST(GZ)；robust asset pricing under factor feature perturbations；Supervisor: Dr. Chao Zhang
   - Fall & Winter 2025：Research Assistant, West China Hospital, Sichuan University；pathology vision-language model fine-tuning；model honesty in clinical settings；Collaborators: Ting Yin, Yuhao Yi
   - Spring 2025：Research Assistant, HKUST(GZ)；asset pricing under financial market regime switching；Supervisor: Dr. Chao Zhang
   - Summer 2024：Research Assistant, MBZUAI；biological audio recognition and dataset construction；infant cry detection and causal temporal representation；Supervisor: Hanan Aldarmaki
3. **Publications** — 三篇论文，标题链接到对应详情页，会议名链接会议官网
4. **Skills** — Data Processing & Analysis；Statistical & Machine Learning；Multimodal & Vision-Language Models；Asset Pricing & Quantitative Finance

## 5. 视觉设计

- 沿用 Academic Pages 默认白色极简学术排版
- 无 banner、无背景图、无动画、无多余图标
- 统一标题层级、段落间距、列表间距
- 侧栏仅保留必要个人与联系方式
- 不修改主题核心 `_sass`，除非出现明显排版问题

## 6. 技术改动

1. **修复 `_config.yml` 的 YAML 语法错误**：`author.bio` 当前由两段字符串错误拼接，存在悬空引号，必须合并为单一合法字符串。
2. 更新 `description`、`author.bio`；保留姓名、pronouns、location、employer、email、GitHub、avatar。
3. 更新 `_data/navigation.yml` 为 About / Publications / CV。
4. 用 3 篇真实论文替换 `_publications/` 下 5 篇模板占位论文；`publication_category` 仅保留 `conferences`。
5. 删除模板演示内容（删除前确认无导航或页面引用；git 历史可恢复）：
   - `_posts/` 下 5 篇 demo 文章
   - `_portfolio/`、`_talks/`、`_teaching/` 下 demo 条目
   - `_pages/` 下 markdown guide、portfolio、talks、teaching、year-archive、cv-json 等演示页
   - `_data/cv.json`（仅 cv-json 使用）
   - 保留 `404.md`、`sitemap.md`
6. 删除 `files/` 下模板示例 PDF/BibTeX，添加 `files/.gitkeep` 保留目录，供用户后续上传 CV/论文 PDF。
7. 保留 `images/profile.png` 占位头像；其他模板示例图片不再被引用，但保留在仓库中以避免误删依赖。

## 7. 待补信息（最后一步）

用户提供每篇论文的：

- 完整作者列表与作者顺序
- 会议全称
- 年份
- Paper / arXiv / DOI 链接
- Code 链接（如有）
- Poster 链接（如有）
- 一句话摘要

在收到信息前，不虚构任何作者或链接。

## 8. 验证

1. 使用本机 Docker 按 README 运行 `docker compose up`，在 `http://localhost:4000` 检查：
   - 首页简介、Research Interests、侧栏信息
   - Publications 列表与详情页
   - CV 页四个版块
   - 导航栏无失效链接
   - 移动端宽度下布局正常
2. 检查构建日志无 Liquid/YAML 错误，无 `site.baseurl` 造成的资源 404。
3. 检查生成页面中无模板占位文字（"Paper Title Number"、"Your Name" 等）。

## 9. 部署

1. commit 所有改动到 `master`
2. push 到 `origin/master`
3. GitHub Pages 自动构建（前提：仓库 Settings → Pages 已从 `master` 分支发布）
4. 通过公开 URL `https://danninglee.github.io/` 检查线上页面
5. 若推送失败（凭据问题）或 Pages 未启用，明确告知用户所需操作

## 10. 验收标准

- 首页、论文页、CV 页均可在本地和线上访问
- 三页内容完整覆盖简介、专业、研究方向、教育背景、研究/实习经历、论文
- 导航仅含 About / Publications / CV
- 页面无模板占位内容
- 构建无错误，推送后 GitHub Pages 成功更新
