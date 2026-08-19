# 烘焙与餐饮配方成本核算器 — 出海独立开发实战方案

**定位一句话**：一个不需要注册、不联网也能用、绝不上传你配方数据的极简成本定价工具——比 Excel 模板快，比老牌网站的 UI 新十年。

---

## 1. 核心数学模型与计算公式

### 1.1 原料单价换算（统一到"基准单位成本"）

```
基准单价 = 采购价格 / (采购规格数量 × 单位换算系数)
```

- 质量类：kg→g（×1000）、lb→g（×453.6）、oz→g（×28.35）
- 体积类：L→ml（×1000）、cup→ml（×236.6）、tbsp→ml（×14.8）、tsp→ml（×4.9）
- 计数类：dozen→个（×12）、pack(n)→个

内置常见原料密度表（g/ml），支持体积→重量自动换算，用户可覆盖：

```
换算后克数 = 体积(ml) × 密度(g/ml)
```
如：面粉 ≈0.53g/ml、砂糖 ≈0.85g/ml、黄油 ≈0.96g/ml、全蛋液 ≈1.03g/ml

这是绝大多数 Excel 模板不具备的能力，是内容 + 产品双重差异化点。

### 1.2 出成率 / 损耗率（Yield Rate）

```
出成率 = 实际可用量 ÷ 采购/投入量 × 100%
损耗调整后单价 = 原料基准单价 ÷ 出成率
```

三类损耗场景做成预设模板：
- 加工损耗（削皮/去核/分蛋，如只用蛋黄出成率≈33%）
- 烘烤损耗（面团→成品水分蒸发，常见8%–15%）
- 操作损耗（搅拌盆残留、洒落、挤料浪费，经验值2%–5%，可自定义）

### 1.3 包装耗材成本

```
单件包装成本 = Σ(耗材单价 × 用量) ÷ 分摊件数
```
支持"每份包装" vs "共享包装"两种分摊模式（开关切换）。

### 1.4 人工与水电/设备分摊

```
单批人工成本 = 时薪 × 制作耗时(备料+烘烤+装饰+打包)
单件人工成本 = 单批人工成本 ÷ 本批产出件数

单件水电分摊 = 月度水电账单 ÷ 月度批次数 ÷ 单批产出件数
单件设备折旧 = 设备购置价 ÷ 预计可用次数
```
支持"按批次实分摊"（精确）与"按营收百分比估算"（简单，新手用，如按售价8%）两种口径。

### 1.5 总成本与目标毛利定价反推

```
单件总成本 = 损耗调整后原料成本 + 包装成本 + 单件人工成本 + 单件分摊成本
```

三种定价法均需支持（对应不同搜索意图）：

① Food Cost % 反推法（行业标准，SEO搜索量最大）：
```
建议售价 = 单件总成本 ÷ 目标食材成本占比(Food Cost %)
```
烘焙行业目标食材成本占比一般 25%–35%。

② 目标毛利率法：
```
建议售价 = 单件总成本 ÷ (1 − 目标毛利率%)
```

③ 简单加价法（Markup）：
```
建议售价 = 单件总成本 × (1 + 加价率%)
```

**关键差异化点**：加价率(Markup) 与 毛利率(Margin) 不同（50%加价 = 33%毛利），做双向联动滑块同时显示两个数字，是很强的传播/内容点（"markup vs margin calculator"）。

附加能力：
- 配方等比缩放（×0.5/×2/×3，成本联动重算）
- 盈亏平衡点：`保本销量 = 固定成本 ÷ (单件售价 − 单件变动成本)`
- 心理定价取整（就近取整到 .50/.99/整数，可关闭）

---

## 2. 产品体验：对 Excel 模板和老旧网站的降维打击

竞品两类：① Etsy 上 $5-15 的静态表格模板（不算体积↔重量、不适配移动端、易改坏公式）；② 2008年画风的老牌计算器网站（广告多、不响应式、需注册、只有一种定价公式）。

| 维度 | 老工具痛点 | 我们的打法 |
|---|---|---|
| 隐私/信任 | 常暗示/要求上传数据 | 全程 LocalStorage，零后端，首屏强调"配方成本只存在你的设备上" |
| 移动端 | Excel手机编辑体验差；老网站不响应式 | 大触控目标、数字键盘优化、原料行左滑删除/复制、底部悬浮"建议售价" |
| 实时性 | 需点击计算/改公式 | 全联动实时重算，毛利滑块拖动价格跟手变化 |
| 专业度 | 不算出成率/体积换算/markup vs margin | 内置密度表+出成率预设+双显示 |
| 可视化 | 纯数字表格 | 环形图实时展示成本构成占比 |
| 留存 | 每次重填 | 本地多配方保存/切换、JSON导入导出 |
| 离线 | 依赖网络 | PWA离线+添加主屏幕，适合厨房弱网环境 |
| 国际化 | 多数只支持美制/USD | 公制/英制切换、多币种符号（不做实时汇率，保持零后端）|
| 导出/分享 | 无 | 品牌化报价单PDF/图片导出（付费点）|

首屏电梯陈述：**"Calculate your true bakery cost in under 60 seconds — no signup, no spreadsheet, 100% private."**

