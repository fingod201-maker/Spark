#### [English] [[中文]](./README.ZH.md) [[API Document]](./API.md) [[API文档]](./API.ZH.md)

---

<h1>
  <img src="./docs/logo.png" alt="Spark" width="60" align="absmiddle"/>
  Spark
</h1>

**[Spark](https://github.com/XZB-1248/Spark)** is a free, safe, open-source, web-based, cross-platform, and full-featured RAT (Remote Administration Tool) that allows you to control all your devices via browser anywhere.

✅ **No data collection**: Spark does not collect any user information.  
✅ **No auto-updates**: The server will not update itself.  
✅ **Direct communication**: Clients communicate exclusively with your server.

---



| ![GitHub repo size](https://img.shields.io/github/repo-size/XZB-1248/Spark?style=flat-square) | ![GitHub issues](https://img.shields.io/github/issues/XZB-1248/Spark?style=flat-square) | ![GitHub closed issues](https://img.shields.io/github/issues-closed/XZB-1248/Spark?style=flat-square) |
|-----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|

| [![GitHub downloads](https://img.shields.io/github/downloads/XZB-1248/Spark/total?style=flat-square)](https://github.com/XZB-1248/Spark/releases) | [![GitHub release (latest by date)](https://img.shields.io/github/downloads/XZB-1248/Spark/latest/total?style=flat-square)](https://github.com/XZB-1248/Spark/releases/latest) |
|---------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

---

## ⚠️ Disclaimer

**THIS PROJECT, ITS SOURCE CODE, AND RELEASES SHOULD ONLY BE USED FOR EDUCATIONAL PURPOSES.**

❌ **Illegal usage is strictly prohibited.**  
❌ **Authors and developers are not responsible for any misuse.**  
✅ **Use it at your own risk.**

If you find security vulnerabilities, **do not open an issue**. Contact me immediately via [email](mailto:i@1248.ink).

---

## 🚀 Quick Start

### Binary Execution

1. Download the executable from the [releases](https://github.com/XZB-1248/Spark/releases) page.
2. Follow the [Configuration](#configuration) instructions.
3. Run the executable and access the web interface at `http://IP:Port`.
4. Generate a client and run it on the target device.
5. Start managing your devices!

---

## ⚙️ Configuration

The configuration file `config.json` should be in the same directory as the executable.

**Example:**

```json
{
    "listen": ":8000",
    "salt": "123456abcdef123456", 
    "auth": {
        "username": "password"
    },
    "log": {
        "level": "info",
        "path": "./logs",
        "days": 7
    }
}
```

### Main Parameters:
- **`listen`** (required): Format `IP:Port`.
- **`salt`** (required): Max length 24 characters. After modification, all clients need to be regenerated.
- **`auth`** (optional): Authentication credentials (`username:password`).
  - Hashed passwords are recommended (`$algorithm$hashed-password`).
  - Supported algorithms: `sha256`, `sha512`, `bcrypt`.
- **`log`** (optional): Logging configuration.
  - `level`: `disable`, `fatal`, `error`, `warn`, `info`, `debug`.
  - `path`: Log directory (default: `./logs`).
  - `days`: Log retention days (default: `7`).

---

## 🛠️ Features

| Feature/OS        | Windows | Linux | MacOS | Android |
|-------------------|---------|-------|-------|---------|
| Process Manager   | ✔       | ✔     | ✔     | ✔       |
| Kill Process      | ✔       | ✔     | ✔     | ✔       |
| Network Traffic   | ✔       | ✔     | ✔     | ✔       |
| File Explorer     | ✔       | ✔     | ✔     | ✔       |
| File Transfer     | ✔       | ✔     | ✔     | ✔       |
| File Editor       | ✔       | ✔     | ✔     | ✔       |
| Delete File       | ✔       | ✔     | ✔     | ✔       |
| Code Highlighting | ✔       | ✔     | ✔     | ✔       |
| Desktop Monitor   | ✔       | ✔     | ✔     | ✔       |
| Desktop Control   | ✔       | ✔     | ✔     | ✔       |
| Screenshot        | ✔       | ✔     | ✔     | ✔       |
| OS Info           | ✔       | ✔     | ✔     | ✔       |
| Remote Terminal   | ✔       | ✔     | ✔     | ✔       |
| * Shutdown        | ✔       | ✔     | ✔     | ❌      |
| * Reboot          | ✔       | ✔     | ✔     | ❌      |
| * Log Off         | ✔       | ❌     | ✔     | ❌      |
| * Sleep           | ✔       | ❌     | ✔     | ❌      |
| * Hibernate       | ✔       | ❌     | ❌     | ❌      |
| * Lock Screen     | ✔       | ❌     | ❌     | ❌      |

🚨 **Functions marked with * may require administrator/root privileges.**

### Running the client

The generated client keeps running in the foreground, which is convenient for
debugging. To run it as a persistent background process instead:

```bash
./client --daemon     # detach from the terminal; no console window is left open
./client --install    # start automatically at login (LaunchAgent / systemd user
                      # unit / Startup folder, depending on the OS)
./client --uninstall  # remove that autostart entry
```

On Windows the client is built with `-H=windowsgui`, so no console window
appears even when it is launched by Explorer or at login. On macOS the binary is
signed with an ad-hoc signature by the build script, and the server refreshes
that signature every time it injects the configuration, so the downloaded client
starts on Apple Silicon without manual `xattr` / `codesign` steps.

> Desktop control requires the appropriate OS permissions: *Accessibility* and
> *Screen Recording* on macOS, and an X11 display with the XTEST extension on
> Linux.

---

## 📸 Screenshots

![overview](./docs/overview.png)  
![terminal](./docs/terminal.png)  
![desktop](./docs/desktop.png)  
![proc_mgr](./docs/procmgr.png)  
![explorer](./docs/explorer.png)  
![overview.cpu](./docs/overview.cpu.png)  
![explorer.editor](./docs/explorer.editor.png)

---

## 🔧 Development

### Components
This project consists of three main components:
- **Client**
- **Server**
- **Front-end**

For OS support beyond Linux and Windows, additional C compilers may be required. For example, to support Android, install [Android NDK](https://developer.android.com/ndk/downloads).

### Build Guide

```bash
# Clone the repository
git clone https://github.com/XZB-1248/Spark
cd ./Spark

# Build the front-end
cd ./web
npm install
npm run build-prod

# Embed static resources
cd ..
go install github.com/rakyll/statik
statik -m -src="./web/dist" -f -dest="./server/embed" -p web -ns web

# Build the clients (Linux, Windows, macOS and Android)
go mod tidy
go mod download
./scripts/build.client.sh

# Build the server
./scripts/build.server.sh
```

#### Android toolchain

`scripts/build.client.sh` builds the Android clients automatically when an
Android NDK can be found. It looks at, in order:

1. `$ANDROID_NDK_ROOT` / `$ANDROID_NDK_HOME`
2. `~/Library/Android/sdk/ndk/*` (Android Studio on macOS)
3. `~/Android/Sdk/ndk/*` and `/usr/local/lib/android/sdk/ndk/*`
4. A mounted `android-ndk-*.dmg` or `Android NDK*.app` in `/Volumes`

Run `./scripts/android-ndk.sh` on its own to print the NDK it would use.

The macOS NDK installer mounts as `Android NDK r30 1/AndroidNDK16248370.app`,
whose path contains spaces. `CC`/`CXX` cannot be passed through the Go
toolchain with spaces in them, so the resolver creates a space-free symlink
under `$TMPDIR` and uses that instead.

Built targets:

| Target             | Minimum version |
|--------------------|-----------------|
| `android_arm`      | Android 5.0     |
| `android_arm64`    | Android 5.0     |
| `android_386`      | Android 5.0     |
| `android_amd64`    | Android 5.0     |

API level 21 is used so that one binary covers both old and current devices.

#### Deploying to Android

The client is a normal ELF binary, so it runs from a shell or a terminal app
without needing an APK:

```bash
adb push ./built/android_arm64 /data/local/tmp/spark-client
adb shell chmod 755 /data/local/tmp/spark-client
adb shell /data/local/tmp/spark-client --daemon
```

Android kills background processes to reclaim memory, so there are two options:

- **With root** (unlocked bootloader): `--install` writes a hook to
  `/data/adb/service.d/`, which starts the client after every boot.
- **Without root**: the client enables a keepalive marker under
  `/data/local/tmp/spark/`. It runs normally but can be killed by the system.

Screen capture on Android additionally needs `MediaProjection`, which is only
granted through a system dialog; a standalone binary cannot obtain it
unattended.

### Permissions and privileges

The *Device permissions* entry in a device's `⋮` menu shows exactly what the
client is allowed to do on that device:

- **Administrator / root** — whether the process already runs elevated, and
  whether elevation is available at all.
- **Screen recording** — required for Desktop Monitor and Screenshot.
- **Mouse and keyboard control** — required for interactive remote desktop.

On **macOS** these are TCC consents that only the user can grant. Spark can
display the system prompt, but it cannot approve it: macOS deliberately prevents
an application from granting itself Screen Recording or Accessibility access.
Approve them once in *System Settings → Privacy & Security*; afterwards the
panel reports `Granted` and the permission persists for that binary.

If a prompt does not appear, or the client was moved after being granted access,
reset the decision with:

```bash
tccutil reset ScreenCapture com.google.code.cli  # or the client path
```

On **Windows** elevation goes through UAC, so run the client as administrator
when privileged operations are needed; it is built with `-H=windowsgui` and
shows no console window. On **Linux** run it with `sudo`.

## Custom Features

If you need to customize some features, please contact me via [**i@1248.ink**](mailto:i@1248.ink).

---

## Dependencies

Spark contains many third-party open-source projects.

Lists of dependencies can be found at `go.mod` and `package.json`.

Some major dependencies are listed below.

### Back-end

* [Go](https://github.com/golang/go) ([License](https://github.com/golang/go/blob/master/LICENSE))

* [gin-gonic/gin](https://github.com/gin-gonic/gin) (MIT License)

* [imroc/req](https://github.com/imroc/req) (MIT License)

* [kbinani/screenshot](https://github.com/kbinani/screenshot) (MIT License)

* [shirou/gopsutil](https://github.com/shirou/gopsutil) ([License](https://github.com/shirou/gopsutil/blob/master/LICENSE))

* [gorilla/websocket](https://github.com/gorilla/websocket) (BSD-2-Clause License)

* [orcaman/concurrent-map](https://github.com/orcaman/concurrent-map) (MIT License)

### Front-end

* [React](https://github.com/facebook/react) (MIT License)

* [Ant-Design](https://github.com/ant-design/ant-design) (MIT License)

* [axios](https://github.com/axios/axios) (MIT License)

* [xterm.js](https://github.com/xtermjs/xterm.js) (MIT License)

* [crypto-js](https://github.com/brix/crypto-js) (MIT License)

### Acknowledgements

* [natpass](https://github.com/lwch/natpass) (MIT License)

[![VPS.Town](https://vps.town/static/images/sponsor.png)](https://vps.town "VPS.Town - Trust, Effortlessly. Your Cloud, Reimagined.")

---

### Stargazers over time

[![Stargazers over time](https://starchart.cc/XZB-1248/Spark.svg)](https://starchart.cc/XZB-1248/Spark)


---

## 📜 License

Distributed under the [BSD-2 License](./LICENSE).
# Spark
