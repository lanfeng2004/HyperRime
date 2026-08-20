# HyperRime 鹤翼双拼

> 「鹤」取自小鹤双拼，「翼」既是 hyper 之翼，亦合鹤翼阵之意。

一套以 **小鹤双拼** 为核心、面向 Windows（小狼毫）并兼容 Linux（iBus）的个人 Rime 输入方案。
方案名：`hyper_flypy`（鹤翼双拼）。

本项目融合了多个优秀开源输入法项目的配置与词库，经整理、合并、去冗余后形成一套清爽易维护的个人配置：

- [万象拼音](https://github.com/amzxyz/RIME-LMDG) —— 词库主体与语法大模型（LMDG）
- [雾凇拼音 rime-ice](https://github.com/iDvel/rime-ice) —— 中英文词库、英文方案（melt_eng）
- [薄荷输入法 oh-my-rime](https://github.com/Mintimate/oh-my-rime) —— 方案骨架与整体配置结构
- [98五笔 / 极点五笔](https://github.com/yanhuacuo/98wubi)、[白霜词库](https://github.com/gaboolic/rime-frost) —— 部分词库与反查设计参考

## 方案特性

- **小鹤双拼**键位，支持小鹤音形辅码（形码辅助筛选）
- **中英混合输入**：内嵌 `melt_eng` 英文方案，含英文词库与英文扩展词库
- **拆字 / 笔画反查**：`Ctrl`+`Shift`+`P` 唤起拆字反查，`Ctrl`+`Shift`+`U` 唤起笔画反查
- **错音纠正**：常见易错音自动纠正
- **日期时间 / 农历 / 计算器**：Lua 脚本驱动的快捷输入
- **语法大模型**：可挂载万象语法模型 `amz-v2n3m1-zh-hans.gram` 提升整句输入体验
- **横向候选词 + 每页 9 个候选**

## 安装

1. 安装 [Rime 输入法](https://rime.im/)（Windows 为 [小狼毫 Weasel](https://github.com/rime/weasel)，Linux 为 iBus-Rime 或 Fcitx5-Rime）；
2. 将本仓库克隆或下载到本地 Rime 用户目录：
   - Windows（Weasel）: `%APPDATA%\Rime`
   - Linux（iBus）: `~/.config/ibus/rime`
   - Linux（Fcitx5）: `~/.local/share/fcitx5/rime`
3. 如需整句输入体验，下载万象语法大模型放入用户目录（详见下文「语法大模型」）；
4. 重新部署 Rime（小狼毫：托盘右键 → 「重新部署」）；
5. 使用 `Ctrl` + `~` 切换到「鹤翼双拼」。

## 配置文件说明

| 文件 | 说明 |
|------|------|
| `hyper_flypy.schema.yaml` | 主方案：小鹤双拼、混输、反查、辅码等 |
| `hyper_flypy.dict.yaml` | 中文主词库入口，聚合下列子词库 |
| `melt_eng.schema.yaml` / `melt_eng.dict.yaml` | 英文输入方案与词库入口 |
| `radical_pinyin_flypy.*` | 拆字反查方案与词库 |
| `stroke.*` | 笔画反查方案与词库 |
| `default.yaml` / `default.custom.yaml` | 方案列表、快捷键、候选页数（9 个/页） |
| `weasel.yaml` / `weasel.custom.yaml` | 小狼毫（Windows）前端配置：横向候选、皮肤等 |
| `ibus_rime.yaml` | iBus（Linux）前端配置：横向候选 |
| `symbols.yaml` | 符号表（`/fh` 呼出） |
| `rime.lua` / `lua/` | Lua 脚本集合（日期、农历、计算器、辅码等） |
| `opencc/` | 繁简转换与 Emoji 映射数据 |

> 本机环境相关文件（`installation.yaml`、`user.yaml`、`*.userdb/`）不参与版本管理，首次部署时由 Rime 自动生成。

## 词库结构

中文与英文词库分类管理，统一以 `hyper_flypy` 前缀命名：

```text
dicts/
├── hyper_flypy.base.dict.yaml         # 基础词库（核心高频）
├── hyper_flypy.chars.dict.yaml        # 单字库
├── hyper_flypy.correlation.dict.yaml  # 关联词库（联想）
├── hyper_flypy.compatible.dict.yaml   # 兼容词库（异体/旧字形）
├── hyper_flypy.places.dict.yaml       # 地名库
├── hyper_flypy.ext.dict.yaml          # 扩展词库
├── hyper_flypy.others.dict.yaml       # 其他（纠错等）
├── hyper_flypy.en.dict.yaml           # 英文基础词库
├── hyper_flypy.en_ext.dict.yaml       # 英文扩展词库
├── hyper_flypy.cn_en_flypy.txt        # 中英混输词条（stabledb）
├── custom_simple.dict.yaml            # 自定义词库（个人词条建议加在这里）
└── other_kaomoji.dict.yaml            # 颜文字库
```

个人常用词、专业词汇（如医疗信息化术语）建议添加到 `custom_simple.dict.yaml`，避免修改主词库，方便后续升级合并。

## 语法大模型

整句输入强烈建议挂载万象语法大模型。模型文件体积较大（约 100MB），**不在本仓库内**，请单独下载：

- 下载地址：[RIME-LMDG Releases](https://github.com/amzxyz/RIME-LMDG/releases)
- 文件：`amz-v2n3m1-zh-hans.gram`
- 放置位置：Rime 用户目录根目录（与本仓库 `hyper_flypy.schema.yaml` 同级）
- 重新部署后生效

## 与上游项目的关系

本项目最初基于薄荷输入法（oh-my-rime）完整配置起步，随后：

1. 以雾凇拼音的小鹤双拼方案为骨架定制主方案；
2. 合并雾凇 / 万象 / 薄荷多套词库，去重合并为统一的 `hyper_flypy` 词库系列；
3. 移除未使用的全拼、五笔、九宫格等冗余方案（合计归档约 278MB）；
4. 统一命名、精简目录结构，仅保留单一主方案及其依赖。

感谢上述项目的开源贡献，本项目遵循 [GPL-3.0](LICENSE) 许可证发布。

## 目录速览

```text
HyperRime/
├── hyper_flypy.schema.yaml   # 主方案（鹤翼双拼）
├── hyper_flypy.dict.yaml     # 中文词库入口
├── melt_eng.*                # 英文方案
├── radical_pinyin_flypy.*    # 拆字反查
├── stroke.*                  # 笔画反查
├── dicts/                    # 词库（见上文结构）
├── lua/                      # Lua 脚本
├── opencc/                   # 繁简转换数据
├── default*.yaml             # 全局配置
├── weasel*.yaml              # Windows 前端
└── ibus_rime.yaml            # Linux 前端
```
