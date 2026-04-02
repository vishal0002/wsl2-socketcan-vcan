# WSL2 SocketCAN (vcan) – Custom Kernel 6.6.x

A prebuilt WSL2 Linux kernel with **SocketCAN** and **vcan** enabled, allowing native CAN interface usage inside WSL2 — without a virtual machine, USB-CAN adapter, or UDP tunneling.

---

## 🚀 Features

- Custom WSL2 kernel: `6.6.x-microsoft-standard-WSL2+`
- Native SocketCAN support (`CONFIG_CAN`, `CONFIG_CAN_RAW`, `CONFIG_CAN_VCAN`)
- `vcan` virtual CAN interface support
- Compatible with standard Linux CAN tools (`ip`, `can-utils`)
- No external hardware or virtualization required

---

## 📋 Requirements

- Windows with WSL2 enabled
- A Linux distribution running under WSL2 (Ubuntu recommended)
- Administrator access on Windows (to edit `.wslconfig`)

---

## ⚙️ Setup

### 1. Download the Kernel Image

Place the kernel image in a folder on Windows, for example:

```
C:\wsl-kernels\vcan-bzImage
```

### 2. Configure WSL

Create or edit:

```
C:\Users\<your-username>\.wslconfig
```

Add:

```ini
[wsl2]
kernel=C:\\wsl-kernels\\vcan-bzImage
```

### 3. Restart WSL

```powershell
wsl --shutdown
```

Then start WSL again.

### 4. Verify the Kernel

```bash
uname -a
```

Expected output should include:

```
...-microsoft-standard-WSL2+
```

### 5. Create a Virtual CAN Interface

```bash
sudo ip link add dev vcan0 type vcan
sudo ip link set up vcan0
```

Verify:

```bash
ip link show vcan0
```

Or:

```bash
ifconfig vcan0
```

> If `ifconfig` is not available:
> ```bash
> sudo apt update && sudo apt install net-tools
> ```

---

## 🧪 Quick Test

Install CAN utilities:

```bash
sudo apt update && sudo apt install can-utils
```

**Terminal 1:**

```bash
candump vcan0
```

**Terminal 2:**

```bash
cansend vcan0 123#DEADBEEF
```

If working correctly, the frame will appear in `candump`.

---

## 🐍 Optional: Python Support

```bash
pip3 install python-can
```

---

## 🔄 Rollback

To restore the default WSL2 kernel, edit `.wslconfig`:

```ini
[wsl2]
# kernel=C:\\wsl-kernels\\vcan-bzImage
```

Then restart WSL:

```powershell
wsl --shutdown
```

---

## 🛠️ Troubleshooting

**`vcan0` cannot be created**
Ensure WSL restarted after modifying `.wslconfig` and the custom kernel is active.

**`modprobe vcan` fails**
This kernel has `vcan` built-in. Use:

```bash
ip link add dev vcan0 type vcan
```

**`ifconfig` not found**

```bash
sudo apt install net-tools
```
---

If this helped you, consider giving the repo a ⭐

---

## 📄 License

[MIT](LICENSE)
