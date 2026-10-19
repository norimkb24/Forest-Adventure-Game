# 森友大冒险 · Animal Friends Adventure

一款「动物森友会」画风的平台跳跃小游戏（类超级马里奥），纯原生 HTML / CSS / JavaScript 实现，**无需构建、无第三方依赖、可完全离线运行**。

<p align="center">
  <img src="preview/screenshot-title.png" width="49%" alt="标题页" />
  <img src="preview/screenshot-gameplay.png" width="49%" alt="游戏画面" />
</p>

## 玩法

- **6 个关卡**，难度递增，逐关解锁：
  1. 春 · 青青草原
  2. 夏 · 海滨沙丘
  3. 秋 · 枫叶林
  4. 冬 · 雪之谷
  5. 黄昏 · 樱花丘
  6. 星夜 · 云端镇
- 跑跳、踩敌人、收集铃钱、躲避深坑，抵达终点风车即过关
- 顶部有**实时进度条**；全通关后触发 **CONGRATULATIONS** 庆祝页（彩带 + 统计）
- 生命值跨关继承，吃水果可加命，途中有检查点
- **20 个铃钱可兑换一把手枪**（本局限定），子弹可击杀敌人
- 自定义玩家名；4 个内置村民形象，或上传照片裁剪成**圆形头像**（头像随时可重新上传修改）
- 存档（进度 / 钱包 / 名字 / 形象）自动保存在浏览器 localStorage

## 操作

| 动作 | 键盘 | 触屏 |
| --- | --- | --- |
| 移动 | `←` `→` / `A` `D` | 左下方向键 |
| 跳跃 | `空格` / `↑` / `W` | 右下「跳」 |
| 射击 | `J` / `K` / `F` | 右下「枪」 |
| 暂停 | `P` / `Esc` | 右上 ⏸ |
| 静音 | `M` | 右上 🔊 |

## 本地运行

方式一：直接双击 `index.html`。

方式二（推荐，避免个别浏览器对本地文件的限制）：

```bash
python3 -m http.server 8000
# 然后打开 http://127.0.0.1:8000
```

## 部署到 GitHub Pages

1. 把本目录内容推送到 GitHub 仓库（**`index.html` 必须在仓库根目录**）
2. 仓库 **Settings → Pages**
3. **Source** 选择 `Deploy from a branch`，分支选 `main`、目录选 `/ (root)`
4. 等待约 1 分钟，访问 `https://<你的用户名>.github.io/<仓库名>/`

### 打不开 / 404 排查

| 现象 | 原因 | 解决 |
| --- | --- | --- |
| 仓库里只有 `animal-crossing-platformer/` 文件夹，`index.html` 在里面 | 多套了一层目录，Pages 根路径找不到入口 | 把文件夹**里面的内容**移到仓库根目录，让根目录直接看到 `index.html` |
| 打开是 GitHub 的 404 页 | Pages 未开启 / 分支或目录选错 / Source 选了 `GitHub Actions` 但没有工作流 | Settings → Pages 重新选 `main` + `/ (root)`，Source 不要选 GitHub Actions |
| 打开是白屏、标题是仓库名 | 上传的是 `.zip` 文件本身，没有解压 | 上传解压后的文件 |
| 提示 "There isn't a GitHub Pages site here" | 构建还没完成（通常 1–3 分钟）或构建失败 | 看仓库 **Actions** 标签里 `pages build and deployment` 是否变绿 |
| 私有仓库无法开启 Pages | GitHub 免费账号的 Pages 只支持公开仓库 | 把仓库改为 Public，或升级账号 |
| 改了代码但页面没变 | 浏览器 / Pages CDN 缓存 | 强制刷新（⌘⇧R）或用无痕窗口 |

> 快速自检：如果 `https://<用户名>.github.io/<仓库名>/animal-crossing-platformer/` 能打开，而 `/` 打不开，就是**多套了一层目录**。

## 单文件版（部署最省心）

仓库里附带 `build-standalone.js`，运行后会生成把 CSS 和全部 JS **内联进单个 HTML** 的版本：

```bash
node build-standalone.js   # 生成 ../单文件版/index.html（约 130KB）
```

部署时只需上传这一个 `index.html` 到仓库根目录即可，不依赖任何其他文件，可彻底避免「js 文件夹没传上去导致页面空白」的问题。

## 文件结构

```
.
├── index.html          # 页面结构（HUD / 标题页 / 暂停 / 通关庆祝）
├── style.css           # 全部样式
├── .nojekyll           # 让 GitHub Pages 跳过 Jekyll 处理
├── js/
│   ├── audio.js        # Web Audio 音乐与音效引擎
│   ├── levels.js       # 6 关程序化生成（固定种子）
│   ├── art.js          # Canvas 绘制层（地形 / 角色 / 敌人 / 道具）
│   └── game.js         # 主循环、物理碰撞、关卡流程、UI 交互
└── preview/            # 预览截图
```

## 技术说明

- **渲染**：Canvas 2D，逻辑分辨率 960×560，按窗口等比缩放适配
- **物理**：自定义 AABB 碰撞，包含土狼时间（coyote time）、跳跃缓冲、可变跳跃高度、移动平台骑乘
- **关卡生成**：固定随机种子程序化生成，保证每个坑都可跨越、每个检查点都有落脚点
- **音乐**：Web Audio 实时合成（74 BPM、F 大调的舒缓 Pad + 旋律 + 混响），每关移调；**不包含任何版权音频文件**
- **存档**：localStorage 单键存储（`ac-adventure-save-v1`）

> 说明：内置 BGM 是为本作**原创合成**的 Relaxing 风格旋律，并非 AAA《LIFE (Relaxing Remake)》——原曲受版权保护不能内置。想听自己的音乐，可在游戏暂停菜单点击「🎵 载入自定义 BGM」选择本地音频文件。

## License

未指定。如需开源，建议在仓库中添加 `LICENSE`（例如 MIT）。
