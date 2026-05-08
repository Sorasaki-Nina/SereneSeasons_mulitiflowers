# Serene Seasons Multiflowers

中文 | [English](#english)

这是基于 [Glitchfiend/SereneSeasons](https://github.com/Glitchfiend/SereneSeasons) 的修改版本，目标环境为 **Minecraft 1.21.1 + NeoForge**。原版 Serene Seasons 为 Minecraft 添加季节、颜色变化、温度变化以及季节性天气等机制。本 fork 在不改动冬季积雪厚度机制的前提下，新增了“春季融雪后生成多种花卉”的玩法。

> 原项目版权归 Glitchfiend Team 所有。本仓库仅记录针对融雪花卉生成机制的修改。

## 主要改动

- 春季自然融雪后，有概率在原雪层位置生成花卉。
- 花卉池支持单层花和双层花。
- 双层花会通过原版 `DoublePlantBlock.placeAt(...)` 正确生成上下两格，避免只生成半截。
- 默认降低 `minecraft:dandelion` 的权重，使小黄花概率接近其他常见花。
- 默认加入低概率特殊花：
  - `minecraft:pitcher_plant`
  - `minecraft:wither_rose`
- 生成概率、重试次数、花卉种类和权重都可在配置文件中调整。
- 本修改不调整冬季降雪、积雪层数或 `snowAccumulationHeight` 逻辑。

## 默认花卉权重

| 花卉 ID | 默认权重 |
| --- | ---: |
| `minecraft:dandelion` | 8 |
| `minecraft:poppy` | 8 |
| `minecraft:azure_bluet` | 8 |
| `minecraft:oxeye_daisy` | 8 |
| `minecraft:cornflower` | 8 |
| `minecraft:allium` | 8 |
| `minecraft:red_tulip` | 8 |
| `minecraft:orange_tulip` | 8 |
| `minecraft:white_tulip` | 8 |
| `minecraft:pink_tulip` | 8 |
| `minecraft:lily_of_the_valley` | 6 |
| `minecraft:blue_orchid` | 4 |
| `minecraft:sunflower` | 4 |
| `minecraft:lilac` | 4 |
| `minecraft:rose_bush` | 4 |
| `minecraft:peony` | 4 |
| `minecraft:pitcher_plant` | 1 |
| `minecraft:wither_rose` | 1 |

权重是相对概率。比如权重为 8 的花会比权重为 1 的花更常见，但实际是否生成还会受到 `chance`、当前位置能否放置花、上方空间是否足够等条件影响。

## 配置

首次运行后，配置会出现在 Serene Seasons 的季节配置文件中，通常路径类似：

```text
config/sereneseasons/seasons.toml
```

新增配置段：

```toml
[flower_growth_after_snow_melt]
enabled = true
chance = 0.08
placement_attempts = 3

[[flower_growth_after_snow_melt.flowers]]
block = "minecraft:dandelion"
weight = 8

[[flower_growth_after_snow_melt.flowers]]
block = "minecraft:pitcher_plant"
weight = 1

[[flower_growth_after_snow_melt.flowers]]
block = "minecraft:wither_rose"
weight = 1
```

字段说明：

| 字段 | 说明 |
| --- | --- |
| `enabled` | 是否启用春季融雪后生成花卉 |
| `chance` | 每个自然融化雪层尝试生成花的概率，`0.08` 表示 8% |
| `placement_attempts` | 抽到的花无法放置时，最多重新抽取几次 |
| `flowers.block` | 花卉方块 ID |
| `flowers.weight` | 花卉的相对权重，必须大于 0 |

## 放置规则

花卉只会在满足以下条件时生成：

- 当前季节是春季。
- 雪是由 Serene Seasons 的自然融雪逻辑融化。
- 当前维度启用了季节系统。
- 当前生物群系没有被 Serene Seasons 黑名单排除。
- 融雪位置为空气。
- 花卉可以在下方方块上生存。
- 如果是双层花，融雪位置上方也必须为空气。

如果配置中写入不存在的方块 ID，或者写入不能在当前位置生存的植物，代码会跳过该项，不会因为单个错误配置直接崩溃。

## 构建 NeoForge 1.21.1

Windows:

```powershell
.\gradlew.bat :NeoForge:build
```

Linux/macOS:

```bash
./gradlew :NeoForge:build
```

构建完成后，NeoForge jar 会输出到：

```text
neoforge/build/libs/
```

## 测试建议

在游戏中可以这样验证：

```text
/weather rain
/gamerule randomTickSpeed 20
```

然后切换或等待到春季，观察自然融雪后是否生成多种花卉。双层花应完整生成上下两格，`pitcher_plant` 和 `wither_rose` 应明显低概率出现。

## 原项目

- GitHub: https://github.com/Glitchfiend/SereneSeasons
- CurseForge: https://www.curseforge.com/minecraft/mc-mods/serene-seasons
- Discord: https://discord.gg/GyyzU6T

-----------------

Copyright 2024 Glitchfiend. All rights reserved.

## English

[中文](#serene-seasons-multiflowers) | English

This is a modified fork of [Glitchfiend/SereneSeasons](https://github.com/Glitchfiend/SereneSeasons), targeting **Minecraft 1.21.1 + NeoForge**. The original Serene Seasons mod adds seasons, color changes, temperature shifts, seasonal weather, and related systems to Minecraft. This fork adds a spring snow-melt flower growth mechanic without changing winter snow accumulation depth.

> The original project belongs to the Glitchfiend Team. This repository documents the snow-melt flower generation changes only.

## Main Changes

- When snow naturally melts during spring, flowers may grow where the snow layer was.
- The flower pool supports both one-block flowers and double-height flowers.
- Double-height flowers are placed through vanilla `DoublePlantBlock.placeAt(...)`, so both halves are generated correctly.
- The default `minecraft:dandelion` weight has been reduced so yellow flowers appear at a similar rate to other common flowers.
- Low-probability special flowers are included by default:
  - `minecraft:pitcher_plant`
  - `minecraft:wither_rose`
- Spawn chance, placement retries, flower IDs, and weights are configurable.
- This fork does not change winter snowfall, snow layer depth, or `snowAccumulationHeight` behavior.

## Default Flower Weights

| Flower ID | Default Weight |
| --- | ---: |
| `minecraft:dandelion` | 8 |
| `minecraft:poppy` | 8 |
| `minecraft:azure_bluet` | 8 |
| `minecraft:oxeye_daisy` | 8 |
| `minecraft:cornflower` | 8 |
| `minecraft:allium` | 8 |
| `minecraft:red_tulip` | 8 |
| `minecraft:orange_tulip` | 8 |
| `minecraft:white_tulip` | 8 |
| `minecraft:pink_tulip` | 8 |
| `minecraft:lily_of_the_valley` | 6 |
| `minecraft:blue_orchid` | 4 |
| `minecraft:sunflower` | 4 |
| `minecraft:lilac` | 4 |
| `minecraft:rose_bush` | 4 |
| `minecraft:peony` | 4 |
| `minecraft:pitcher_plant` | 1 |
| `minecraft:wither_rose` | 1 |

Weights are relative probabilities. A flower with weight 8 is more likely to be selected than a flower with weight 1, but actual placement also depends on `chance`, valid ground, and available space.

## Configuration

After the first run, the new settings are written to the Serene Seasons season config, usually at:

```text
config/sereneseasons/seasons.toml
```

New section:

```toml
[flower_growth_after_snow_melt]
enabled = true
chance = 0.08
placement_attempts = 3

[[flower_growth_after_snow_melt.flowers]]
block = "minecraft:dandelion"
weight = 8

[[flower_growth_after_snow_melt.flowers]]
block = "minecraft:pitcher_plant"
weight = 1

[[flower_growth_after_snow_melt.flowers]]
block = "minecraft:wither_rose"
weight = 1
```

Field reference:

| Field | Description |
| --- | --- |
| `enabled` | Enables flower growth after spring snow melt |
| `chance` | Chance for each naturally melted snow layer to attempt flower growth; `0.08` means 8% |
| `placement_attempts` | How many weighted flower picks to retry when the selected flower cannot be placed |
| `flowers.block` | Flower block ID |
| `flowers.weight` | Relative selection weight; must be greater than 0 |

## Placement Rules

Flowers are generated only when all of the following conditions are met:

- The current season is spring.
- The snow was melted by Serene Seasons' natural melt logic.
- Seasons are enabled in the current dimension.
- The biome is not blacklisted by Serene Seasons.
- The snow-melt position is air.
- The selected flower can survive on the block below.
- For double-height flowers, the block above must also be air.

Invalid block IDs or plants that cannot survive at the chosen position are skipped instead of crashing the game.

## Build NeoForge 1.21.1

Windows:

```powershell
.\gradlew.bat :NeoForge:build
```

Linux/macOS:

```bash
./gradlew :NeoForge:build
```

The NeoForge jar is written to:

```text
neoforge/build/libs/
```

## Testing

In game, you can verify with:

```text
/weather rain
/gamerule randomTickSpeed 20
```

Then switch to or wait for spring and watch natural snow melt. You should see multiple flower types. Double-height flowers should place both halves, while `pitcher_plant` and `wither_rose` should appear rarely.

## Original Project

- GitHub: https://github.com/Glitchfiend/SereneSeasons
- CurseForge: https://www.curseforge.com/minecraft/mc-mods/serene-seasons
- Discord: https://discord.gg/GyyzU6T

-----------------

Copyright 2024 Glitchfiend. All rights reserved.
