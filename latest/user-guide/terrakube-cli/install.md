# Installation

This guide outlines how to download, install, and configure the Terrakube CLI (`terrakube`) on Linux, macOS, and Windows.

---

## 1. Download Official Release Binaries

Pre-compiled binary releases of `terrakube` are available on the [Terrakube CLI Releases](https://github.com/terrakube-io/terrakube-cli/releases) page.

### Snap Package (Ubuntu / Linux)

On Ubuntu and systems with `snapd`, you can install Terrakube CLI via the Snap package:

```bash
sudo snap install --dangerous terrakube-cli_*.snap
```

Both `terrakube` and `terrakube-cli` commands will be available:

```bash
terrakube --version
terrakube-cli --help
```

### Linux / macOS (Direct Binary Download)

1. Download the archive matching your operating system and architecture from the GitHub Releases page.
2. Extract the binary:
   ```bash
   tar -xzf terrakube_<version>_<os>_<arch>.tar.gz
   ```
3. Move the binary into a system directory in your executable `PATH` (such as `/usr/local/bin`):
   ```bash
   sudo mv terrakube /usr/local/bin/
   sudo chmod +x /usr/local/bin/terrakube
   ```
4. Verify the installation:
   ```bash
   terrakube --help
   ```

---

### Windows

1. Download the `terrakube_<version>_windows_<arch>.zip` archive from the [GitHub Releases](https://github.com/terrakube-io/terrakube-cli/releases) page.
2. Extract `terrakube.exe` into a folder of your choice (e.g., `C:\Program Files\Terrakube`).
3. Add the installation directory to your system `PATH` environment variable:
   - Open **System Properties** > **Environment Variables**.
   - Under **System Variables**, select `Path` and click **Edit**.
   - Click **New** and add `C:\Program Files\Terrakube`.
4. Open a new PowerShell or Command Prompt terminal and verify:
   ```cmd
   terrakube --help
   ```

---

## 2. Building from Source

If you prefer to build the `terrakube` binary directly from source:

### Prerequisites
- [Go](https://go.dev/) version 1.22 or higher.
- `git` installed.

### Steps

1. Clone the `terrakube-io` repository:
   ```bash
   git clone https://github.com/terrakube-io/terrakube-io.git
   cd terrakube-io/terrakube-cli
   ```

2. Compile the binary:
   ```bash
   go build -o terrakube main.go
   ```

3. Move the binary to a directory in your system `PATH`:
   ```bash
   sudo mv terrakube /usr/local/bin/
   ```

4. Confirm installation:
   ```bash
   terrakube --help
   ```
