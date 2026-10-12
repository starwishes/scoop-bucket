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
| `7zip` | 26.04 | main |
| `abdownloadmanager` | 1.10.4 | extras |
| `adb` | 37.0.1 | main |
| `bitcomet` | 2.22 | BitComet 官网 *(自建)* |
| `codeg` | 0.35.1 | spacering-net/codeg *(自建)* |
| `ffmpeg` | 9.0.2 | main |
| `filelight` | 26.08-2202 | extras |
| `gh` | 2.102.0 | main |
| `git` | 2.56.0.2 | main |
| `github` | 3.6.6 | extras |
| `innounp` | 2.71.1 | — |
| `Joplin` | 3.7.21 | extras |
| `keepassxc` | 2.7.12 | extras |
| `librewolf` | 157.0.1-1 | extras |
| `lxmusic` | 2.12.6 | dorado |
| `Meslo-NF` | 3.5.1 | nerd-fonts |
| `nodejs-lts` | 24.21.0 | main |
| `obsidian` | 1.14.4 | extras |
| `oh-my-posh` | 31.5.0 | main |
| `PixPin` | 3.5.5.1 | abgox/abgo_bucket *(已重写)* |
| `potplayer` | 260819 | extras |
| `python311` | 3.11.9 | versions |
| `qbittorrent-enhanced` | 5.2.4.10 | extras |
| `qview-nightly` | 18433912418 | versions |
| `Rclone` | 1.75.1 | main |
| `reviva` | 1.4.1 | mingchen666/Reviva *(自建)* |
| `scoop-search` | 2.1.0 | — |
| `streamdeck` | 7.6.0.23012 | extras |
| `sudo` | 0.2020.01.26 | — |
| `telegram` | 7.2.9 | extras |
| `thunderbird` | 157.0.1 | — |
| `TightVNC` | 2.8.90 | extras |
| `twinkle-tray` | 1.17.2 | extras |
| `ungoogled-chromium` | 154.0.8037.97-1.1 | extras |
| `vscode` | 1.141.0 | extras |
| `windows-terminal` | 1.25.2733.0 | extras |
| `winshun` | 0.7.3 | LingCore/WinShun *(自建)* |
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
| mingchen666/Reviva | <https://github.com/mingchen666/Reviva> |
| BitComet | <https://www.bitcomet.com/> |

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

- `winshun` 取官方 portable zip。`post_install` 会把 %APPDATA%\WinShun 与 %LOCALAPPDATA%\WinShun 改成指向 `persist\winshun\Profile` 的**目录联接（junction）**，把数据钉在 D 盘（不在 C 盘；重装系统后重新 `scoop install` 即可接回）。若这两个路径已存在**真实目录**，脚本不动它、只给提示。`pre_uninstall` 在卸载时移除这两个联接、保留数据。
- `codeg` 是 Tauri NSIS 安装器，用 `#/dl.7z` 解包后在 `pre_install` 里清掉 `$PLUGINSDIR` 与 `uninstall.exe`。
- `reviva` 是 electron-builder NSIS 安装器，需两步解包：先解外层，再在 `pre_install` 里解嵌套的 `$PLUGINSDIR\app-64.7z`。
- `bitcomet` 取官方免安装 ZIP；用 `persist` 保存 `BitComet.xml`、`Downloads.xml`、`torrents`、`share`、`archive`，
  并在 `pre_install` 里预建那两个 xml（Scoop 对**不存在**的持久化文件会建成目录，必须先建文件）。

这几条的 `autoupdate` 都不写 `hash`：`winshun` 从随附的 `SHA256SUMS.txt` 取；
`codeg` / `reviva` 由 Scoop 自动读 GitHub release asset 的 `digest`；
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
