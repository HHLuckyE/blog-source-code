# 游戏封面（横屏 16:9）

`data/games.ts` 里每条的 `cover` 是 `assets/games/<id>.webp` 形式的相对路径，
本目录同步到代码仓的 `src/assets/games/`，由 Astro 图片管线处理。

## 现状

21 条游戏的封面**已经抓好并落盘**：统一 960×540 WebP，每张 15–120KB。
缺图时卡片会自动退化为主色渐变横幅 + 居中 Icon，所以换图、删图都不会破版。

| 游戏 | 文件名 | 尺寸 | 体积 |
| --- | --- | --- | --- |
| Garry's Mod | `garrys-mod.webp` | 960×540 | 16KB |
| Stellaris | `stellaris.webp` | 960×540 | 24KB |
| 爱上火车-Last Run!!- | `maitetsu-last-run.webp` | 960×540 | 113KB |
| 爱上火车-Pure Station!!- | `maitetsu-pure-station.webp` | 960×540 | 95KB |
| 彼方的她-Aliya | `aliya.webp` | 960×540 | 39KB |
| 不/存在的你，和我 | `noexistence-of-you-and-me.webp` | 960×540 | 107KB |
| 东方官方作品集 | `touhou-official-collection.webp` | 960×540 | 45KB |
| 东方夜雀食堂 | `touhou-mystias-izakaya.webp` | 960×540 | 117KB |
| 黑神话：悟空 | `black-myth-wukong.webp` | 960×540 | 54KB |
| 饿殍：明末千里行 | `efu-mingmo-qianlixing.webp` | 960×540 | 59KB |
| I wanna be the creator | `i-wanna-be-the-creator.webp` | 960×540 | 15KB |
| 犹格索托斯的庭院 | `yog-sothoths-yard.webp` | 960×540 | 49KB |
| ATRI -My Dear Moments | `atri-my-dear-moments.webp` | 960×540 | 91KB |
| Not Another Needle Game | `not-another-needle-game.webp` | 960×540 | 26KB |
| Sid Meier's Civilization VI | `civilization-vi.webp` | 960×540 | 46KB |
| Plague Inc:Evolved | `plague-inc-evolved.webp` | 960×540 | 64KB |
| CS 1.6 | `counter-strike-1-6.webp` | 960×540 | 51KB |
| CFM | `cfm.webp` | 960×540 | 64KB |
| 不思议迷宫 | `gumballs-and-dungeons.webp` | 960×540 | 80KB |
| 碧蓝航线 | `azur-lane.webp` | 960×540 | 80KB |
| 地下城堡2 | `dungeon-castle-2.webp` | 960×540 | 30KB |

## 图源与再抓取

- 图源清单：[scripts/cover-sources.json](../scripts/cover-sources.json)，每条记录了
  实际采用的原始地址与原图尺寸。
- 抓取脚本：[scripts/fetch-covers.mjs](../scripts/fetch-covers.mjs)
  - Steam 游戏取官方 `capsule_616x353.jpg` 胶囊封面（个别缺失时退回头图或截图）；
  - 手游与同人作取官网 KV、发行商 CDN 或商店页截图：CFM 用官网主视觉，
    碧蓝航线 / 地下城堡2 用 TapTap 官方图，不思议迷宫用雷霆官网 KV，
    Not Another Needle Game 用 Delicious Fruit 的游戏截图，
    爱上火车-Pure Station!!- 用官网 KV。
- 重新抓取：`node scripts/fetch-covers.mjs`（`--dry` 只验证不写盘，
  `--only=id1,id2` 只处理指定条目，`--manifest` 更新图源清单）。
- 抓取后核验：`node scripts/verify-covers.mjs`，逐条比对 `data/games.ts` 的 `cover`
  路径与磁盘文件、尺寸和体积，并拼出总览图 `scripts/cover-contact-sheet.png`。

## 注意

- 写入本目录需要**一次非受限执行**：当前沙箱的写入授权只覆盖工作区根目录与
  `scripts/`，`assets/` 等挂载子目录没有写权限，受限模式下 Node 写入会报 `EPERM`。
- 换图请保持文件名等于 `data/games.ts` 里的 `id`；换格式（jpg/png/avif）时记得
  同步改数据文件里的后缀。
- 这些封面来自各游戏官方商店页 / 官网的宣传素材，仅用于本站的个人收藏展示；
  想换成自截图直接覆盖同名文件即可。
