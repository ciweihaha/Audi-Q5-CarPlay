# 奥迪 Q5（2018 / 2019 款，标准轴距，非 Q5L）刷 CarPlay 教程

2018 / 2019 款奥迪 Q5（FY，标准轴距，**不是 Q5L**）把 Apple CarPlay 刷进虚拟座舱的教程合集。
适用机头是 **MHI2Q**（MIB2 High，高通方案）+ 全液晶虚拟座舱。

> ⚠️ 刷机有风险，会改写车机固件和配置，操作不当可能导致车机无法开机，恢复可能要拆机接串口。
> 务必先备份，电瓶接充电器，全程不要断电。以下内容仅供学习参考，后果自负。

## 先确认：你的车能不能刷

以下 6 条必须**全部**满足，缺一条都别往下走：

| 检查项 | 怎么看 |
| --- | --- |
| 机头是 **MHI2Q**，不是普通 MHI2 | MMI 设置 → 版本信息，媒体驱动版本里含 **`MMX2QC`** 就是 MHI2Q（例如 `CLU8_MMX2QC_AU_ER_G22_…`）；含 `MMX2`（如 `CLU6_MMX2_…`）是普通 MHI2，本教程不适用 |
| 知道固件 **train 和 MU 号** | 进红色工程菜单 → Version information，例如 `MHI2Q_ER_AUG22_P5092`、MU `1329` |
| 地区是 US、ER、JP 或 CN | train 第二段就是地区码（`_ER_` = 欧洲/世界其他地区） |
| 固件是已验证版本 | US `P5087`、ER `P5092`、CN `K1004`、JP `P1017`；`P5152` 与 `P5092` 主程序相同，也支持。老固件要先升级（见第四步） |
| **全液晶虚拟座舱** | 指针仪表不支持 |
| **原厂 CarPlay 插线已能在中控屏上用** | 这个破解是把原厂 CarPlay **扩展到虚拟座舱**，不是从无到有激活 CarPlay。如果插线后中控屏根本没有 CarPlay，先解决原厂 CarPlay 激活（FEC / SWaP）再来 |

### 怎么进工程菜单（Q5 2017–2020 的按键）

中控 MENU / BACK 两侧各有一个上下拨动的摇杆：左边是 NAV/MAP ↕ TEL，右边是 RADIO ↕ MEDIA。
按住几秒：

| 打开 | 按法 |
| --- | --- |
| **红色工程菜单** | 左摇杆**上**（NAV/MAP）+ 右摇杆**下**（MEDIA） |
| **绿色工程菜单 GEM** | 左摇杆**上**（NAV/MAP）+ 右摇杆**上**（RADIO），需先装 M.I.B. 开启后才有效 |
| **重启 MMI** | 两个摇杆都**上** + 按中间旋钮 |

## 路线 A（推荐）：软件破解，原生 CarPlay 进虚拟座舱

