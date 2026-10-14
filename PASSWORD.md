# CatDB 密码处理说明 / Password Handling

最后更新 / Last updated: 2026-10-15

## 中文

### 当前行为

- **只保存在内存中**：你在连接对话框中输入的数据库密码只在本次运行期间保存在内存里，关闭 CatDB 后即丢失，下次启动需要重新输入。
- **不写入磁盘或配置文件**：保存连接时只记录类型、名称、主机、端口、数据库名和用户名；密码不会写入配置文件，旧版本可能残留的密码字段也会被主动清除。
- **传给内置工具的方式**：使用内置的 PostgreSQL 工具（如 `pg_dump`）时，密码只通过该子进程的环境变量（`PGPASSWORD`）传递，不会写入任何文件或命令行参数。
- **从不上传**：密码只用于连接你自己配置的数据库服务器，不会发送给我们或任何第三方。
- **不记录到日志**：密码不会写入 CatDB 的日志。

### 计划中（尚未提供）

> 以下功能仍在设计中，**当前版本尚未提供**。上线时本文档和隐私政策会同步更新。

- **可选的“记住密码”**：由你勾选决定是否保存。密码将保存在操作系统的凭据库中（Windows 凭据管理器 / macOS 钥匙串），配置文件中不保存密码。系统凭据库不可用时，在 Windows 上改用系统自带的 DPAPI 加密（仅当前系统用户可解密），不使用程序内置的固定密钥。
- **加密分享字符串**：可以把连接导出为一段分享字符串发给他人。是否包含密码由你选择（默认不包含）；包含密码时，用你临时设置的分享口令加密，对方需输入同一口令才能导入。分享字符串带有有效期和格式版本号，过期后无法导入密码。
- 以上数据同样只保存在本机或由你自行分享，CatDB 不会上传。

## English

### Current behavior

- **Memory only**: Database passwords you enter in the connection dialog are kept in memory for the current session only. They are lost when CatDB closes, and you need to enter them again next time.
- **Never written to disk or config**: When connections are saved, only the type, name, host, port, database, and user name are stored. Passwords are never written to the settings file, and any password field left by older versions is actively removed.
- **Bundled tools**: When CatDB runs its bundled PostgreSQL tools (such as `pg_dump`), the password is passed only through that child process's environment (`PGPASSWORD`), never through files or command-line arguments.
- **Never uploaded**: Passwords are used only to connect to the database servers you configure. They are never sent to us or any third party.
- **Not logged**: Passwords are never written to CatDB's logs.

### Planned (not yet available)

> The following features are still being designed and are **not available in the current version**. This document and the privacy policy will be updated when they ship.

- **Optional "remember password"**: Saved only if you opt in. Passwords would be stored in the operating system's credential store (Windows Credential Manager / macOS Keychain), never in the settings file. If the credential store is unavailable, Windows would fall back to the built-in DPAPI encryption (decryptable only by the current OS user), with no hardcoded key in the app.
- **Encrypted share strings**: Export a connection as a string to share with others. Including the password is your choice (off by default); if included, it is encrypted with a passphrase you set, and the recipient must enter the same passphrase to import it. Share strings carry an expiry time and a format version; expired strings cannot be used to import the password.
- This data likewise stays on your computer or is shared only by you; CatDB never uploads it.
