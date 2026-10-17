# CatDB 密码处理说明 / Password Handling

最后更新 / Last updated: 2026-10-17

> 适用于包含“记住密码”和“连接分享串”功能的 CatDB 版本（1.0.2.0 及以后）。更早的版本不保存密码，只在内存中使用。
> Applies to CatDB 1.0.2.0 and later, which add "Remember password" and connection share strings. Earlier versions never save passwords and keep them in memory only.

## 中文

### 默认行为：不保存

- 连接对话框中的“记住密码”默认**不勾选**。不勾选时，密码只在本次运行期间保存在内存中，关闭 CatDB 后即丢失。
- 连接配置中只保存类型、名称、主机、端口、数据库名、用户名，以及“是否记住密码”的标记，**从不保存明文密码**；旧版本可能残留的密码字段会被主动清除。

### 勾选“记住密码”后

- 密码同时保存在两个地方，两处都不是明文：
  1. **操作系统凭据库**：Windows 凭据管理器 / macOS 钥匙串 / Linux 上可用的系统密钥环。条目名以 `CatDB/` 开头。
  2. **本机加密密码库**：CatDB 设置目录下的本地数据库中，只保存经 AES-256-GCM 加密后的密文。
- 第一次勾选“记住密码”时，CatDB 会请你设置一个至少 8 个字符的**本地主密码**。主密码本身不会被保存；它用于在系统凭据库无法读取时解锁本地加密密码库（密钥由 PBKDF2-HMAC-SHA256，60 万次迭代派生）。
- 正常情况下，CatDB 启动时直接从系统凭据库读取，不会要求输入主密码。只有系统凭据库无法读取时，才会请你输入主密码，每次运行最多一次。
- **请记住你的主密码**：如果忘记主密码，同时系统凭据库也无法读取，本地加密密码库无法恢复，只能重新输入数据库密码。
- 取消勾选“记住密码”或删除连接时，该连接在两处保存的密码都会被删除。
- 程序中没有内置的固定密钥，也不会在无法安全保存时退回明文保存。

### 连接分享串

- 可以通过“复制当前连接分享串”把连接导出为一段 `catdb://share/v1/...` 字符串，发给他人导入。
- **默认不包含密码**。选择包含密码时，需要设置一个至少 8 个字符的**分享口令**，密码会用该口令加密（PBKDF2-HMAC-SHA256 + AES-256-GCM）。请通过另一个渠道把口令告诉对方。
- 分享串中的连接信息（类型、名称、主机、端口、数据库名、用户名）**不加密**，任何拿到分享串的人都能看到。导入前 CatDB 会显示这些信息并请你确认。
- 分享串带有格式版本号和有效期（1 至 365 天，默认 7 天）。过期后只能导入连接信息，不能导入密码。有效期依据接收方电脑的时间判断，且对方一旦导入，就无法撤回已分享的密码。
- 导入的连接默认不记住密码。

### 其他

- **传给内置工具的方式**：使用内置的 PostgreSQL 工具（如 `pg_dump`）时，密码只通过该子进程的环境变量（`PGPASSWORD`）传递，不会写入文件或命令行参数。
- **从不上传**：密码、主密码和分享口令只在你的电脑上使用，或由你自己分享；CatDB 不会发送给我们或任何第三方。
- **不记录到日志**：密码、主密码、分享口令和分享串不会写入 CatDB 的日志。

### 如何彻底删除已保存的密码

- 在 CatDB 中对每个连接取消“记住密码”或删除连接；或
- 卸载后，删除 CatDB 设置文件夹，并在 Windows“凭据管理器”（或 macOS“钥匙串访问”）中删除以 `CatDB/` 开头的条目。仅删除设置文件夹不会删除系统凭据库中的条目。

## English

### Default: nothing is saved

- "Remember password" in the connection dialog is **off by default**. When it is off, the password is kept in memory for the current session only and is lost when CatDB closes.
- Saved connections store only the type, name, host, port, database, user name, and a "remember password" flag. **Plain-text passwords are never saved**, and any password field left by older versions is actively removed.

### When "Remember password" is on

- The password is saved in two places, neither in plain text:
  1. **The operating system's credential store**: Windows Credential Manager, macOS Keychain, or an available system keyring on Linux. Entries are named starting with `CatDB/`.
  2. **A local encrypted vault**: a local database in CatDB's settings folder that holds only AES-256-GCM ciphertext.
- The first time you turn on "Remember password", CatDB asks you to set a **local master password** of at least 8 characters. The master password itself is never saved. It unlocks the local encrypted vault when the system credential store can't be read (the key is derived with PBKDF2-HMAC-SHA256, 600,000 iterations).
- Normally CatDB reads saved passwords from the system credential store at startup without asking for the master password. It asks only when the credential store can't be read, at most once per run.
- **Remember your master password**: if you forget it and the system credential store also can't be read, the local vault can't be recovered and you will need to re-enter your database passwords.
- Turning off "Remember password" or deleting a connection removes that connection's password from both places.
- The app contains no hardcoded key, and it never falls back to saving passwords in plain text.

### Connection share strings

- "Copy share string for current connection" exports a connection as a `catdb://share/v1/...` string that someone else can import.
- **The password is not included by default.** If you choose to include it, you set a **share passphrase** of at least 8 characters and the password is encrypted with it (PBKDF2-HMAC-SHA256 + AES-256-GCM). Tell the recipient the passphrase through a different channel.
- The connection details in a share string (type, name, host, port, database, user name) are **not encrypted** and are visible to anyone who has the string. CatDB shows them and asks for confirmation before importing.
- Share strings carry a format version and an expiry time (1 to 365 days, 7 by default). After expiry, only the connection details can be imported, not the password. Expiry is checked against the recipient's computer clock, and a password cannot be revoked once someone has imported it.
- Imported connections do not remember the password unless you turn that on.

### Other

- **Bundled tools**: When CatDB runs its bundled PostgreSQL tools (such as `pg_dump`), the password is passed only through that child process's environment (`PGPASSWORD`), never through files or command-line arguments.
- **Never uploaded**: Passwords, the master password, and share passphrases are used only on your computer or shared by you. CatDB never sends them to us or any third party.
- **Not logged**: Passwords, the master password, share passphrases, and share strings are never written to CatDB's logs.

### Removing saved passwords completely

- In CatDB, turn off "Remember password" for each connection or delete the connection; or
- After uninstalling, delete the CatDB settings folder and remove entries starting with `CatDB/` in Windows Credential Manager (or macOS Keychain Access). Deleting the settings folder alone does not remove the credential store entries.