---

## 3. SEO 长尾词矩阵与内容截流策略

核心策略：程序化SEO（Programmatic SEO）——同一计算引擎，预设不同默认值/文案/H1生成多个独立落地页，内部互链导流到核心工具。

### 3.1 关键词矩阵

**A. 核心工具词**：bakery pricing calculator / bakery cost calculator、cake pricing calculator、cookie cost calculator、cupcake pricing calculator、recipe cost calculator、food cost calculator、food cost percentage calculator

**B. 细分品类词**（独立落地页，量小竞争低，起量主力）：custom cake price calculator、wedding cake cost calculator、macaron cost calculator、sourdough bread pricing calculator、catering cost calculator、home bakery pricing calculator、small batch bakery pricing

**C. 近邻手作品类**（同一模型换皮扩TAM）：soap making cost calculator、candle making cost calculator、craft pricing calculator / handmade product pricing calculator

**D. 信息型长尾词**（博客内容页，建立权威度+反向导流）：how to price baked goods for profit、how to calculate food cost percentage、what percentage should food cost be、markup vs margin calculator、how much should I charge for a cake、ideal food cost percentage for small bakery

**E. 比较/替代词**（截流竞品流量）：food cost calculator free、bakery costing spreadsheet template alternative

### 3.2 落地页结构模板

```
/tools/cake-pricing-calculator
  H1: Cake Pricing Calculator — Price Your Cakes for Profit in Seconds
  复用计算引擎，仅预置该品类原料密度/损耗默认值
  嵌入实操案例（如8寸生日蛋糕全流程算价）
  FAQ区块 + FAQPage Schema
  底部"相关计算器"内链矩阵
```

技术SEO：SoftwareApplication + FAQPage + HowTo 结构化数据；每页配套800-1200字博客文章；反向链接来源：Etsy工具类目、r/AskBaking、r/Baking、r/smallbusiness、Pinterest（成本分解可视化图天然传播）。

---

## 4. 变现模式与商业化路径

| 模式 | 打法 | 备注 |
|---|---|---|
| 展示广告 | 前期AdSense过渡，流量稳定后转Ezoic/Mediavine | 广告位克制，不干扰计算器核心交互 |
| 联盟营销 | Amazon Associates（厨房秤/工具）、包装耗材品牌（Uline/Nashville Wraps）、小微商家SaaS（Square/Shopify）、Cottage Food Law相关服务 | 与"刚定完价准备开卖"用户心智高度契合 |
| Freemium | 免费：无限本地计算+保存1-3配方；Pro（$19-29一次性 或 $3-5/mo）：无限配方、品牌化PDF导出、多配方对比、CSV导出 | 一次性买断转化率通常优于订阅 |
| 模板/资产销售 | Gumroad卖细分定价模板包 | 边际成本近零，适合冷启动变现 |
| B2B授权/嵌入 | 烘焙品牌/学校网站嵌入组件获客 | 长期最优利润率路径 |
| 打赏 | Buy Me a Coffee | 零成本baseline |
| 邮件列表 | "定价指南PDF"换邮箱，不强制注册 | 复购渠道，需保持"无需注册"核心卖点 |

优先级节奏：Day3上线即接AdSense占位 → 第1月做联盟营销+博客内容 → DAU>500后上线Pro一次性解锁 → 流量起来后转Mediavine/Ezoic。

---

## 5. 3天极速交付排期

技术选型：纯 HTML/CSS + 原生JS（或Alpine.js）+ Vite，零重框架，首屏秒开，天然适配 Cloudflare Pages。

### Day 1 — 核心引擎与UI骨架
- 项目脚手架、LocalStorage数据层（配方/原料/设置）、单位换算与密度表模块
- 核心计算引擎（1.1-1.5全部公式）+ 关键公式单元测试
- 移动优先UI骨架：输入区、实时成本卡片、环形图占位、保存/加载配方列表

### Day 2 — PWA化 + 体验打磨 + SEO骨架
- manifest.json + Service Worker（App Shell离线缓存）、添加主屏幕引导
- 交互打磨：markup/margin双联动滑块、原料行左滑操作、暗色模式、心理定价取整
- 导出能力：JSON导入导出、报价单PDF/图片导出（html2canvas等纯前端方案）
- 落地页模板化：产出3-5个高优先级细分页 + Schema标记
- 接入隐私友好统计（Cloudflare Web Analytics / Plausible）

### Day 3 — 内容、QA、部署、冷启动
- 补全长尾内容页（真实案例+FAQ+公式讲解）
- 真机QA：iOS Safari PWA安装、Android Chrome、断网测试、多屏幕触控
- Cloudflare Pages部署、自定义域名+SSL、提交Sitemap到GSC、申请AdSense
- 冷启动分发：Product Hunt / Indie Hackers、Reddit相关社区、烘焙Facebook小组、Pinterest

---

## 待决策事项

1. 技术栈：纯静态+Vite，还是特定框架（React/Svelte）以支持后续更复杂Pro功能？
2. 首发语言：先英文打海外SEO，还是中英双语同时上线？
3. 是否立即按Day1排期开始搭建项目骨架？
