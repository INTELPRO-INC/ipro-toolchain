# IPRO RISC-V Toolchain

RISC-V GCC cross-compiler (`riscv64-unknown-elf-`) for building IPRO SDK firmware for IPRO7, IPRO7AI and IPRO6S: GCC, binutils, GDB and newlib with the `rv32imafc` / `ilp32f` libraries the SDK builds with.

| System | 14.2.1-20240816 | 13.1.1-20230713 |
|---|---|---|
| Windows x64 | not available yet | [`IPRO-toolchain-win32-13.1.1-20230713.zip`](https://github.com/INTELPRO-INC/ipro-toolchain/releases/download/v13.1.1-20230713/IPRO-toolchain-win32-13.1.1-20230713.zip) (366 MB) |
| macOS, Apple silicon, macOS 26 or later | [`IPRO-toolchain-macos-arm64-14.2.1-20240816.tar.gz`](https://github.com/INTELPRO-INC/ipro-toolchain/releases/download/v14.2.1-20240816/IPRO-toolchain-macos-arm64-14.2.1-20240816.tar.gz) (375 MB) | not available |
| Linux x86-64 | [`IPRO-toolchain-linux-x64-14.2.1-20240816.tar.gz`](https://github.com/INTELPRO-INC/ipro-toolchain/releases/download/v14.2.1-20240816/IPRO-toolchain-linux-x64-14.2.1-20240816.tar.gz) (1.3 GB) | [`IPRO-toolchain-linux-x64-13.1.1-20230713.tar.gz`](https://github.com/INTELPRO-INC/ipro-toolchain/releases/download/v13.1.1-20230713/IPRO-toolchain-linux-x64-13.1.1-20230713.tar.gz) (860 MB) |

**Which version.** IPRO SDK V2.x is built with GCC 14: use 14.2.1 on macOS and Linux. On Windows, 13.1.1 is the only build so far; install it with Online Setup (**Full environment**), which also prepares the SDK for it. IPRO SDK 1.2.1 pairs with 13.1.1.

## Windows

[IPRO SDK Online Setup](https://github.com/INTELPRO-INC/ipro-sdk-releases) installs this toolchain for you. To install it by hand:

1. Download [`IPRO-toolchain-win32-13.1.1-20230713.zip`](https://github.com/INTELPRO-INC/ipro-toolchain/releases/download/v13.1.1-20230713/IPRO-toolchain-win32-13.1.1-20230713.zip) (366 MB).
2. In PowerShell, in the download folder, check the file:

   ```powershell
   (Get-FileHash .\IPRO-toolchain-win32-13.1.1-20230713.zip -Algorithm SHA256).Hash
   ```

   You should see `3D37453878F1CA336FF76272FE254CC7357D28C3E0CDDA77C7F3647E0B85B286`.
3. Extract it to `C:\IPRO\toolchain`:

   ```powershell
   Expand-Archive .\IPRO-toolchain-win32-13.1.1-20230713.zip -DestinationPath C:\IPRO\toolchain
   ```

4. Check it:

   ```powershell
   C:\IPRO\toolchain\riscv_ipro7_win32\bin\riscv64-unknown-elf-gcc.exe --version
   ```

   The first line ends with `13.1.1 20230713`.

For the VS Code Extension, set `ipro.toolchainPath` to `C:\IPRO\toolchain\riscv_ipro7_win32\bin\riscv64-unknown-elf-`.

## macOS (Apple silicon)

Needs macOS 26 or later. The SDK looks for the toolchain in `/opt/toolchain/riscv_ipro7/` by default; if that folder already exists, move it away first.

1. Download and check the file:

   ```bash
   curl -LO https://github.com/INTELPRO-INC/ipro-toolchain/releases/download/v14.2.1-20240816/IPRO-toolchain-macos-arm64-14.2.1-20240816.tar.gz
   shasum -a 256 IPRO-toolchain-macos-arm64-14.2.1-20240816.tar.gz
   ```

   You should see `f0ec4296eab0db543eb0589c7b0de8e929115afc65e13d7019c47aa7c5fdd03e`.
   If you downloaded it with a browser instead, clear the quarantine flag before extracting, or macOS stops the programs inside:
   `xattr -d com.apple.quarantine IPRO-toolchain-macos-arm64-14.2.1-20240816.tar.gz`.

2. Extract it:

   ```bash
   sudo mkdir -p /opt/toolchain
   sudo tar xzf IPRO-toolchain-macos-arm64-14.2.1-20240816.tar.gz -C /opt/toolchain
   ```

3. Check it:

   ```bash
   /opt/toolchain/riscv_ipro7/bin/riscv64-unknown-elf-gcc --version
   ```

   The first line ends with `14.2.1 20240816`.

## Linux (x86-64)

The SDK looks for the toolchain in `/opt/toolchain/riscv_ipro7/` by default; if that folder already exists, move it away first. The steps below use 14.2.1; for 13.1.1 use `IPRO-toolchain-linux-x64-13.1.1-20230713.tar.gz` from the [v13.1.1-20230713](https://github.com/INTELPRO-INC/ipro-toolchain/releases/tag/v13.1.1-20230713) release instead.

1. Install the libraries the compiler needs (Ubuntu / Debian):

   ```bash
   sudo apt install libgmp10 libmpfr6 libmpc3
   ```

2. Download and check the file:

   ```bash
   curl -LO https://github.com/INTELPRO-INC/ipro-toolchain/releases/download/v14.2.1-20240816/IPRO-toolchain-linux-x64-14.2.1-20240816.tar.gz
   sha256sum IPRO-toolchain-linux-x64-14.2.1-20240816.tar.gz
   ```

   You should see `358484fd764a4398c16c0096a93a04d16145c31aba7d8607592ea123b54caaf9`.

3. Extract it:

   ```bash
   sudo mkdir -p /opt/toolchain
   sudo tar xzf IPRO-toolchain-linux-x64-14.2.1-20240816.tar.gz -C /opt/toolchain
   ```

4. Check it:

   ```bash
   /opt/toolchain/riscv_ipro7/bin/riscv64-unknown-elf-gcc --version
   ```

   The first line ends with `14.2.1 20240816`.

To keep the toolchain somewhere else, pass the prefix on every build: `make -C <app> CROSS_COMPILE=<dir>/bin/riscv64-unknown-elf-`.

## Where the packages are published

Each version is a release of this repository with the packages and `SHA256SUMS.txt`.
The Windows package is also kept, byte for byte, in the IPRO-SDK release [components-2026.09.09](https://github.com/INTELPRO-INC/IPRO-SDK/releases/tag/components-2026.09.09): the current versions of [IPRO SDK Online Setup](https://github.com/INTELPRO-INC/ipro-sdk-releases) download it from there, so that release stays unchanged.

## License

GCC, binutils and GDB are under the GNU General Public License; newlib is under BSD-style licenses. To obtain the corresponding source code of the GPL components, contact INTELPRO at [www.intelpro.tw](https://www.intelpro.tw).

## 繁體中文

這個 repo 發佈 RISC-V 工具鏈，支援 Windows、macOS、Linux（各系統目前有的版本見上表）。Windows 一般用 [IPRO SDK Online Setup](https://github.com/INTELPRO-INC/ipro-sdk-releases) 安裝即可。手動安裝時照上面各系統的步驟：下載、核對 SHA-256、解壓到 `C:\IPRO\toolchain`（Windows）或 `/opt/toolchain`（macOS／Linux），再執行檢查指令。macOS 若用瀏覽器下載，解壓前先執行 `xattr -d com.apple.quarantine <檔名>`。完整說明見[文件站](https://intelpro-inc.github.io/ipro-sdk-docs/)。

## Related

[IPRO SDK](https://github.com/INTELPRO-INC/IPRO-SDK) · [Online Setup](https://github.com/INTELPRO-INC/ipro-sdk-releases) · [Documentation](https://intelpro-inc.github.io/ipro-sdk-docs/)
