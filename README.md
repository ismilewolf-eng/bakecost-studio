# BakeCost Studio (暖木工坊 · 配方成本与定价核算器)

专为海外独立烘焙师、家庭私房甜品作坊 (Cottage Bakery) 与小型 Café 设计的纯前端单页成本核算与智能定价工具。

## 🌟 核心特性
1. **100% 浏览器本地存储 (Local Storage)**：所有配方商业机密绝不上云，保护独立烘焙师配方隐私。
2. **40+ 常见烘焙原料容重/密度自动换算**：支持按重量 (g/kg/oz/lb) 或体积 (Cup/勺) 自动折算进货包装单价。
3. **损耗与烤损率 (Yield % & Baking Loss)**：精准计入去皮去蒂损耗与整炉烘烤水分蒸发。
4. **三段式成本分摊**：食材原料 + 包装耗材 + 人工工时 + 水电固定开销分摊。
5. **实时双向定价联动**：拖动目标 Food Cost % 滑块即时给出建议售价，或输入期望售价反推实际毛利与净利润。
6. **一键生成高规格报价单**：支持一键导出高清成本卡图片 (PNG) 或调用系统打印存为 PDF。
7. **数据冷备份与恢复**：支持一键导出/导入完整配方 JSON。

## 🚀 部署上线指引 (Cloudflare Pages)
1. 登录 Cloudflare Dashboard -> 进入 Workers & Pages;
2. 选择 Create application -> Pages -> Upload assets;
3. 将当前 outputs 文件夹直接拖拽上传;
4. 点击 Deploy 即可获得免费极速的 global CDN 线上站点!
