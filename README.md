<div align="center">
  <br>
  <img src="./docs/assets/icon.png" alt="Lokey App Icon" width="120">
  <br>
  <h1>Lokey</h1>
  <p><strong>纯离线、物理级零联网的 iOS 26 键全拼与双拼中文输入法 · 1:1 原生手感零适应成本</strong></p>
  <p>无需开启「允许完全访问」即可使用默认 26 键全拼离线盲打</p>
  <p>
    <a href="#中文说明">简体中文</a> ·
    <a href="#english">English</a> ·
    <a href="#external-lexicon-packs">📦 外部词库包 (.lokeypack)</a> ·
    <a href="./PRIVACY.md">🔒 隐私政策</a>
  </p>
  <p>
    <img alt="Zero Network" src="https://img.shields.io/badge/Zero%20Network-0%20Sockets%20%2F%200%20Trackers-1E7E45?style=flat-square">
    <a href="https://github.com/Kianuni/lokey-frost-pack"><img alt="Frost Pack" src="https://img.shields.io/badge/Frost%20Pack-553K%20Entries%20(mmap)-0EA5E9?style=flat-square"></a>
  </p>
</div>

<div align="center">
  <br>
  <img src="./docs/assets/studio-keyboard.png" width="185" alt="Lokey 体验台与 1:1 液态玻璃键盘"/>&nbsp;&nbsp;
  <img src="./docs/assets/heatmap-dark.png" width="185" alt="Lokey 纯黑深色外观与 26 键盲打热区云图"/>&nbsp;&nbsp;
  <img src="./docs/assets/feel-light.png" width="185" alt="Lokey 手感与输入方案设置"/>&nbsp;&nbsp;
  <img src="./docs/assets/lexicon-light.png" width="185" alt="Lokey 词库数据与 .lokeypack 热挂载"/>
  <br>
  <sub>▲ 1:1 原生液态玻璃键盘、曜石深色与 26 键盲打热区云图、无级震动调校及只读零拷贝词库管理</sub>
</div>

---

## 中文说明

**Lokey** 是一款纯离线、物理级零联网的 iOS 26 键全拼与小鹤/自然码双拼中文输入法，**兼顾「1:1 原生手感 · 零学习适应成本」与「端侧智能 · 源码级零联网隐私」**：

