# 狼人杀法官助手

面向线下面杀主持人的离线工具，提供网页版和 Android 安装包。

**作者：Rat** · [GitHub](https://github.com/ChristopherRatRax) · 当前版本 **1.3.0**

## 下载与运行

- Android：[下载 WerewolfJudge-1.3.0.apk](WerewolfJudge-1.3.0.apk)，覆盖安装旧版本即可。
- 网页：[下载 index.html](index.html)，保存后在浏览器打开，完整功能在一个文件中，不需要服务器。
- 完整源码：[下载 WerewolfJudge-1.3.0-GitHub.zip](WerewolfJudge-1.3.0-GitHub.zip)，包含 Android 原生代码、网页源码、资源和测试报告。源码包不含发布签名密钥。
- 本地对局和工具记录保存在设备内，浏览器与安卓应用之间不会自动同步。

## 功能

- 4–18人角色配置、随机分配、全村民直接开局；玩家统一使用号码头像。
- 白痴、白狼王、黑狼王、骑士、隐狼、狼美人等角色和技能记录。
- 昼夜推进、动作日志、撤销、普通生死修正及结算回看。
- 多选指向投票，警长1.5/2票切换，最高票与平票高亮，独立投票撤销。
- 大数字全屏号码牌，背景沿用玩家头像颜色。
- 正反计时、常用时间卡片、暂停与重来。剩余N秒起逐秒短提示，归零单次清脆叮声，音效和振动可分别控制。
- 手机一屏优先适配，保持日夜配色和圆角立体按钮。

隐狼采用接刀版。工具用于主持人记录与修正，昼夜阶段及对局结束由主持人控制。

## 源码结构（解压源码包后）

```text
index.html                   完整离线网页
downloads/                   已签名Android安装包
android/web/                 网页主流程、角色和工具源码
android/assets/index.html    构建后嵌入Android的完整网页
android/src/                 原生WebView、计时音效与振动
android/res/                 主题、图标与本地叮声
android/tests/               浏览器交互和回归检查
```

## 构建

先解压上方完整源码包并进入 `WerewolfJudge/` 目录。需要 Python 3、JDK 17、Android SDK Platform 36 和 Build Tools 36.0.0。

```sh
cd android
python3 build.py --build-tools /path/to/build-tools/36.0.0 --android-jar /path/to/platforms/android-36/android.jar
```

没有 `javac` 时可用 Eclipse ECJ，并增加 `--ecj /path/to/ecj.jar`。构建会先生成完整网页，然后编译、对齐并签名APK。

发布签名密钥不在公开源码中。首次本地构建会生成本地签名，生成的包不能覆盖由原发布签名签署的安装包。维护者发布升级版时应使用私下保存的原签名。

## 测试与已知边界

先解压完整源码包。需要 Node.js、Playwright 和 Chromium。可在解压后的项目根目录安装测试依赖：

```sh
npm install --no-save playwright
npx playwright install chromium
node android/tests/integration.cjs
node android/tests/abstention-regression.cjs
node android/tests/v120-regression.cjs
node android/tests/v121-regression.cjs
node android/tests/v130-regression.cjs
```

可以设置 `WW_BROWSER_BINARY` 指定本地Chromium路径。本次验证包含旧功能回归、半票计算、生死修正、倒数提示、音频回退和手机布局。报告保存在 `android/tests/`。

尚未进行安卓实机扬声器、马达或不同厂商后台策略测试。安卓版提醒使用闹钟音量，可在应用内点击“测试提醒”检查；网页声音受浏览器播放策略影响。应用进程被系统结束后，工具不会代替系统闹钟。

## 署名

© 2026 Rat。详细署名见 [AUTHORS.md](AUTHORS.md)。
