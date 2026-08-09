# 鸿蒙课表 App（HarmonyOS）

基于 HarmonyOS ArkTS 的周课表应用：周课表网格、ICS 课程导入、手动添加课程、课程批量管理、主题定制（主色调 / 课程色板 / 背景色 / 背景图）、桌面服务卡片（2x2 / 2x4 / 4x4）。

## 版本记录

### v0.1.1（当前）
- 背景定制：首页与「桌面卡片」支持背景色（8 种预设）与自定义背景图；背景图在桌面卡片中通过 `formImages`（fd）+ `memory://` 机制显示（ArkTS 卡片不支持 `file://`）。
- 课程同色系自动配色：新增课程按主色调色阶（HSL 派生 8 档）自动取色；ICS 导入课程按「课程名-星期-节次」哈希映射到同色系色阶。
- 顶部周按钮选中态强化（加粗 / 胶囊圆角 / 同色阴影）。
- 课程管理页（CourseManagePage）：多选、全选/反选、批量删除、清空。
- 背景偏好配置持久化（mode / color / image path）。

### v0.1.0-alpha（初版）
- 周课表网格（10 节课 × 5 天）、ICS 导入、手工添加课程、桌面卡片基础显示。

## 已知问题（供后续排查，暂不修复）

### 课程颜色无法调整
1. **色板修改不影响已有课程**：在「设置 → 课程配色」中对色板增删改（含修改某个色块）后，网格中已创建的课程颜色不会随之变化。
2. **编辑课程改色无效/不显示**：在添加/编辑课程页面重新选择颜色并保存后，课程格子的颜色没有变化或未生效。
3. **新增课程颜色不区分**：连续新增的多门课程在自动配色下颜色趋同，视觉上难以区分。

> 说明：课程颜色按课程单独存储，色板只影响后续选色的候选，未同步回溯已存在课程；新增课程的自动配色基于主色调色阶 + 哈希，名称相近时可能落为相邻色阶。以上现象已记录待确认，本版本不做修复。

## 构建

DevEco Studio 打开 `schedule/` 目录即可构建。命令行（debug、无签名）示例：

```powershell
$env:DEVECO_SDK_HOME = "C:\Program Files\Huawei\DevEco Studio\sdk"
$env:Path = "C:\Program Files\Huawei\DevEco Studio\jbr\bin;" + $env:Path
& "C:\Program Files\Huawei\DevEco Studio\tools\hvigor\bin\hvigorw.bat" assembleHap --mode module -p product=default -p buildMode=debug --no-daemon
```

## 源码结构
- `schedule/entry/src/main/ets/pages/`：页面（Index、SettingsPage、AddCoursePage、ImportPage、CourseManagePage）
- `schedule/entry/src/main/ets/service/`：业务与解析（FormUpdateService、WeekCalculator、IcsParser）
- `schedule/entry/src/main/ets/repository/ScheduleRepository.ets`：持久化
- `schedule/entry/src/main/ets/common/Constants.ets`：常量与颜色工具
- `schedule/entry/src/main/ets/widget/pages/WidgetCard.ets`：桌面卡片

## 归档包
- `课表-alpha-v0.1.0.zip`（老版本，历史归档，请勿改动）
- `课表-v0.1.1.zip`（当前版本）
