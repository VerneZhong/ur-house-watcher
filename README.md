# UR House Watcher

自动监控 UR 大島四丁目 / 大島六丁目空房，并在发现符合条件的新房源时发送 Telegram 通知。

## 监控条件

- 大島四丁目：2LDK / 3DK，面积 >= 50㎡
- 大島六丁目：2LDK / 3DK，面积 >= 50㎡
- 日本时间 07:00–23:50，每 10 分钟检查一次
- 00:00–06:59 不检查
- 也可以在 Actions 页面手动运行

## 通知

发现新房源时：

- Telegram 发送房源信息
- 消息附带「🏠 立即查看・仮申込」按钮，直接打开 UR 房源页面
- 同时创建 GitHub Issue 作为记录

Telegram 需要在仓库 `Settings → Secrets and variables → Actions` 中配置：

- `TELEGRAM_BOT_TOKEN`
- `TELEGRAM_CHAT_ID`

## 安全设计

- 只读取 UR 官方当前空房 API 的真实房源数据
- 仅在房型和面积满足条件时记录
- UR API 请求失败时不会覆盖上一次正常 state，避免误判为“0 套”
- `state.json` 用于防止同一房源重复通知

## 文件

- `check_ur.py`：监控逻辑
- `config.yml`：监控目标与筛选条件
- `state.json`：上一次已知房源状态
- `.github/workflows/ur-house-watcher.yml`：GitHub Actions 定时任务

> UR 房源通常是先着順。收到通知后请尽快打开 UR 官网确认并自行完成仮申込。
