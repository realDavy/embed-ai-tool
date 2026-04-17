# 嵌入式代理技能仓库

可复用的 Codex skill 集合，服务嵌入式 MCU 固件开发场景。覆盖工程识别、多工具链构建、烧录、调试、串口监视、外设驱动适配等主干流程，支持 Linux、macOS、Windows 三平台。

## 快速开始

### 前置条件

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI 已安装并完成认证
- Git

### 安装所有 skill

```bash
git clone https://github.com/LeoKemp223/em_skill.git
cp -r em_skill/skills/*  your-project/.claude/skills/
cp -r em_skill/shared    your-project/.claude/skills/shared
```

### 安装指定 skill

只复制你需要的模块：

```bash
git clone https://github.com/LeoKemp223/em_skill.git

# 示例：只安装 build-cmake 和 flash-openocd
cp -r em_skill/skills/build-cmake   your-project/.claude/skills/
cp -r em_skill/skills/flash-openocd your-project/.claude/skills/
cp -r em_skill/shared               your-project/.claude/skills/shared
```

### 工具路径配置

部分 skill 依赖外部工具（OpenOCD、Keil、arm-none-eabi-gcc 等），可通过内置 CLI 配置路径：

```bash
# 设置工具路径（工作区级别）
python3 scripts/em_config.py set openocd /usr/bin/openocd

# 设置全局工具路径
python3 scripts/em_config.py set uv4 "C:\Keil_v5\UV4\UV4.exe" --global

# 查看已配置的工具
python3 scripts/em_config.py list

# 查看配置文件位置
python3 scripts/em_config.py path
```

## 技能列表

| 技能 | 说明 |
|------|------|
| `build-cmake` | 配置并构建基于 CMake 的 MCU 固件工程 |
| `build-keil` | 配置并构建基于 Keil MDK 的固件工程 |
| `build-iar` | 配置并构建基于 IAR EWARM 的固件工程 |
| `build-platformio` | 配置并构建基于 PlatformIO 的固件工程 |
| `flash-openocd` | 通过 OpenOCD 烧录 ELF/HEX/BIN 产物 |
| `debug-gdb-openocd` | 通过 OpenOCD 附着 GDB，支持下载后调试、仅附着和崩溃现场排查 |
| `serial-monitor` | 选择串口并抓取运行日志 |
| `peripheral-driver` | 搜索并适配开源 BSP 外设驱动到目标工程 |
| `stm32-hal-development` | STM32 HAL 库开发指导与最佳实践 |

## 仓库结构

```text
.
├── skills/                     # 技能模块
│   ├── build-cmake/            # CMake 构建
│   ├── build-keil/             # Keil 构建
│   ├── build-iar/              # IAR 构建
│   ├── build-platformio/       # PlatformIO 构建
│   ├── flash-openocd/          # OpenOCD 烧录
│   ├── debug-gdb-openocd/      # GDB 调试
│   ├── serial-monitor/         # 串口监视
│   ├── peripheral-driver/      # 外设驱动适配
│   └── stm32-hal-development/  # STM32 HAL 开发
├── shared/                     # 共享约定
│   ├── contracts.md            # 上下文交接合约
│   ├── failure-taxonomy.md     # 失败分类
│   ├── platform-compatibility.md
│   └── references/
├── templates/                  # Skill 模板
│   └── skill-template/
└── scripts/
    ├── validate_repo.py        # 结构校验
    └── em_config.py            # 工具路径配置 CLI
```

## 共享约定

所有 skill 围绕同一套核心上下文进行输入与输出：

- **Project Profile** — 工作区、目标、构建系统、探针和产物的标准化元数据
- **Skill Handoff Contract** — 下游 skill 可直接继承的上下文
- **Command Outcome Schema** — 成功、失败或阻塞结果的统一格式
- **Failure Taxonomy** — 标准失败分类及推荐后续动作

详见 [shared/contracts.md](shared/contracts.md) 和 [shared/failure-taxonomy.md](shared/failure-taxonomy.md)。

## 校验

修改后执行结构校验：

```bash
python3 scripts/validate_repo.py
```

校验器会检查所有 skill 必需文件、frontmatter 和章节标题是否齐全。

## 贡献

参见 [CONTRIBUTING.md](CONTRIBUTING.md)。新 skill 请基于 [templates/skill-template/](templates/skill-template/) 模板创建。

## 后续扩展

仓库结构已为后续扩展预留空间，例如 `flash-jlink`、`flash-pyocd`、`vendor-tools`、`fault-triage`、`rtos-debug`、`trace-analysis`，无需改动核心约定。
