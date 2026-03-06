---
name: wecom-calendar
description: 企业微信日历管理 - 创建/查询/更新/删除日程
metadata: {"clawdbot":{"emoji":"📅","requires":{"bins":["node","curl","jq"],"env":["WECOM_CORP_ID","WECOM_AGENT_ID","WECOM_AGENT_SECRET"]}}}
---

# WeCom Calendar - 企业微信日历管理

管理企业微信日程的完整工具，支持创建、查询、更新、删除日程。

## 功能

### 1. 创建日程
```bash
node calendar.mjs add --summary "需求评审会议" --start 1709280000 --end 1709283600 --attendees "zhangsan,lisi"
```

### 2. 获取日程列表
```bash
node calendar.mjs list --cal_id "wcjgewCwAA..."
```

### 3. 获取日程详情
```bash
node calendar.mjs get --schedule_id "xxx"
```

### 4. 更新日程
```bash
node calendar.mjs update --schedule_id "xxx" --summary "更新后的标题"
```

### 5. 取消日程
```bash
node calendar.mjs cancel --schedule_id "xxx"
```

### 6. 创建日历
```bash
node calendar.mjs create-cal --summary "团队日历" --color "#4285F4"
```

## 安装依赖

```bash
cd /root/.openclaw/workspace/skills/wecom-calendar
npm install axios
```

## 配置

### 环境变量

在 `~/.bashrc` 或 `.env` 文件中配置：

```bash
export WECOM_CORP_ID="你的企业 ID"
export WECOM_AGENT_ID="1000001"
export WECOM_AGENT_SECRET="应用 Secret"
```

### 或使用配置文件

创建 `.env` 文件：

```
WECOM_CORP_ID=wwxxx
WECOM_AGENT_ID=1000001
WECOM_AGENT_SECRET=xxx
```

## 使用示例

### 创建一次性会议

```bash
node calendar.mjs add \
  --summary "项目启动会" \
  --description "讨论项目计划和分工" \
  --start 1709280000 \
  --end 1709283600 \
  --location "10 楼会议室" \
  --attendees "zhangsan,lisi,wangwu"
```

### 创建重复会议（每周一）

```bash
node calendar.mjs add \
  --summary "周例会" \
  --start 1709280000 \
  --end 1709283600 \
  --repeat-type 1 \
  --repeat-interval 1 \
  --repeat-day-of-week 1 \
  --attendees "team_all"
```

### 创建全天事件

```bash
node calendar.mjs add \
  --summary "公司年会" \
  --start 1709251200 \
  --end 1709337600 \
  --whole-day 1
```

### 添加提醒

```bash
node calendar.mjs add \
  --summary "重要会议" \
  --start 1709280000 \
  --remind 1 \
  --remind-before 3600  # 提前 1 小时提醒
```

## 参数说明

### 基本参数

| 参数 | 说明 | 必填 |
|------|------|------|
| `--summary` | 日程标题 | 否（默认"新建事件"） |
| `--description` | 日程描述 | 否 |
| `--start` | 开始时间（Unix 时间戳） | 是 |
| `--end` | 结束时间（Unix 时间戳） | 是 |
| `--location` | 日程地点 | 否 |
| `--attendees` | 参与者 ID 列表（逗号分隔） | 否 |
| `--admins` | 管理员 ID 列表（逗号分隔） | 否 |
| `--cal_id` | 日历 ID | 否（使用默认日历） |

### 提醒参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `--remind` | 是否需要提醒 (0/1) | 0 |
| `--remind-before` | 提前多少秒提醒 | 300 |
| `--remind-times` | 多个提醒时间（逗号分隔） | - |

### 重复参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `--repeat` | 是否重复 (0/1) | 0 |
| `--repeat-type` | 重复类型：0=每日，1=每周，2=每月，5=每年，7=工作日 | 0 |
| `--repeat-interval` | 重复间隔 | 1 |
| `--repeat-until` | 重复结束时间戳 | 0（一直重复） |
| `--repeat-day-of-week` | 每周周几 (1-7) | - |
| `--repeat-day-of-month` | 每月哪几天 (1-31) | - |
| `--timezone` | 时区（UTC 偏移） | 8（北京） |

## 支持的提醒时间

- `0` - 事件开始时
- `300` - 事件开始前 5 分钟
- `900` - 事件开始前 15 分钟
- `3600` - 事件开始前 1 小时
- `86400` - 事件开始前 1 天
- `32400` - 事件开始当天 09:00（仅全天事件）
- `-172800` - 事件开始前两天（仅全天事件）
- `-604800` - 事件开始前 1 周（仅全天事件）

## 返回结果

```json
{
  "errcode": 0,
  "errmsg": "ok",
  "schedule_id": "xxx"
}
```

## 错误码

| 错误码 | 说明 |
|--------|------|
| 0 | 成功 |
| 40003 | 无效的企业 ID |
| 40014 | 无效的 access_token |
| 40035 | 参数格式错误 |
| 60205 | 日程不存在 |
| 60206 | 无权限操作该日程 |

## 注意事项

- 时间戳使用 Unix 时间戳（秒）
- 参与者最多支持 1000 人
- 管理员最多支持 3 人
- 重复日程的时间跨度不能超过 1 年
- 需要企业微信应用权限

---

**位置**: `/root/.openclaw/workspace/skills/wecom-calendar/`
**API 文档**: https://developer.work.weixin.qq.com/document/path/93703
