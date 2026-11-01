# FuxiFtsTouch

板级输入、源码修改、构建集成和验证顺序，先看[适配流程](/zh/porting/input)。

ST FingerTip FTM5 触控协议与触点跟踪，为小米 13（fuxi，SM8550）编写。

[源码仓库](https://github.com/OddlyLabs/FuxiFtsTouch)。

控制器的固件在芯片自带的 flash 里：主机只需要复位它、等它的就绪事件、开始扫描，然后读事件 FIFO。传输由调用方提供（`struct fts_bus`：全双工收发、延时、复位脚）。在 fuxi 上，板级探针用 [QcomGeniSpi](/zh/drivers/qcom-geni-spi) 的 GENI SPI 传输。

| 文件 | 作用 |
| --- | --- |
| `FtsProtocol.c` | 组帧、寄存器读写、读 FIFO、复位并等待就绪（含 4 线 SPI 回退）、开始扫描 |
| `FtsTouchCore.c` | 按控制器跟踪 ID 管理触点槽位、错误恢复标志、板级坐标变换 |

## 板级集成

fuxi 设备仓库 [OddlyLabs/unixnu-fuxi](https://github.com/OddlyLabs/unixnu-fuxi) 里有板级探针（`package/FuxiTouchProbe.cpp`，用启动参数 `fuxi-touch=<n>` 分阶段打开）、带出处的板级常量，以及归档脚本 `scripts/build-fuxi-touch.py`。

```sh
./unixnu build kernel -D ../unixnu-fuxi
```

[组件目录](/zh/reference/components)
