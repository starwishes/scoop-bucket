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
| `android-clt` | 15859902 | main |
| `cherry-studio` | 2.1.4 | extras |
| `dark` | 3.14.1 | main |
| `everything-cli` | 1.1.0.38 | main |
| `everything` | 1.4.1.1032 | extras |
| `feishin` | 1.17.0 | extras |
| `ffmpeg` | 9.0.2 | main |
| `filelight` | 26.08-2202 | extras |
| `Flow-Launcher` | 2.1.4 | extras |
| `gh` | 2.102.0 | main |
| `git` | 2.56.0.2 | main |
| `github` | 3.6.6 | extras |
| `Hiddify` | 4.1.1 | extras |
| `innounp` | 2.71.1 | main |
| `Joplin` | 3.7.21 | extras |
| `keepassxc` | 2.7.12 | extras |
| `kelivo` | 1.3.0 | extras |
| `librewolf` | 157.0.1-1 | extras |
| `Listary` | 6.3.5.94 | extras |
| `lxmusic` | 2.12.6 | dorado |
| `Meslo-NF` | 3.5.1 | nerd-fonts |
| `Motrix` | 1.8.19 | extras |
| `nodejs-lts` | 24.21.0 | main |
| `obsidian` | 1.14.4 | extras |
| `oh-my-posh` | 31.5.0 | main |
| `PixPin` | 3.5.5.1 | abgox/abgo_bucket *(已重写)* |
| `playnite` | 10.62 | extras |
| `potplayer` | 260819 | extras |
| `python311` | 3.11.9 | versions |
| `qbittorrent-enhanced` | 5.2.4.10 | extras |
| `qview-nightly` | 18433912418 | versions |
| `Rclone` | 1.75.1 | main |
| `scoop-search` | 2.1.0 | main |
| `streamdeck` | 7.6.0.23012 | extras |
| `sudo` | 0.2020.01.26 | main |
| `telegram` | 7.2.9 | extras |
| `thunderbird` | 157.0.1 | extras |
| `TightVNC` | 2.8.90 | extras |
| `twinkle-tray` | 1.17.2 | extras |
| `ungoogled-chromium` | 154.0.8037.97-1.1 | extras |
| `vscode` | 1.141.0 | extras |
| `Windhawk` | 1.7.3 | extras |
| `windows-terminal` | 1.25.2733.0 | extras |
| `yasb` | 2.0.7 | extras |

## 上游来源

| 上游 bucket | 地址 |
| --- | --- |
| main | <https://github.com/ScoopInstaller/Main> |
| extras | <https://github.com/ScoopInstaller/Extras> |
| versions | <https://github.com/ScoopInstaller/Versions> |
| nerd-fonts | <https://github.com/matthewjberger/scoop-nerd-fonts> |
| dorado | <https://github.com/h404bi/dorado> |
| scoopet | <https://github.com/ivaquero/scoopet> |
| abgo_bucket | <https://github.com/abgox/abgo_bucket> |

## 相对上游做的改动

为了让 bucket 自包含、能直接在新机器上使用，对少数 manifest 做了本地化处理：

- `everything` / `python311` / `vscode` / `windows-terminal`
  把硬编码的 `$bucketsdir\<上游bucket>\scripts\...` 改为 `$bucketsdir\$bucket\scripts\...`，
  并把所需的 `.reg` 脚本 vendor 到本仓库 `scripts/` 目录。
- `7zip` / `git`
  manifest 本身已使用动态 `$bucket`，无需改动；配套 `scripts/` 已 vendor。
- `pixpin`
  原 manifest 依赖 `abgox/abgo_bucket` 的 `bin/utils.ps1`，已重写为自包含版本
  （改用标准 `persist` 字段持久化 `Config` / `Data` / `History` / `OcrModel`），
  写法参考了 `h404bi/dorado` 与 `ivaquero/scoopet`。

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
