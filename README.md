# Vinted 价格追踪器

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://bright.cn)
[![Vinted Price Tracker](https://img.shields.io/badge/Vinted%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://bright.cn/products/insights/price-tracker/vinted)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://bright.cn/products/insights/price-tracker/vinted)

实时 Vinted 价格追踪——一个欧洲二手服装和配饰在线市场。提供两种入门方式：**全托管**情报平台，或用于构建您自有 pipeline 的**自助式 API**。

---

## 选项 1：Bright Insights - AI 驱动的价格追踪（推荐）

**[Bright Insights](https://bright.cn/products/insights/price-tracker/vinted)** 是 Bright Data 的全托管零售情报平台。无需构建 scraper，无需维护基础设施——只需将结构化、可直接分析的价格数据交付到 dashboard、数据 feed 或您的 BI 工具中。

**团队选择 Bright Insights 的原因：**
- 🚀 **零配置** - 通过开箱即用的 dashboard 和数据 feed，在几分钟内上线
- 🤖 **AI 驱动的推荐** - 对话式 AI 助手可将数百万数据点即时转化为可执行洞察
- ⚡ **实时监控** - 支持按小时到按天的刷新频率，并提供即时告警（email、Slack、webhook）
- 🌍 **无限扩展** - 任意网站、任意地区、任意刷新频率
- 🔗 **即插即用集成** - AWS、GCP、Databricks、Snowflake 等
- 🛡️ **全托管** - Bright Data 自动处理 schema 变更、站点更新和数据质量问题

**关键用例：**
- ✅ **追踪 Vinted 上的转售价值趋势**
- ✅ **监控特定商品的挂牌价和成交价**
- ✅ **识别平台之间的套利机会**
- ✅ 监控 MAP 政策合规性并检测定价违规
- ✅ 追踪竞争对手促销及促销动态
- ✅ 将干净、统一的数据直接输入动态定价算法或 AI 模型

> **每月 $250 起 - [获取定制报价 →](https://bright.cn/products/insights/price-tracker/vinted)**

---

## 选项 2：通过 Web Scraper API 自助使用

更倾向于构建您自己的 pipeline？Bright Data 的 **Web Scraper API** 可让您以编程方式访问 Vinted 商品数据——价格、库存状态、评论等——无需管理代理或抓取基础设施。

### 前提条件

- Python 3.9 或更高版本
- 一个 [Bright Data account](https://bright.cn)（提供免费试用）
- 一个 Bright Data **API token**（[如何获取](https://docs.bright.cn/general/account/account-settings#api-token)）
- 一个用于 Vinted 商品的 Bright Data **Web Scraper ID**（[在 Web Scrapers 控制面板中查找](https://bright.cn/cp/scrapers)）

### 设置

1. **克隆此 repository**

   ```bash
   git clone https://github.com/bright-cn/vinted-price-tracker.git
   cd vinted-price-tracker
   ```

2. **安装依赖**

   ```bash
   pip install -r requirements.txt
   ```

3. **配置凭据**

   将 `.env.example` 复制为 `.env`，并填入您的值：

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **查找您的 Web Scraper ID**
   > 登录 [Bright Data Control Panel](https://bright.cn/cp/scrapers)，进入 **Web Scrapers**，
   > 搜索 “Vinted”，然后复制 Web Scraper ID（格式：`gd_xxxxxxxxxxxx`）。

---

## 用法

### 1. 通过 URL 追踪特定商品

传入一组 Vinted 商品 URL 以获取结构化价格数据：

```python
from price_tracker import track_prices

urls = [
    "https://www.vinted.com/product/sample-item-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

或直接运行：

```bash
python price_tracker.py
```

### 2. 通过关键词发现商品

查找与关键词搜索匹配的商品：

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. 通过分类 URL 浏览商品

从 Vinted 分类页面采集所有商品：

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://vinted.com/category/example",
    limit=100,
)
```

---

## 输出字段

每条结果记录包含以下字段：

| Field | Description |
|-------|-------------|
| `url` | 列表 URL |
| `title` | 商品标题 |
| `brand` | 品牌 |
| `price` | 挂牌价格 |
| `currency` | 货币代码 |
| `condition` | 商品成色（全新/二手/等） |
| `size` | 尺码（如适用） |
| `seller_name` | 卖家用户名 |
| `seller_rating` | 卖家评分 |
| `images` | 商品图片 URL |
| `description` | 商品描述 |
| `timestamp` | 采集时间戳 |

### 示例输出

```json
[
  {
    "url": "https://www.vinted.com/product/sample-item-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://vinted.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高级选项

`trigger_collection()` 函数接受可选参数来控制数据采集：

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返回的最大记录数 |
| `include_errors` | boolean | `true` | 在结果中包含错误报告 |
| `notify` | string (URL) | - | 当快照准备就绪时调用的 webhook URL |
| `format` | string | `json` | 输出格式：`json`、`csv` 或 `ndjson` |

示例（带选项）：

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.vinted.com/product/sample-item-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## 资源

- 🌟 [Vinted Price Tracker - Bright Insights (Managed)](https://bright.cn/products/insights/price-tracker/vinted)
- 🔧 [Vinted Scraper API](https://bright.cn/products/web-scraper/vinted)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.bright.cn/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://bright.cn/cp/scrapers)
- 🔑 [How to get an API token](https://docs.bright.cn/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://bright.cn)

---

*由 [Bright Data](https://bright.cn) 构建——行业领先的网络数据平台。*