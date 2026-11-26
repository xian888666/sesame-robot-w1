# 罗娴 W1 个人任务证据 · Sesame 四足机器人

这是罗娴在赛道 C Sesame 8 自由度四足机器人项目 W1 阶段的个人证据页。目标是记录**本人实际完成的资料核对、产出、Issue、提交和 PR**，不把资料核对写成已经通过的实机测试。

## 快速入口

| 项目 | 入口 |
|---|---|
| 团队成员 | 罗娴（GitHub：`xian888666`） |
| 团队仓库 | [ou02080159/sesame-robot-team](https://github.com/ou02080159/sesame-robot-team) |
| 任务 Issue | [#8 [W1][罗娴] 固件依赖与系统/供电核对](https://github.com/ou02080159/sesame-robot-team/issues/8) |
| 团队 PR | [#10 W1 固件依赖与系统/供电核对](https://github.com/ou02080159/sesame-robot-team/pull/10) |
| 团队个人证据页 | [reports/w1-firmware-power/README.md](https://github.com/ou02080159/sesame-robot-team/blob/w1-firmware-power-evidence/reports/w1-firmware-power/README.md) |
| 固定上游版本 | [dorianborian/sesame-robot@449ad1e](https://github.com/dorianborian/sesame-robot/commit/449ad1e237fa419f575ea1bb183716134e02310c) |
| 团队实现提交 | [`a3918a2`](https://github.com/ou02080159/sesame-robot-team/commit/a3918a2c0d0ed02ceeacd7b26a0705c99e8de728) |
| 团队链接回填提交 | [`ad4ab67`](https://github.com/ou02080159/sesame-robot-team/commit/ad4ab67a739f04a4a3b462e4d1131f22b2a6f793) |

## 我的三项任务

| W1 任务 | 本次完成什么 | 可核验证据 | 还没有做什么 |
|---|---|---|---|
| 整理固件依赖与板型 | 核对 Arduino IDE、ESP32 板卡支持、LOLIN S2 Mini、库、GPIO、OLED 和 PWM 参数 | [固件依赖核对](https://github.com/ou02080159/sesame-robot-team/blob/w1-firmware-power-evidence/docs/firmware-dependencies.md) | 未在本机安装库、编译或烧录 |
| 核对系统图与供电草案 | 核对电池、KCD1、降压 5 V、8 舵机、主控和 OLED 的关系，补公共地、USB 独立路径及分步上电 | [系统架构](https://github.com/ou02080159/sesame-robot-team/blob/w1-firmware-power-evidence/docs/system_architecture.md)、[接线指南](https://github.com/ou02080159/sesame-robot-team/blob/w1-firmware-power-evidence/docs/wiring-guide.md) | 未做实物极性、5 V 输出、电流、压降和温升测量 |
| 完成个人任务证据 | 用本人账号创建 Issue、提交团队分支、PR，并留下个人 README 和操作记录 | 本页、[Issue #8](https://github.com/ou02080159/sesame-robot-team/issues/8)、[PR #10](https://github.com/ou02080159/sesame-robot-team/pull/10) | PR 尚待队友审核，不能自行合并 |

## 核对摘要

### 1. 固件与板型

- Arduino IDE 2.0 或更高版本。
- Espressif ESP32 板卡支持 2.0.0 或更高版本。
- 目标板选择 `LOLIN S2 Mini`。
- 上传速度 `921600`。
- `USB CDC On Boot = Enabled`。
- `Partition Scheme = Default 4MB with spiffs`。
- 外部库：`ESP32Servo 3.0.9`、`Adafruit SSD1306`、`Adafruit GFX Library`。
- 本地头文件：`face-bitmaps.h`、`movement-sequences.h`、`captive-portal.h`。
- 8 路舵机 GPIO：`1, 2, 4, 6, 8, 10, 13, 14`。
- OLED：128×64、I²C 地址 `0x3C`、SDA GPIO `33`、SCL GPIO `35`。
- 串口 `115200`，舵机 PWM 50 Hz，脉宽约 `732–2929 us`。

### 2. 系统与供电

```text
5–12 V 电池
      │
    KCD1 开关
      │
  降压模块，输出调至 5 V
      ├── 8 个 MG90S 舵机的 5 V / GND
      └── LOLIN S2 Mini 的 5 V / GND

S2 GPIO 1/2/4/6/8/10/13/14 ──> 8 路舵机信号
S2 GPIO 33/35 ──> OLED SDA/SCL
所有模块必须共地
USB-C 是独立的另一条供电路径
```

关键结论：

1. 舵机不能从 S2 Mini 的 3V3 引脚直接取电。
2. KCD1 只能切断电池路径，不能切断 USB 路径。
3. 未确认电源隔离前，不建议电池和 USB 同时供电。
4. 上游 5 V/3 A 是参考能力，不是本组实测结果。
5. 上电必须限流、悬空、分步接入，出现堵转、异常温升或压降立即停机。

### 3. 固定版本中注意到的两处文档差异

- 固件 README 的主程序链接比实际路径少一层目录。
- README 要求“取消注释 S2 Mini 配置”，但固定版本主程序已经启用了该配置；W2 应核对只保留一组板型配置。

## 文件与提交

团队 PR 包含：

| 文件 | 变更 |
|---|---|
| `docs/firmware-dependencies.md` | 新增固件依赖、板型和引脚核对 |
| `docs/system_architecture.md` | 更新系统架构和供电草案 |
| `docs/wiring-guide.md` | 新增 S2 Mini 接线与首上电检查表 |
| `docs/upstream.md` | 更新上游资源、开发环境和依赖记录 |
| `reports/w1-firmware-power/README.md` | 新增罗娴个人任务证据 |
| `reports/W1.md` | 更新个人贡献行 |
| `README.md` | 增加文档索引 |
| `docs/requirements.md` | 补充罗娴 GitHub 链接 |

## 资料核对与实机验证的边界

**已经完成：**

- 固定版本源码和说明的文字、路径、参数交叉核对
- 固定版本关键文件的 SHA-256 记录
- 系统关系图、接线表和检查表的资料整理
- 用罗娴本人 GitHub 账号建立 Issue、分支、提交和 PR

**仍待本人/小组实机完成：**

- Arduino IDE 和库的实际安装版本
- 完整编译日志和 USB 烧录
- 串口输出、Wi-Fi 配网和 OLED 显示
- 电池、KCD1、降压模块的实物极性和绝缘
- 降压输出 5 V 的准确度
- 单舵机与整机电流、压降、温升
- 舵机实际方向、关节顺序和机械限位
- G0 安全培训和现场负责人确认

## 自己核对的操作方法

1. 打开 [Issue #8](https://github.com/ou02080159/sesame-robot-team/issues/8)，确认任务描述、指派人和完成评论。
2. 打开 [PR #10](https://github.com/ou02080159/sesame-robot-team/pull/10)，确认 base 是 `main`、head 是 `w1-firmware-power-evidence`。
3. 在 PR 的 `Files changed` 中核对上表 8 个文件。
4. 打开 `docs/firmware-dependencies.md`，对照固定上游源码第 25–40、100–110、651–743 行。
5. 打开 `docs/system_architecture.md` 和 `docs/wiring-guide.md`，确认“待实测”没有被改成“已完成”。
6. 在仓库的 `Commits` 中核对 `a3918a2`（提交内容）和 `ad4ab67`（回填 PR 链接）。
7. PR 合并后，保留本页的 Issue、PR 和最终合并提交链接。

## AI 使用声明

Codex 协助读取固定版本源码、提取依赖和接线关系、起草文档与 GitHub 操作步骤。GitHub 日常操作使用罗娴本人账号 `xian888666`。AI 辅助不冒充实机编译、烧录或测量结果。

上游项目：[Dorian Todd / dorianborian/sesame-robot](https://github.com/dorianborian/sesame-robot)，许可证 [Apache-2.0](https://github.com/dorianborian/sesame-robot/blob/449ad1e237fa419f575ea1bb183716134e02310c/LICENSE)。
