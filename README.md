# Viora-plugins 官方插件仓库

本仓库是 **Viora 插件市场的官方数据源**。Viora 客户端的插件市场会从这里拉取插件目录(`registry.json`)与插件包(`packages/*.zip`),实现**插件与客户端完全分离**:客户端本体不内置任何插件,安装区从空开始,全部插件由用户在市场里按需下载,下载来源即本仓库(GitHub)。

当前本仓库托管 **55 个官方系统预设**(风格化插件)。

---

## 目录结构

```
Viora-plugins/
├── registry.json        # 插件总目录(市场的唯一数据源,含全部插件元数据)
├── packages/            # 每个插件一个完整安装包(zip = plugin.json + 插件 DLL)
│   ├── builtin.viora.mosaic.zip
│   ├── builtin.viora.oil-painting.zip
│   └── ...
└── README.md
```

## 目录条目格式(`registry.json` 的 `presets` 数组)

```json
{
  "pluginId": "builtin.viora.watercolor",
  "presetId": "builtin.watercolor",
  "name": { "zh": "水彩画", "en": "Watercolor" },
  "author": "Viora Team",
  "category": "Style.Cat.Watercolor",
  "version": "1.0.0",
  "package": "packages/builtin.viora.watercolor.zip",
  "repository": "https://github.com/yourname/your-plugin",
  "tags": ["水彩"],
  "description": { "zh": "……", "en": "……" }
}
```

| 字段 | 说明 |
|---|---|
| `pluginId` | 宿主插件 id = 安装区文件夹名,格式 `builtin.viora.<短id>`(官方)或 `<作者名>.<插件名>`(社区)。**发布后不可更改** |
| `presetId` | 预设 id(风格化面板中的唯一键),通常为 `builtin.<短id>` |
| `name` | 显示名,至少提供中文;建议同时提供英文 |
| `author` | 作者名,将与 GitHub 账号一致;市场卡片上点击作者名跳转 `repository` |
| `category` | 从 20 个官方大类中选一(见下表) |
| `version` | 语义化版本 `主.次.修订` |
| `package` | 安装包路径 `packages/<pluginId>.zip` |
| `repository` | 可选。插件仓库地址(作者自行配置);官方条目缺省指向本仓库 `packages/` 内对应的包 |
| `tags` | 可选。1–2 个作者自配标签,与分类一起展示并参与搜索 |
| `description` | 一段话说明插件功能与适用场景(中/英) |

## 20 个官方大类

`Style.Cat.Anime`(动漫)、`Style.Cat.Illustration`(插画)、`Style.Cat.OilPainting`(油画)、`Style.Cat.Watercolor`(水彩)、`Style.Cat.Sketch`(素描)、`Style.Cat.Printmaking`(版画)、`Style.Cat.Pixel`(像素艺术)、`Style.Cat.RetroFilm`(复古胶片)、`Style.Cat.ThreeD`(3D 立体)、`Style.Cat.Cyberpunk`(赛博朋克)、`Style.Cat.Neon`(霓虹灯光)、`Style.Cat.Handcraft`(手工艺)、`Style.Cat.PaperCraft`(纸艺纸雕)、`Style.Cat.Collage`(拼贴混合)、`Style.Cat.Photography`(摄影暗房)、`Style.Cat.GlassMosaic`(玻璃马赛克)、`Style.Cat.Metallic`(金属质感)、`Style.Cat.LightParticles`(光影粒子)、`Style.Cat.Traditional`(国风传统)、`Style.Cat.Experimental`(实验创意)

---

## 注意事项(发布前必读)

1. **id 唯一且不可变**:一旦发布,`id` 不能修改——客户端用它做安装/卸载/查重的唯一凭据。
2. **不要重复提交**:提交前先在 `registry.json` 里搜索你的插件 id 与名称,重复的 PR 会被关闭。
3. **描述必须真实**:描述应说明插件实际效果;禁止堆砌关键词、仿冒官方或他人插件名称。
4. **截图规范**:暂用官方样张(`preview: "sample"`);启用截图上传后,要求 16:10、单张 ≤ 500KB、不得使用与实际效果不符的图片。
5. **版本号**:每次更新内容必须递增版本号,并在描述中注明变更。
6. **兼容性**:插件依赖的客户端能力需 ≥ Viora 1.0;使用实验性 API 必须在描述中声明。
7. **内容合规**:禁止违法违规、侵权、含恶意代码或误导用户的插件;一经发现下架并封禁提交者。
8. **分类准确**:宁可归入接近的大类,也不要滥用「实验创意」。

## 提交流程

1. Fork 本仓库 → 把插件包 `packages/<pluginId>.zip` 放入 `packages/` → 在 `registry.json` 的 `presets` 数组中追加对应条目。
2. 提交 PR,标题格式:`[插件] <id> <名称>`。
3. 通过校验(格式 + 查重 + 人工审核)后合并,客户端市场即可搜索并下载。

## 客户端如何使用本仓库

- 市场启动时拉取 `registry.json`(原始地址:`raw.githubusercontent.com/huyangpahuo/Viora-plugins/main/registry.json`)。
- 每个插件是**独立完整的个体**:`packages/<id>.zip` 内含 `plugin.json` 与编译好的插件 DLL。
- 下载插件 = 客户端拉取 zip 并解压到本地插件目录(宿主内置查重:已安装同 id 会跳过)。
- 卸载插件 = **真正删除**本地插件文件夹(含 DLL),可通过市场重新下载。
- 网络不佳时,客户端回退到随包内置的 registry 快照,保证市场始终可用。
