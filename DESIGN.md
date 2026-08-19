# DESIGN.md — BakeCost Studio (暖木工坊)

## 1. 视觉主题与核心氛围
- **定位**：海外独立烘焙师、家庭烘焙坊 (Home Bakery) 与精品 Café 的配方成本核算与定价工具。
- **气质**：温暖、亲和、手作质感与精确后厨实用主义的结合（Notion Warm / Stripe / Airbnb 家族）。
- **色板**：页面底色 #FAF8F5 (Oat Milk) + 纯白卡片 #FFFFFF + 焦糖琥珀重音 #D97706 / #B45309。

## 2. 排版与交互约定
- **数字显示**：启用 `font-variant-numeric: tabular-nums`，保证金额与用量跳动时无列宽抖动。
- **触控优化**：最小触控高度 44px~48px，大按键便于后厨单手操作。
- **工艺密度**：`::selection` 全局染色为焦糖琥珀 #FEF3C7，双向滑块联动与 100% 离线秒开。
