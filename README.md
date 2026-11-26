# scoop-bucket

[starwishes](https://github.com/starwishes) 的个人 [Scoop](https://scoop.sh) bucket —— 收录我在用的 Windows 软件。

> 本仓库只放 manifest，安装包一律由各软件官方地址下载。

## 安装使用

```powershell
scoop bucket add starwishes https://github.com/starwishes/scoop-bucket
scoop install starwishes/<app>
```

> 注意：清单里多数应用在官方 `main` / `extras` 中同样存在。若你同时装了官方 bucket，
> `scoop install <app>` 可能装到官方那份；要确保用本仓库的版本，请显式写
> `scoop install starwishes/<app>`。

## 收录清单

| 应用 | 本仓库版本 | 上游来源 |
| --- | --- | --- |
| `7zip` | 26.05 | main |
| `abdownloadmanager` | 1.11.2 | extras |
| `adb` | 37.0.1 | main |
| `bitcomet` | 2.23 | BitComet 官网 *(自建)* |
| `codeg` | 0.41.3 | spacering-net/codeg *(自建)* |
| `edgeever` | 2.30.0 | tianma-if/edgeever *(自建)* |
| `ffmpeg` | 9.0.2 | main |
| `filelight` | 26.08-2226 | extras |
| `gh` | 2.103.3 | main |
| `git` | 2.56.0.4 | main |
| `github` | 3.6.10 | extras |
| `innounp` | 2.71.3 | — |
| `Joplin` | 3.7.22 | extras |
| `keepassxc` | 2.7.12 | extras |
| `librewolf` | 159.0-1 | extras |
| `lxmusic` | 2.12.6 | dorado |
| `Meslo-NF` | 3.5.1 | nerd-fonts |
| `misterdownloadmanager` | 0.5.2 | anefzaoui/misterdownloadmanager *(自建)* |
| `nodejs-lts` | 26.13.0 | main |
| `obsidian` | 1.14.5 | extras |
| `oh-my-posh` | 31.9.1 | main |
| `PixPin` | 3.6.6.0 | abgox/abgo_bucket *(已重写)* |
| `potplayer` | 261021 | extras |
| `python311` | 3.11.9 | versions |
| `qbittorrent-enhanced` | 5.2.4.10 | extras |
| `qview-nightly` | 41787387905 | versions *(自建覆盖)* |
| `Rclone` | 1.75.2 | main |
| `reviva` | 1.4.2 | mingchen666/Reviva *(自建)* |
| `scoop-search` | 2.1.0 | — |
| `streamdeck` | 7.6.0.23012 | extras |
| `sudo` | 0.2020.01.26 | — |
| `telegram` | 7.3.3 | extras |
| `thunderbird` | 159.0 | — |
| `TightVNC` | 2.8.90 | extras |
| `twinkle-tray` | 1.17.2 | extras |
| `ungoogled-chromium` | 156.0.8078.73-1.1 | extras |
| `vscode` | 1.144.0 | extras |
| `wandao` | 1.5.3 | tllovesxs/wandao *(自建)* |
| `wenlanai` | 2.8.0 | ddys9621/WenlanAI *(自建)* |
| `windows-terminal` | 1.25.2733.0 | extras |
| `winshun` | 0.17.0 | LingCore/WinShun *(自建)* |
| `yasb` | 2.0.7 | extras |

## 上游来源

| 上游 bucket / 项目 | 地址 |
| --- | --- |
| main | <https://github.com/ScoopInstaller/Main> |
| extras | <https://github.com/ScoopInstaller/Extras> |
| versions | <https://github.com/ScoopInstaller/Versions> |
| nerd-fonts | <https://github.com/matthewjberger/scoop-nerd-fonts> |
| dorado | <https://github.com/h404bi/dorado> |
| scoopet | <https://github.com/ivaquero/scoopet> |
| abgo_bucket | <https://github.com/abgox/abgo_bucket> |
| LingCore/WinShun | <https://github.com/LingCore/WinShun> |
| spacering-net/codeg | <https://github.com/spacering-net/codeg> |
| tianma-if/edgeever | <https://github.com/tianma-if/edgeever> |
| mingchen666/Reviva | <https://github.com/mingchen666/Reviva> |
| BitComet | <https://www.bitcomet.com/> |
| tllovesxs/wandao | <https://github.com/tllovesxs/wandao> |
| anefzaoui/misterdownloadmanager | <https://github.com/anefzaoui/misterdownloadmanager> |

## 相对上游做的改动

为了让 bucket 自包含、能直接在新机器上使用，对少数 manifest 做了本地化处理：

- `python311` / `vscode` / `windows-terminal`
  把硬编码的 `$bucketsdir\<上游bucket>\scripts\...` 改为 `$bucketsdir\$bucket\scripts\...`，
  并把所需的 `.reg` 脚本 vendor 到本仓库 `scripts/` 目录。
- `7zip` / `git`
  manifest 本身已使用动态 `$bucket`，无需改动；配套 `scripts/` 已 vendor。
- `pixpin`
  原 manifest 依赖 `abgox/abgo_bucket` 的 `bin/utils.ps1`，已重写为自包含版本
  （改用标准 `persist` 字段持久化 `Config` / `Data` / `History` / `OcrModel`）。

### 本仓库自建的 manifest

上游没有现成的 Scoop manifest，以下为**自建**，不要被上游内容覆盖：

- `winshun` 取官方 portable zip。`post_install` 会把 %APPDATA%\WinShun 与 %LOCALAPPDATA%\WinShun 改成指向 `persist\winshun` 的**目录联接（junction）**，把数据钉在 D 盘（不在 C 盘；重装系统后重新 `scoop install` 即可接回）。若这两个路径已存在**真实目录**，脚本不动它、只给提示。`pre_uninstall` 在卸载时移除这两个联接、保留数据。
- `codeg` 是 Tauri NSIS 安装器，用 `#/dl.7z` 解包后在 `pre_install` 里清掉 `$PLUGINSDIR` 与 `uninstall.exe`。
- `reviva` 是 electron-builder NSIS 安装器，需两步解包：先解外层，再在 `pre_install` 里解嵌套的 `$PLUGINSDIR\app-64.7z`。
- `bitcomet` 取官方免安装 ZIP；用 `persist` 保存 `BitComet.xml`、`Downloads.xml`、`torrents`、`share`、`archive`，
  并在 `pre_install` 里预建那两个 xml（Scoop 对**不存在**的持久化文件会建成目录，必须先建文件）。
- `wandao` 是 Tauri NSIS 安装器（同样 `#/dl.7z` + 清 `$PLUGINSDIR`/`uninstall.exe`）。它把用户数据放在
  %APPDATA%\wandao 而不是安装目录，所以**不需要 `persist`**；不走真安装器也顺带避开了它那段会去卸载
  旧版、写注册表项的迁移钩子。注意它自带 Tauri 在线更新，用 Scoop 管理时不要在应用内更新。
- `edgeever` 也是 electron-builder NSIS 安装器，与 `reviva` 同样两步解包（`#/dl.7z` → `pre_install` 解
  `$PLUGINSDIR\app-64.7z`，再清掉 `$PLUGINSDIR` / `$R0`）。数据在 %APPDATA%\@edgeever\desktop，**不需要 `persist`**。
  它自带 electron-updater，应用内更新会另装一份独立程序，用 Scoop 时请走 `scoop update edgeever`。
- `qview-nightly` 覆盖 `versions` 上游版：2026-11-05 起 nightly zip 把文件放进了一层
  `qView-nightly-Windows_<arch>-<date>.<attempt>\` 子目录，上游 `pre_install` 仍在根目录找
  `qView-nightly-*.exe`，于是 `Can't shim 'qView.exe': File doesn't exist.`。本仓库版本在
  `pre_install` 里先把唯一子目录的内容摊平、再重命名 exe，安装源写 `starwishes/qview-nightly`。
- `misterdownloadmanager` 取官方免安装 `MisterDownloadManager-<ver>-portable.zip`，`extract_dir` 指向包内的同名目录。
  它的便携模式把下载列表与设置放在程序目录的 `data` 子目录里，所以 `persist` `data`；`-cli` / `-mcp` 两个
  可执行文件一并加入 PATH。便携模式本身不写注册表，浏览器扩展的原生消息宿主无法自行注册，所以本仓库在
  `post_install` 里代它写一份 host manifest 到 `persist\misterdownloadmanager\native-host\`（`path` 指向
  `apps\misterdownloadmanager\current\misterdownloadmanager-nmhost.exe`，`allowed_extensions` 为
  `integration@misterdownloadmanager.com`），并注册 `HKCU\Software\Mozilla\NativeMessagingHosts\` 与
  `HKCU\Software\LibreWolf\NativeMessagingHosts\` 两处键、复制一份到 `%APPDATA%\Mozilla\NativeMessagingHosts\`；
  `pre_uninstall` 再把它们清掉。Firefox / LibreWolf 装 AMO 上的扩展后弹窗即显示「已连接到 Mister Download Manager」；
  Chromium 系另写一份 `allowed_origins` 版（`persist\misterdownloadmanager\native-host\com.misterdownloadmanager.host.chromium.json`），
  扩展 ID 由安装目录 `extension\chromium\manifest.json` 的 `key`（DER SPKI）推算：SHA-256 取前 16 字节，每个十六进制
  半字节 n 映射成字母 `'a'+n`，本版为 `oogbclbbninliongijffeeggalppliok`；注册 `HKCU\Software\` 下
  `Google\Chrome`、`Microsoft\Edge`、`Chromium`、`BraveSoftware\Brave-Browser`、`Vivaldi` 各自的 `NativeMessagingHosts` 键。
  实测用该 ID 作 argv[1] 调 `misterdownloadmanager-nmhost.exe` 时不再输出 `refusing caller ...: not the Mister Download Manager extension`，
  换任意假 ID 则被拒——说明 origin 校验按扩展 key 走，与商店版 ID 一致。
这几条的 `autoupdate` 都不写 `hash`：`winshun` 从随附的 `SHA256SUMS.txt` 取；
`codeg` / `reviva` / `wandao` / `edgeever` / `misterdownloadmanager` 由 Scoop 自动读 GitHub release asset 的 `digest`；
`bitcomet` 取不到哈希源时，Scoop 会直接下载该文件自行计算 SHA256。

`scripts/` 下的 `.reg` 文件用于注册右键菜单与文件关联，保持与上游一致。

## 自动更新

- `.github/workflows/excavator.yml`：每 4 小时在 **GitHub Actions（云端）** 运行
  [Excavator](https://github.com/ScoopInstaller/GithubActions)，检查上游新版本并自动提交更新。
  它只改本仓库里的 manifest，不会更新你本机已装的软件。
- 本地同步与升级：

  ```powershell
  scoop update     # 同步各 bucket 的 manifest（git pull）
  scoop update *   # 升级所有已装软件
  ```

## CI

`.github/workflows/ci.yml` 在 push / PR 时用 Scoop 官方的 schema 与 bucket 测试校验 manifest。

## License

[Unlicense](LICENSE)。manifest 内容来自上表各上游公开仓库（多为 Unlicense / MIT）。
