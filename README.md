# urlcheck
Multi-platform domain security checker – 聚合 QQ、微信、vivo、OPPO、百度、华为等主流平台的域名拦截检测 API，一键查询域名在各大生态内的安全状态，助力站长快速排查风险。


# 多平台域名拦截检测工具

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![API Status](https://img.shields.io/badge/API-Online-brightgreen)](https://tmini.net)

聚合多个主流平台（QQ、微信、vivo、OPPO、百度、华为）的域名安全检测接口，提供统一的调用方式，帮助网站管理员快速了解域名是否被拦截或标记风险。

## ✨ 功能特点

- 支持 **QQ、微信、vivo、OPPO、百度、华为** 六大平台检测
- 每种接口免费调用，频率限制清晰（30次/分钟或3次/小时）
- 返回结构化 JSON 数据，便于集成到脚本或后台系统
- 代码示例丰富，开箱即用

## 📦 支持的 API 列表

| 平台 | 接口地址 | 说明 | 频率限制 |
|------|----------|------|----------|
| QQ（详细） | `https://tmini.net/api/detailed` | 返回标题和描述，状态 `safe` / `blocked` | 30次/分钟 |
| 微信（通用） | `https://tmini.net/api/wechaturl` | 返回 code 码（0正常，-3拦截） | 30次/分钟 |
| vivo | `https://tmini.net/api/vivourl` | 返回风险等级、是否恶意等 | 30次/分钟 |
| OPPO | `https://tmini.net/api/oppourl` | 返回 `block` 布尔值 | 30次/分钟 |
| 百度 | `https://tmini.net/api/baiduurl` | 返回风险等级和具体类型（如博彩） | 30次/分钟 |
| 华为 | `https://tmini.net/api/hwurl` | 返回 `policy` 策略码（0安全，1/2拦截） | 30次/分钟 |
| 微信（备用） | `https://tmini.net/api/wechaturlcheck` | 返回 `status` 和详细描述 | 3次/小时 |
| 聚合（QQ+微信） | `https://www.tmini.net/api/check` | 快速返回 `reCode` 判断拦截 | 30次/分钟 |

> ⚠️ 注意：所有接口均需传入 `url` 参数（主域名），可选 `ckey`（后台KEY，免费用户可忽略）。

## 🚀 快速开始

### 示例请求（使用 cURL）

```bash
# 检测 QQ 拦截
curl "https://tmini.net/api/detailed?url=example.com"

# 检测微信拦截
curl "https://tmini.net/api/wechaturl?url=example.com"

# 检测 vivo
curl "https://tmini.net/api/vivourl?url=example.com"

# 检测 OPPO
curl "https://tmini.net/api/oppourl?url=example.com"

# 检测百度
curl "https://tmini.net/api/baiduurl?url=example.com"

# 检测华为
curl "https://tmini.net/api/hwurl?url=example.com"

# 备用微信检测（每小时3次）
curl "https://tmini.net/api/wechaturlcheck?url=example.com"

# 聚合检测（QQ+微信）
curl "https://www.tmini.net/api/check?url=example.com"

```

```python
import requests

def check_domain(domain, api_type='qq'):
    endpoints = {
        'qq': 'https://tmini.net/api/detailed',
        'wechat': 'https://tmini.net/api/wechaturl',
        'vivo': 'https://tmini.net/api/vivourl',
        'oppo': 'https://tmini.net/api/oppourl',
        'baidu': 'https://tmini.net/api/baiduurl',
        'huawei': 'https://tmini.net/api/hwurl',
        'wechat2': 'https://tmini.net/api/wechaturlcheck',
        'aggregate': 'https://www.tmini.net/api/check'
    }
    url = endpoints.get(api_type)
    if not url:
        return {'error': 'Invalid API type'}
    resp = requests.get(url, params={'url': domain})
    return resp.json()

# 使用示例
result = check_domain('tmini.net', 'qq')
print(result)


```


## 📊 响应示例

```json

// QQ检测（安全）
{
    "code": 200,
    "status": "safe",
    "domain": "tmini.net",
    "data": {
        "title": "未拦截",
        "desc": "该网站目前在腾讯安全中心显示为正常，可以正常访问。"
    }
}

// 微信检测（拦截）
{
    "code": -3,
    "msg": "已被拦截，请联系微信申诉！"
}

// vivo检测（安全）
{
    "success": true,
    "code": 0,
    "message": "判定为安全",
    "data": {
        "url": "tmini.net",
        "risk_level": "安全",
        "is_malicious": false,
        "blocked": false,
        ...
    }
}
```
