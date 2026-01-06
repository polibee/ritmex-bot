# StandX 做市积分策略使用教程（新手版）

本教程用于帮助你快速上手 StandX 做市积分策略（Maker Points）。无需交易经验，照着步骤做即可。

---

## 1. 准备工作

### 1.1 安装 Bun
本项目使用 Bun 运行。

如果你还没有安装 Bun，请先参考官方文档完成安装：
- https://bun.sh/

安装完成后，进入项目目录执行：
```bash
bun install
```

### 1.2 获取 StandX 登录凭证（Token）
策略需要 StandX 的登录 token 才能下单。

获取方式：
1. 打开 https://standx.ritmex.one/
2. 连接钱包
3. 点击“登录”
4. 导出登录信息（里面会包含 TOKEN 以及生成的代理钱包私钥）

请妥善保存，不要分享给他人。

---

## 2. 配置环境变量

在项目根目录新建 `.env` 文件（或修改已有 `.env`），填入以下配置：

```bash
EXCHANGE=standx
STANDX_REQUEST_PRIVATE_KEY=你的代理钱包私钥
STANDX_TOKEN=你的token
STANDX_SYMBOL=BTC-USD

MAKER_POINTS_ORDER_AMOUNT=0.01
MAKER_POINTS_CLOSE_THRESHOLD=0.1
MAKER_POINTS_STOP_LOSS_USD=0
MAKER_POINTS_MIN_REPRICE_BPS=3

MAKER_POINTS_BAND_0_10=true
MAKER_POINTS_BAND_10_30=true
MAKER_POINTS_BAND_30_100=true
```

### 配置说明（新手可直接照抄）
- `STANDX_TOKEN`: 登录后导出的 token（必须）
- `STANDX_SYMBOL`: 交易对（默认 `BTC-USD`）
- `MAKER_POINTS_ORDER_AMOUNT`: 每一笔挂单数量
- `MAKER_POINTS_CLOSE_THRESHOLD`: 持仓达到该数值进入平仓模式（0 表示不自动平仓）
- `MAKER_POINTS_STOP_LOSS_USD`: 亏损超过该值时市价平仓（0 表示关闭）
- `MAKER_POINTS_MIN_REPRICE_BPS`: 盘口变动达到多少 bps 才会重算并撤单（默认 3）
- `MAKER_POINTS_BAND_0_10` / `10_30` / `30_100`: 三个档位开关

---

## 3. 启动策略

### 3.1 普通启动
```bash
bun run index.ts --strategy maker-points --exchange standx
```

### 3.2 使用 PM2 后台运行（推荐）
```bash
bun run pm2:start:maker-points
```

PM2 会自动重启策略，适合长期运行。

---

## 4. 运行后你会看到什么

启动后会显示仪表盘，包含：
- 当前交易对与盘口
- 当前仓位与浮动盈亏
- 目标挂单列表
- 已挂单列表
- Binance 深度失衡状态

---

## 5. 新手常见问题

### Q1：为什么会撤单重挂？
策略只有在盘口变化超过 `MAKER_POINTS_MIN_REPRICE_BPS` 或 Binance 深度状态变化时才会重算挂单。

### Q2：担心平掉手动持仓怎么办？
把 `MAKER_POINTS_CLOSE_THRESHOLD` 设为 0 或者设成大于你的持仓即可。

### Q3：我只想挂某个档位？
把不需要的档位开关设为 `false` 即可。

---

## 6. 注意事项

1. 请确保钱包里有足够的保证金/资金。
2. 不要泄露 `STANDX_TOKEN`。
3. 初次使用请先用小仓位测试。

---

如果你需要我帮你定制参数或排查问题，直接把日志贴给我即可。
