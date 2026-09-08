# 这个项目应该怎么看

先看主页的视频和关卡截图，再看 [Blueprint 逻辑说明](blueprints/README.md)。节点图可以直接在 GitHub 阅读，不需要安装 Unreal。图是根据工程文件里的真实节点连线重建的，不是编辑器截图。

## 各类文件是什么

| 文件或目录 | 含义 | 怎么打开 |
| --- | --- | --- |
| `README.md` | 项目首页，介绍玩法、展示视频和截图 | GitHub 直接显示 |
| `docs/blueprints/README.md` | 各个系统怎么工作，包括设计说明和主要流程 | GitHub 直接阅读 |
| `docs/blueprints/BA_Player.md` 等 | 对应蓝图的完整节点参考，包含连线、输入值和节点编号 | GitHub 直接阅读 |
| `Content/blueprints/*.uasset` 及子目录 | Unreal 保存的蓝图等资源；蓝图本身就是这个项目的逻辑实现，不是普通文本代码 | 在 Unreal 内容浏览器打开 |
| `Content/Maps/*.umap` | 关卡文件，保存场景中摆放的物体等信息 | 在 Unreal 打开 |
| `Config/*.ini` | 输入、引擎、项目配置；例如 WASD 的轴映射 | 文本编辑器可以查看 |
| `SmallBallAdventure.uproject` | Unreal 工程入口，记录引擎版本等信息 | 用对应版本 Unreal 打开 |
| `.gitignore` | 告诉 Git 哪些缓存、临时文件不要上传 | 文本文件，招聘人员通常不用看 |
| `docs/images` | README 使用的关卡截图 | GitHub 直接查看 |
| `asset-manifest.json` | 记录本次检查的资源、节点数量和文件校验值 | 用于核对资料对应的是哪一份蓝图 |

这个仓库仍是作品展示副本，缺少部分场景素材，不能把它当成已经验证能直接运行的完整发行版。Starter Content 的使用已经在首页注明。

## 面试时最值得讲清楚的逻辑

### 1. 小球为什么会滚

[BA_Player](blueprints/BA_Player.md) 接收 WASD 对应的轴输入，先确认还没获胜，再把输入值乘以 `amplification`，用 `AddTorqueInRadians` 给球施加扭矩。不是每帧直接改球的位置。当前类默认倍率是 30；关卡中的实例可能覆盖默认值。

### 2. 炮台怎样循环开火

[Cannon_Blueprint](blueprints/Cannon_Blueprint.md) 从 BeginPlay 进入 Delay，时间到后播放效果、生成炮弹，再连回 Delay。另一条 Sequence 输出播放后坐力 Timeline，改变炮管的相对 Y 位置。炮弹出生位置和方向来自炮口组件。

[cannon_bullet](blueprints/cannon_bullet.md) 命中后销毁自身，检查碰到的是不是玩家，是的话施加 1 点伤害。玩家收到任意伤害就淡出并重开，没有实现多段血量系统。

### 3. 水晶不是碰到就直接消失

[BP_cristal](blueprints/BP_cristal.md) 确认玩家碰到后，先保存位置、大小和灯光亮度，再用 Timeline 让它向玩家移动、缩小、变暗。动画结束才给收集数量加一，最后销毁水晶。

### 4. 怎么判断通关

[Success_trigger](blueprints/Success_trigger.md) 开局统计关卡水晶总数。玩家进入出口时比较已收集数量和总数：没收齐就让 UI 闪烁；收齐后判断是否最后一关，否则保存时间并切到下一关。最后一关把 `IsWin` 设为 true。

### 5. 玩家、界面为什么都知道已经获胜

[BP_GameInstance](blueprints/BP_GameInstance.md) 保存共享状态。[BA_Player](blueprints/BA_Player.md) 读 `IsWin` 来停止移动和物理模拟；[BP_UI](blueprints/BP_UI.md) 读它来停止计时、显示胜利文字。

### 6. 重复用到的节点怎么组织

[NewMacroLibrary](blueprints/NewMacroLibrary.md) 把“是不是玩家”“获取自己的 GameInstance”“淡入淡出”和“重新加载当前关卡”封成宏，其他蓝图调用这些宏。淡出宏只启动淡出，真正等待一秒的是调用方的 Delay。

## 节点参考表怎么看

- **实线箭头**：执行顺序。
- **虚线箭头**：数据从哪里来。
- **ID**：这份资源内部的节点编号，便于对照表格，不是关卡物体编号。
- **Connected inputs**：这个节点输入来自哪个节点，或使用哪个固定值。
- **Unconnected**：没有向后连接，不代表一个完整功能。

不用背每个编号。面试时优先讲清楚“什么触发 → 做什么判断 → 改变什么 → 玩家看到什么”，再打开相应节点页举例。`hitwall` 里的两个练习蓝图没有接好的事件逻辑，不要把它们当成已完成的命中系统介绍。

## 常见节点名字是什么意思

| 节点 | 在这里的作用 |
| --- | --- |
| `Branch` | 根据一个真假值选择 True / False 两条路 |
| `Get` / `Set` 变量 | 读取 / 改写一个状态，例如收集数量或 IsWin |
| `Cast to BP_GameInstance` | 把通用 GameInstance 引用转成项目自己的类型，才能读写自定义变量 |
| `Lerp` / `VLerp` | 根据 Alpha 在起点和终点之间插值；后者处理位置、大小这样的向量 |
| `Timeline` | 随时间输出数值，并提供 Update 和 Finished 执行点 |
| `Delay` | 等一段时间后继续执行，不会卡住整个游戏 |
| `Sequence` | 按顺序触发多个执行输出 |
| `DoOnce` | 只放行第一次执行；这个项目用它避免获胜后每帧重复关闭物理 |
| `Reroute` | 整理连线的转接点，本身不增加玩法逻辑 |
| `Array Get` | 通过索引取列表中的一个元素，例如下一关名字 |
| `Graph boundary` | 函数或宏的输入、输出边界 |
| `Conv_...` | 转换数据类型，例如把水晶数量从整数转成界面文字 |