核心是 [luka-dev/mib2q-carplay-rgi](https://github.com/luka-dev/mib2q-carplay-rgi)，
通过 [M.I.B.（More Incredible Bash）](https://github.com/Mr-MIBonk/M.I.B._More-Incredible-Bash)
从 SD 卡安装。最完整的一步步教程是
[realshaunoneill/audi-mhi2q-carplay-cockpit](https://github.com/realshaunoneill/audi-mhi2q-carplay-cockpit)
（作者在 2018 款 S5 上实测，基于 mod `MHI2Q-2026-10-19.1` 和 M.I.B. `V3.8.0`，**以该教程和 mod 最新 release 为准**）。

刷完能得到：CarPlay 地图显示在虚拟座舱（View 键切换布局，方向盘滚轮缩放）、
逐向导航（转向箭头、距离条、车道线、到达时间、下一条路名，带 HUD 的车 HUD 上也有）、
CarPlay 通话显示在座舱电话页、专辑封面、MMI 触摸板移动 CarPlay 焦点、语音键短按唤出 Siri；
带外置功放的车（B&O 已验证）CarPlay 音乐是真 5.1。

### 需要准备

- 一张可清空的 SD 卡，32 或 64 GB，格式化成 FAT32（MBR 分区表）
- 一台能写 FAT32 卡的电脑
- **电瓶充电器/维护仪全程接上**：点火开着、发动机不开，两个小时能把电瓶放光
- 预留 2–3 小时：升固件 30–60 分钟，其余 30–45 分钟，另加下载和备卡时间
- 一根 iPhone 数据线

### 第一步：升级车机固件（已是验证版本可跳过）

只升**同一 train 内**的版本（`MHI2Q_ER_AUG22_…` 升到 `MHI2Q_ER_AUG22_…`），
**绝对不要把 MHI2Q 固件刷到普通 MHI2 机头上，反过来也不行，会变砖。**

1. 固件在 [mibvault.com](https://mibvault.com/)，MHI2 - HARMAN → Firmware → Audi。
   ER 版文件：`MHI2Q_ER_AUG22_P5092_MU1329 (A4-A5-Q5-Q7).7z`，约 1.9 GB，
   SHA-256：`e46f3be615dc227f1f127ed26a5aaf0b4d343f37efac072e917a667073de022f`。
   其他地区的文件和校验值在 mibvault 各文件的 ⓘ 信息里。**下载完务必核对校验值**，
   免费下载限速 50 Mbps 且链接很快过期，下载中断也可能看着像完成了。
2. 卡格式化成 FAT32（MBR），把压缩包解到卡根目录，保证 `metainfo2.txt` 在卡根目录。
   解开约 3.5 GB、642 个文件。
3. 接充电器，点火 ON、发动机 OFF（无钥匙进入的车，钥匙放车内，别带走，否则点火会断）、拔掉手机。
   卡插 **SD 卡槽 1**（B9 在手套箱）。
4. 红色工程菜单 → Update → SD 卡 → Standard → 拉到底 → Update。
   带 B&O 或 Bose 的车建议把**功放（amplifier）取消勾选**；没外置功放就没什么可取消的。
5. 等着别动，15–60 分钟不等，中间多次重启，黑屏或长时间停顿都正常。
   别拔卡、别开门、别启动发动机、别断点火。
6. 结束时个别组件可能显示 NOK（比如一个跟固件无关的 "Toolbox" 项），
   只要固件内的组件都是 OK 就选 Resume。随后出现 "Start version compare / connect diagnostic tester"
   时按 **Cancel**（这是 4S 的 SVM 注册步骤，取消会记一个 SVM 故障码并重启车机，正常，M.I.B. 之后会清除）。
7. 红菜单 → Version information 确认已是新 train，例如 `MHI2Q_ER_AUG22_P5092`、MU `1329`，
   再确认收音机、导航、CarPlay 都正常。

### 第二步：做 M.I.B. + CarPlay 安装卡

1. 下载 [M.I.B.](https://github.com/Mr-MIBonk/M.I.B._More-Incredible-Bash)（用 `V3.8.0` 或更新的 tag，只有 source 源码包）。
2. 下载 mod 的最新 release：[luka-dev/mib2q-carplay-rgi releases](https://github.com/luka-dev/mib2q-carplay-rgi/releases)，
   下 `install_*.zip`，同时把 `uninstall_*.zip` 和 `logging_*.zip` 留着备用，逐个核对 release 页上的 sha256。
3. 卡重新清空并格成 FAT32（固件文件不需要了）。
4. 把 M.I.B. 文件夹的**内容**拷到卡根目录，卡上应有 `metainfo2.txt`、`Launcher/`、`apps/`、`esd/`、`mod/` 等。
5. 把 mod 的 `install_MoreIncredibleBash/` 的**内容**合并拷到卡上：`mod/custom.sh` 会**替换** M.I.B. 自带的空占位脚本（正常），
   并新增 `mod/command.sh` 和 `mod/carplay/`。
6. 干净弹出卡（macOS 先跑 `dot_clean -m`）。

注意：`uninstall_*.zip` 和 `logging_*.zip` 里也有一个 `mod/custom.sh`，所以**同一时间卡上只能放安装、卸载、日志三者之一**，
另外两个放电脑上。

### 第三步：装 M.I.B.，再装 CarPlay

同样接充电器、点火 ON、钥匙在车内、**手机拔掉**、卡插 SD 卡槽 1，整个过程卡别拔。
从这里起**发动机可以开**（固件刷完了，后面只是拷文件改配置），但只能在步骤之间启动，
脚本运行时千万别打火（启动会重启 MMI），在室外操作。

1. **装 M.I.B.：** 红菜单 → Update → SD 卡 → **"FREE for all - M.I.B. Launcher V1.2"** → 开始。
   车机会重启三次（5–10 分钟），摘要里 Launcher 显示完成即可。如果又出现诊断仪/版本对比画面，按 Cancel。
   装完绿色工程菜单（GEM）就开启了。
2. **进绿菜单：** 左上 + 右上。冷启动后可能要等 **5 分钟**才能打开，应看到 `production`、`mib` 等条目。
3. **备份（强制）：** mib → Backup → **"System backup - MANDATORY before patching and other operations"** → 约 5 分钟。
   备份存到卡上的 `backup/` 文件夹，**事后拷一份到电脑上妥善保存**，里面是车机的 EEPROM、功能激活（FEC）、存储和版本信息。
   M.I.B. 会邀请你上传到 mibsolution.one，那是自愿的，而且是你这台车的身份和激活数据。
4. **清 SVM 故障：** 跑 M.I.B. 的 **SVM fix**。
5. **装 mod：** 手机仍拔掉，进 mib → **Advanced Settings → Run Custom Script**
   （老版 M.I.B. 叫 Run individual script，两种都行）。把输出拍下来，应看到：
   - `Firmware: MHI2Q_ER_AUG22_P5092 (platform MHI2Q, region ER, brand AU)`（或你自己的 train）
   - 一行 `Amplifier:`（没 B&O/Bose 的车显示 internal、保持原厂立体声是正常的）
   - 约 20 秒后结尾：**`DONE (install). Reboot the HU to load.`**
   
   如果某行写 **`FAILED`** 就停：检查没通过时安装器什么都不会改。
   如果停在警告（固件未验证、或检测到装过别的 CarPlay 插件）先读清楚；
   同一通电周期内再跑一次脚本会强制安装，没把握别这么干。
6. **重启 MMI**（双摇杆上 + 旋钮，或熄火再点火）。mod 的 Java 部分只有完全重启后才加载。
7. **插上 iPhone**，CarPlay 正常启动，座舱里出现 CarPlay 地图。开一段苹果地图导航验证箭头和车道线。
   冷启动后座舱可能短暂显示 "Initialising navigation"，正常。
8. 拔出 SD 卡，mod 不需要卡。

### 禁忌（别踩）

- **别跑 M.I.B. 自带的 "CarPlay Maps VC"**：那是给大众/斯柯达/西雅特机头的另一个 CarPlay 座舱插件，
  动的是同一批系统文件，两个 CarPlay 插件装一台机头是已知的搞坏方式。
- **别装其他 CarPlay 座舱插件**（MIBSI、mibr、CarPlay AltScreen toolbox 等），它们的卸载器会留文件，
  导致这个 mod 失效（mod issue #39、#53）。新版安装器检测到会警告停下；如果以前装过，干净办法是先重刷原厂固件。
- **别开 NavActiveIgnore / "dual navigation"**：mod 会主动删掉它，它会把 CarPlay 的状态对车机隐藏。
- **ifs-root / FEC 破解别动**，除非为了别的用途（比如地图更新），这个破解不需要，风险自担。

### 更新、卸载、恢复

- **更新到新版 mod：** 不用卸载，直接覆盖装：下载新 `install_*.zip` 核对 sha256；
  卡上把 `mod/carplay/` 整个换掉，`mod/custom.sh` 和 `mod/command.sh` 用新的覆盖
  （release 说明说换整个 `mod/` 文件夹，但卡上 `mod/` 里还有 M.I.B. 自己的文件，所以只换 mod 的那几样）；
  手机拔掉 → 绿菜单 → mib → Advanced Settings → Run Custom Script → 等 DONE → 重启。
- **看日志报 bug：** 把 `logging_MoreIncredibleBash/` 的内容换到卡上，同样跑一次；
  跑一次、连手机复现问题、再跑一次，卡上 `carplay_logs/` 下第二个编号文件夹就是详细记录，附到
  [mod 的 issue](https://github.com/luka-dev/mib2q-carplay-rgi/issues) 里。
- **卸载：** 同样方法换上 `uninstall_MoreIncredibleBash/`，它会用安装时留的 `.carplay-stock` 备份恢复所有被替换的文件，
  删掉新增的东西，跑两次也安全，然后重启。
- **装完卡在 Audi logo 上：** mod 仓库有 rescue 卡（`rescue_MoreIncredibleBash/`），不用拆机就能去掉 Java 部分；
  已知最坏情况需要拆下车机接串口。

### 限制

- **仅支持有线 CarPlay**。这代机头没有无线 CarPlay；mod 自己的无线研究卡在车机 Wi-Fi 速度上
  （稳定约 9 Mbps，需要约 19 Mbps）。外接无线 CarPlay 适配器时好时坏
  （Carlinkit 5.0 能连上，但音频从手机出、音乐页缺失、座舱小地图不会缩放；杂牌适配器完全不行）。
- **5.1 声道要外置功放**（B&O 已验证，Bose 未测）。车机自带功放的车，CarPlay 音频保持立体声。
- **逐向导航看 App**：苹果地图会发完整导航信息；Waze 不发，会自己在座舱地图里画转向卡片。

### 中国版（CN 固件）特别提示

新版安装器会读固件 train，按地区自动选：US、ER、JP 用 `carplay_hook.jar`，
**CN 用专用的 `carplay_hook_cn.jar`**，其他地区则不安 Java 部分（没有座舱/HUD 功能，但不会让 HMI 无法开机）。
老版本 release 曾明确警告不要装 CN 固件（`MIB2Q_CN_***` 会变砖），用**最新版**安装器就没这个问题，
但务必用 CN 对应的 `K1004` 固件，不要跨地区混刷。

## 路线 B（零风险）：外接 CarPlay 盒子

不想动原车系统，就加一个外接盒子：插在原车机头后面，即插即用，不改线、不刷机，
长按 MEDIA 键在原车界面和盒子界面之间切换。支持无线/有线 CarPlay、Android Auto、投屏，
用原车旋钮和方向盘按键操作，Siri 长按 MEDIA 3 秒。

常见型号（以卖家给的最新接线图和 DIP 设置为准）：

| 型号 | 适用 |
| --- | --- |
| Cartizan VI-AD320C-B9 | MIB / MIB2，2018 款起 Q5（7 寸屏），**不适用 8.3 寸屏** |
| ZZ-2 IT3-AUDI（MIB2 版） | MIB2，2017 款起 A4/A5/Q5/A6/A7 |
| EBILAEN 无线 CarPlay 盒子 | MIB / MIB2 / MIB2 Plus / MIB3 |

注意事项：

- DIP 开关按车型拨，改完要重启盒子才生效（2018 款 Q5 7 寸屏的一组常见设置是 ON ON OFF ON OFF OFF，**以产品说明书为准**）。
- 音频要走 AUX 或原车蓝牙：先把原车音源切到 AUX（或 OEM 蓝牙），否则 CarPlay 没声音；
  没 AUX 口只有 AMI 口的车要配 AMI 转接线。
- 盒子方案的 CarPlay 在**中控屏**上，不进虚拟座舱；要地图进座舱只能走路线 A。
- 买之前跟卖家确认三点：你的机头型号（MHI2Q）、屏幕尺寸（7 寸还是 8.3 寸）、有没有 AUX 口。

## 链接汇总

| 内容 | 链接 |
| --- | --- |
| **一步步安装教程（首选）** | [realshaunoneill/audi-mhi2q-carplay-cockpit](https://github.com/realshaunoneill/audi-mhi2q-carplay-cockpit) |
| **CarPlay mod 本体** | [luka-dev/mib2q-carplay-rgi](https://github.com/luka-dev/mib2q-carplay-rgi)（[最新 release](https://github.com/luka-dev/mib2q-carplay-rgi/releases)） |
| **M.I.B. 工具箱** | [Mr-MIBonk/M.I.B._More-Incredible-Bash](https://github.com/Mr-MIBonk/M.I.B._More-Incredible-Bash) |
| CarPlay + Android Auto 座舱版 | [kamgurgul/mib2q-carplay](https://github.com/kamgurgul/mib2q-carplay) |
| AltScreen（座舱第二路导航视频） | [yuedizhibo/MHI2Q-CarPlay-AltScreen](https://github.com/yuedizhibo/MHI2Q-CarPlay-AltScreen) |
| AltScreen 个人分支（2019 Q5 P5092 MU1329 实测记录） | [lindayi/mib2-carplay-rgi-altscreen](https://github.com/lindayi/mib2-carplay-rgi-altscreen) |
| 固件下载 + 按键组合表 | [mibvault.com](https://mibvault.com/)（MHI2 - HARMAN → Firmware → Audi） |
| 外接盒子说明书参考 | [Cartizan VI-AD320C-B9 说明书](https://manuals.plus/m/51741bfe0bce74d902599fe1019cf1ec35a775bf71479a3763ceca58c534094b) |

## 免责

本仓库只整理公开教程和链接，不分发任何固件或二进制文件，所有下载都来自原始来源。
刷机、改装与你自己的车、保修和当地法规有关，操作前请自行判断，风险自担。
与奥迪、大众集团、苹果及上述工具作者均无关联。
