# 📅 WeCom Calendar - 企业微信日历管理

企业微信日程管理工具，支持创建、查询、更新、删除日程。

## 安装

```bash
cd /root/.openclaw/workspace/skills/wecom-calendar
npm install
```

## 配置

创建 `.env` 文件：

```bash
WECOM_CORP_ID=wwxxx
WECOM_AGENT_ID=1000001
WECOM_AGENT_SECRET=xxx
```

## 使用

```bash
# 创建日程
node calendar.mjs add --summary "会议" --start 1709280000 --end 1709283600

# 获取日程列表
node calendar.mjs list --cal_id "xxx"

# 获取日程详情
node calendar.mjs get --schedule_id "xxx"

# 更新日程
node calendar.mjs update --schedule_id "xxx" --summary "新标题"

# 取消日程
node calendar.mjs cancel --schedule_id "xxx"

# 创建日历
node calendar.mjs create-cal --summary "团队日历" --color "#4285F4"
```

## 文档

查看 [SKILL.md](./SKILL.md) 获取完整使用说明。

---

**API 文档**: https://developer.work.weixin.qq.com/document/path/93703
