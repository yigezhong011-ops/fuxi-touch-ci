# FuxiFtsTouch

Start with the [adaptation procedure](/porting/input) for board inputs, source changes, build integration and verification order.

ST FingerTip FTM5 touch protocol and contact tracking, written for the Xiaomi 13 (fuxi, SM8550).

[Source repository](https://github.com/OddlyLabs/FuxiFtsTouch).

The controller keeps its firmware in its own flash: the host resets it, waits for its ready event, starts sensing and
drains its event FIFO. Callers provide the transport (`struct fts_bus`: full-duplex transfer, delay, reset line); on
fuxi the board probe passes GENI SPI transfers from [QcomGeniSpi](/drivers/qcom-geni-spi).

| File | Purpose |
| --- | --- |
| `FtsProtocol.c` | Framing, register access, FIFO reads, reset with the ready wait and the 4-wire SPI fallback, sensing start |
| `FtsTouchCore.c` | Contact slots by controller tracking id, error recovery flags, board coordinate transform |

## Board integration

The fuxi device repository [OddlyLabs/unixnu-fuxi](https://github.com/OddlyLabs/unixnu-fuxi) holds the board probe
(`package/FuxiTouchProbe.cpp`, staged by the boot argument `fuxi-touch=<n>`), the board constants with their sources and
the archive recipe `scripts/build-fuxi-touch.py`.

```sh
./unixnu build kernel -D ../unixnu-fuxi
```

[Component catalog](/reference/components)
