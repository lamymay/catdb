# CatDB 隐私政策 / Privacy Policy

最后更新 / Last updated: 2026-10-17

## 中文

CatDB 是一款运行在你本地电脑上的数据库客户端。我们重视你的隐私，具体说明如下：

- **不收集个人信息**：CatDB 不会收集、存储或上传任何可以识别你身份的个人信息。
- **不上传数据**：CatDB 只会连接你自己配置的数据库服务器（如 MySQL、PostgreSQL、SQLite）。你的查询、表数据和查询结果只在你的电脑和你指定的数据库之间传输，不会经过我们的服务器，也不会发给任何第三方。
- **本地存储**：连接配置（主机、端口、用户名等）、界面设置和日志只保存在你本机的用户目录里，不会上传。数据库密码默认不保存；只有你勾选“记住密码”时，才会加密保存在本机的系统凭据库和 CatDB 的本地加密密码库中，详见 [密码处理说明](PASSWORD.md)。
- **连接分享**：只有你主动复制连接分享串并自行发送给他人时，连接信息（以及你选择包含的、用分享口令加密的密码）才会离开你的电脑。CatDB 本身不会发送分享串。
- **无统计和广告**：CatDB 不包含任何数据统计、遥测、广告或第三方跟踪组件。
- **删除数据**：卸载 CatDB，删除本机用户目录里的 CatDB 配置文件夹；如果使用过“记住密码”，再在 Windows“凭据管理器”（或 macOS“钥匙串访问”）中删除以 `CatDB/` 开头的条目，即可清除所有本地数据。

### 密码处理

关于 CatDB 如何保存、加密和分享数据库密码，请参阅 [密码处理说明](PASSWORD.md)（https://github.com/lamymay/catdb/blob/main/PASSWORD.md）。

如果你对本政策有任何疑问，请在 [GitHub 仓库](https://github.com/lamymay/catdb/issues) 提交 issue 联系我们。

## English

CatDB is a database client that runs locally on your computer.

- **No personal data collection**: CatDB does not collect, store, or transmit any personally identifiable information.
- **No data upload**: CatDB only connects to database servers you configure yourself (such as MySQL, PostgreSQL, or SQLite). Your queries, table data, and results travel only between your computer and the databases you choose. They never pass through our servers and are never shared with third parties.
- **Local storage**: Connection settings (host, port, user name, and so on), UI preferences, and logs are stored only in your local user profile and are never uploaded. Database passwords are not saved by default. Only if you turn on "Remember password" are they saved, encrypted, in your system credential store and CatDB's local encrypted vault on this computer. See [Password Handling](PASSWORD.md).
- **Connection sharing**: Connection details (and a password, only if you choose to include it, encrypted with your share passphrase) leave your computer only when you copy a share string and send it to someone yourself. CatDB never sends share strings anywhere.
- **No analytics or ads**: CatDB contains no analytics, telemetry, advertising, or third-party tracking.
- **Deleting your data**: Uninstall CatDB and delete the CatDB settings folder in your user profile. If you used "Remember password", also remove entries starting with `CatDB/` in Windows Credential Manager (or macOS Keychain Access). This removes all local data.

### Password handling

For how CatDB saves, encrypts, and shares database passwords, see [Password Handling](PASSWORD.md) (https://github.com/lamymay/catdb/blob/main/PASSWORD.md).

If you have any questions about this policy, please open an issue on [GitHub](https://github.com/lamymay/catdb/issues).