- **1:1 原生手感，零学习适应成本**：透明根视图透出 iOS 26 Liquid Glass 原生液态玻璃底座，精确对齐常规 iPhone 四档高度及 iPhone Duo 内外双屏几何与光学中心；保留空框高频起始候选、120Hz 异步键帽气泡、左滑删除键（`⌫`）清空待上屏长句、横拖 `Shift` / 长按空格移动拼音光标、长按表情键快切左右单手模式、空格左侧可配置快捷标点（关闭 / 逗号 / 句号）、成对括号引号自动闭合、智能数字时间半角标点（`12:00` / `3.14` / `localhost:8080`）与 `CoreHaptics` `0%–100%` 无级震动，从系统自带键盘切换过来无需重新适应。
- **自研「落墨 · Inkstone」离线引擎**：支持 **26 键盲打热区自适应校准**（`2×2` 协方差高斯实时贴合拇指落点）、**双向神经网络长句语义与邻键误触纠错**、**展开候选面板拼音/部首/笔画多维快筛**、**句内无缝中英混输**、**OpenCC 词级简繁实时切换**（简体 / 台灣正體 / 香港標準）、**本机 SQLite 自造词记忆（21 天遗忘曲线）与自定义快捷短语**（支持 `rq`/`sj`/`xq` 动态日期时间、系统文本替换与 Rime `custom_phrase.txt` 双向互通），并支持只读 `mmap` 零拷贝热挂载可选 `55.3 万词` [白霜 `.lokeypack` 扩展包](https://github.com/Kianuni/lokey-frost-pack) 与个人 Rime 词表。
- **物理级零联网，源码与二进制独立可审计**：全工程源码与编译产物均不含任何网络框架、Socket 符号或数据采集，关闭系统「允许完全访问」即可使用默认 26 键全拼与内置词库离线盲打；开启仅用于本机 App Group 同步双拼/OpenCC 繁体/模糊音等偏好、个人词库、外置包与触感震动，依然零联网。

### 工程架构与源码一致性

- **`Shared/Engine/*.swift`（12 个文件）与 `KeyboardExtension/**/*.swift`（6 个文件）**：与商业版 `Lokey` 主仓库 **100% 字节级一致（SHA-256 完全匹配）**。
- **`AuditDemoApp/`**：极简 SwiftUI 宿主壳应用，仅保留键盘启用状态检测、实时试打沙盒、系统设置引导与零联网声明；省略了商业版宿主应用的 `StoreKit 2` 买断管理、`.lokeypack` 外部词库管理器、Rime 个人词库导入与备份 UI、诊断日志浏览及打包脚本。
- **`Shared/Resources/`**：仅包含由 `scripts/build_demo_lexicon.py`（及 `scripts/build_hanzi_meta.py`）离线编译的最小化示范词库（`demo_lexicon.bin` 约 4,000 词条 + 中性 `LNLM-v2` 占位权重，覆盖全部 416 个标准汉语拼音音节）、最小化 `lokey_emoji.bin`、`lokey_opencc.bin` 与 `lokey_hanzi_meta.bin`（汉字部首、总笔画与读音索引表，约 80 KB；不含商业版内置词典与模型参数资源）。构建时由 Xcode Build Phase 将 `demo_lexicon.bin` 复制为包内的 `lokey_lexicon.bin`，从而在不改动任何核心引擎 Swift 源码的前提下完成完整编译与测试。

### 本地构建与验证

#### 1. 生成工程并运行模拟器全量测试
```zsh
# 可选：离线或使用本地/拉取的开放许可语料重编示范词库
python3 scripts/build_demo_lexicon.py

# 生成 LokeyAudit.xcodeproj
python3 scripts/generate_xcodeproj.py

# 运行 XCTest（包含 Mach-O 二进制零联网审计、PrivacyInfo、全拼解码、hasFullAccess==false 离线测试与 VC 生命周期测试）
xcodebuild test \
  -project LokeyAudit.xcodeproj \
  -scheme LokeyAudit \
  -derivedDataPath build/DerivedData \
  -destination 'platform=iOS Simulator,name=iPhone 17 Pro'
```

#### 2. 构建产物 Mach-O 二进制与 `.debug.dylib` 核验

手动执行 `otool -L` / `nm -u` / `codesign` 核验：
```zsh
APP_DIR="build/DerivedData/Build/Products/Debug-iphonesimulator/AuditDemoApp.app"
EXT_DIR="$APP_DIR/PlugIns/LokeyKeyboard.appex"

# 检查动态库依赖（0 个网络框架）
otool -L "$EXT_DIR/LokeyKeyboard"
otool -L "$EXT_DIR/LokeyKeyboard.debug.dylib"

# 检查未定义导入符号（0 个 Socket / URLSession / NWConnection 符号）
nm -u "$EXT_DIR/LokeyKeyboard" | grep -E 'socket|connect|getaddrinfo|URLSession|NWConnection|CFNetwork' || echo "0 network symbols"
nm -u "$EXT_DIR/LokeyKeyboard.debug.dylib" | grep -E 'socket|connect|getaddrinfo|URLSession|NWConnection|CFNetwork' || echo "0 network symbols"

# 检查内嵌权限
codesign -d --entitlements - --xml "$EXT_DIR"
```

### 第三方许可声明
- 示范资源对应的第三方开源许可声明见 `LICENSES/`。

---

<a id="external-lexicon-packs"></a>

## 外部词库包（`.lokeypack`）格式说明与获取指引 / External `.lokeypack` Lexicon Guide

Lokey 内置 100% 开放许可的基础词库（开箱即用，无需联网），同时也支持用户通过本地文件导入被动的只读静态 `.lokeypack` 扩展基础词库包（`LKLF-v2` + `LNLM-v2` + `EOF-16`）。Lokey 本体永远不会联网下载任何词库；用户手动导入本地文件后，App 在本机完成魔数、段边界与 SHA-256 校验，再通过只读零拷贝 `mmap` 热挂载至键盘扩展。

### 1. 社区开源扩展词库参考（独立仓库）
- **`Kianuni/lokey-frost-pack`**（独立开源外部词库与离线编译器仓库）：
  - 仓库主页：<https://github.com/Kianuni/lokey-frost-pack>
  - 最新预编译 Release 下载：<https://github.com/Kianuni/lokey-frost-pack/releases/latest>（下载其中的 `rime-frost.lokeypack`；约 `44.32 MB`，含 `553,352` 条中文词条、单文件内嵌端侧神经打分/盲打纠错/语境记忆权重及可独立校验的 `.sha256` 文件）

### 2. iOS 设备导入步骤
1. **下载保存至「文件」App**：用系统浏览器打开上方 Releases 页面下载 `*.lokeypack` 文件至 iPhone「文件」App（或从 Mac 隔空投送 AirDrop 到 iPhone）。
2. **两种任选方式导入 Lokey**：
   - **方式 A（在 Lokey 内导入）**：打开 **Lokey → 词库数据 → 导入外部 `.lokeypack` 词库包**（或进入「管理基础词库与已安装包」），在文件选择器中选中下载好的 `.lokeypack` 文件。
   - **方式 B（用 Lokey 打开）**：在系统「文件」App 中长按或点击该 `.lokeypack` 文件 → 点击左下角**分享** → 选择 **「用 Lokey 打开」**。
3. **本机校验与热挂载**：在预览页核对词条数、来源与 SHA-256 摘要后确认安装，即可零延迟切换生效；在「基础词库管理」中支持多包共存，可随时**一键切回内置词库**或左滑删除外部包。

### 3. 如何自行离线编译 `.lokeypack` 词库包
Lokey 不限制官方签名包，任何用户均可在电脑端使用 [`Kianuni/lokey-frost-pack`](https://github.com/Kianuni/lokey-frost-pack) 提供的零第三方依赖 Python 3 编译器离线打包（复现发布包、追更上游热词，或通过 `--extra-dict` 合入个人/行业 `.dict.yaml` 专业词表）：
```zsh
git clone https://github.com/Kianuni/lokey-frost-pack.git
cd lokey-frost-pack
./scripts/fetch_rime_frost.sh

# 直接编译标准包，或通过 --extra-dict 合入自定义 Rime .dict.yaml / TSV 词表
python3 scripts/build_lokeypack.py --extra-dict my_domain.dict.yaml -o dist/my-custom.lokeypack
```

> **English Summary**: Lokey ships with an independent built-in lexicon and never downloads dictionaries over the network. Users can download a prebuilt community `.lokeypack` (such as [`Kianuni/lokey-frost-pack`](https://github.com/Kianuni/lokey-frost-pack/releases/latest)) or compile their own custom `.lokeypack` offline with `python3 scripts/build_lokeypack.py --extra-dict my_domain.dict.yaml`, save/AirDrop it to the iOS **Files** app, and import it via **Lokey → Lexicon Data → Import External `.lokeypack`** or **Share → Open in Lokey**. Lokey validates format bounds and SHA-256 locally before read-only `mmap` mounting and supports one-tap rollback to the built-in lexicon at any time.

---

## English

> Companion Lexicon Pack: **[`Kianuni/lokey-frost-pack`](https://github.com/Kianuni/lokey-frost-pack)** · Privacy Policy: **[`PRIVACY.md`](./PRIVACY.md)**

**Lokey** is an offline, physically zero-network iOS 26-key Full Pinyin & Xiaohe/Ziranma Shuangpin keyboard built around **1:1 native iOS ergonomics (zero learning curve)** and **auditable zero-data-collection privacy**. Powered on-device by the **Inkstone** engine, it pairs adaptive 26-key blind-typing heatmap calibration, bidirectional neural sentence semantics, Pinyin/Radical/Stroke candidate filtering, inline CN-EN mixed decoding, word-level OpenCC Simplified/Traditional conversion, left/right one-handed layouts, local user-phrase memory with custom phrase shortcuts, continuous `CoreHaptics`, and zero-copy `.lokeypack` `mmap` mounting—with zero networking frameworks or tracking in source or compiled binaries.

### Architecture & Source Parity

- **`Shared/Engine/*.swift` (12 files) & `KeyboardExtension/**/*.swift` (6 files)**: **100% byte-for-byte identical (matching SHA-256)** to the commercial `Lokey` repository.
- **`AuditDemoApp/`**: Minimal SwiftUI host shell providing keyboard activation status, a live typing sandbox, iOS Settings onboarding, and zero-network documentation. Commercial host-only `StoreKit 2` entitlement UI, `.lokeypack` pack management, Rime/backup UI, diagnostic viewer, and packaging scripts are omitted.
- **`Shared/Resources/`**: Contains minimal demonstration binaries compiled offline by `scripts/build_demo_lexicon.py` and `scripts/build_hanzi_meta.py`:
  - `demo_lexicon.bin`: ~4,000 entries covering all 416 canonical Mandarin syllables, basic English words, clean next-word bigrams, and neutral `LNLM-v2` placeholder weights (omitting commercial lexicon/model assets). Copied as `lokey_lexicon.bin` inside built app/extension/test bundles at build time so all engine & keyboard `.swift` files stay byte-identical.
  - `lokey_emoji.bin`, `lokey_opencc.bin`, & `lokey_hanzi_meta.bin`: Minimal Emoji association, Simplified/Traditional conversion, and CJK radical/stroke/syllable metadata (`LKHM-v1`, ~80 KB) tables.

### Local Build & Verification

1. **Regenerate Project & Run XCTest Suite**:
   ```zsh
   python3 scripts/build_demo_lexicon.py
   python3 scripts/generate_xcodeproj.py
   xcodebuild test \
     -project LokeyAudit.xcodeproj \
     -scheme LokeyAudit \
     -derivedDataPath build/DerivedData \
     -destination 'platform=iOS Simulator,name=iPhone 17 Pro'
   ```
2. **Built Mach-O & `.debug.dylib` Verification**:
   Manually inspect with `otool -L`, `nm -u`, and `codesign -d --entitlements - --xml` on both `<Exec>` and `<Exec>.debug.dylib` inside `AuditDemoApp.app` and `AuditDemoApp.app/PlugIns/LokeyKeyboard.appex`.

### Third-Party Licenses
- Permissive upstream data attributions for `demo_lexicon.bin`, `lokey_emoji.bin`, `lokey_opencc.bin`, and `lokey_hanzi_meta.bin` are included in `LICENSES/`.

---

<p align="center">
  <sub>
    <strong>Kianuni 苹果生态开源矩阵 · Apple Ecosystem Works</strong><br>
    <a href="https://github.com/Kianuni/Erosion-CN">Erosion-CN</a> ·
    <a href="https://github.com/Kianuni/AppleFlow">AppleFlow</a> ·
    <a href="https://github.com/Kianuni/AppleLens">AppleLens</a> ·
    <a href="https://github.com/Kianuni/WQGuard">WQGuard</a> ·
    <a href="https://github.com/Kianuni/Camlo">Camlo · 开幕</a> ·
    <a href="https://github.com/Kianuni/Enkard">Enkard</a>
  </sub>
</p>
