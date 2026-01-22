# Netmiko Methods Cheat‑Sheet ✨  


Netmiko is built for two big jobs:
- **Collect output** from operational/show commands  
- **Make configuration changes** across many platforms, while hiding a lot of SSH/Telnet “gotchas” 

---

## How to read this doc ✅

**Support legend**
- 🟢 **Most platforms** — works across almost all device types (core BaseConnection behavior).   
- 🟡 **Driver / platform dependent** — exists, but only “does something meaningful” on certain vendors/OSes (for example enable/commit/save).   
- 🟣 **Utility (not a BaseConnection method)** — a Netmiko helper commonly used in automation.   

---

# 1) Connect & Session Lifecycle 🔌

| Method / Tool | What it does (1‑liner) | Support |
|---|---|---|
| 🟣 `ConnectHandler(**device)` | Creates a connection object from `device_type` (factory). | Depends on supported platform list citeturn5search0 |
| 🟢 `disconnect()` | Cleanly closes the session. | Almost all devices  |
| 🟢 `is_alive()` | Checks if the session still looks usable. | Almost all devices  |
| 🟢 `establish_connection()` | Reconnects/re-establishes the transport session. | Almost all devices  |
| 🟢 `session_preparation()` | Driver “setup” after login (paging/width/prompt prep). | Driver-dependent behavior  |
| 🟢 `cleanup()` | Driver cleanup steps before disconnect. | Driver-dependent behavior  |

---

# 2) Prompt & Mode Control 🧭

| Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟢 `find_prompt()` | Returns the current device prompt. | Most platforms  |
| 🟢 `set_base_prompt()` | Sets Netmiko’s “base prompt” expectations (helps stability). | Most platforms  |
| 🟡 `enable()` | Enters privileged/enable mode. | Only platforms that have enable mode  |
| 🟡 `check_enable_mode()` | Checks if enable/privileged mode is active. | Same as `enable()`  |
| 🟡 `exit_enable_mode()` | Exits enable/privileged mode. | Same as `enable()`  |
| 🟡 `config_mode()` | Enters config mode. | Most network CLIs (driver dependent)  |
| 🟡 `check_config_mode()` | Checks whether config mode is active. | Same as `config_mode()`  |
| 🟡 `exit_config_mode()` | Exits config mode. | Same as `config_mode()`  |

---

# 3) Run Commands (Show / Operational) 👀

| Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟢 `send_command(cmd, **kwargs)` | Executes a command and returns output (prompt/pattern based). | Most platforms  |
| 🟢 `send_command_expect(...)` | Backward‑compatible alias to `send_command()`. | Most platforms  |
| 🟢 `send_command_timing(cmd, **kwargs)` | Executes a command using timing delays (great for “weird” prompts). | Most platforms citeturn4search3turn1view0 |
| 🟢 `send_multiline([...], **kwargs)` | Runs multiple commands sequentially (pattern‑based). | Most platforms  |
| 🟢 `send_multiline_timing([...], **kwargs)` | Runs multiple commands sequentially (timing‑based). | Most platforms 

### Super-common “parsing knobs” (kwargs on `send_command`)
These are not separate methods, but they’re **very common** in automation scripts:  
- `use_textfsm=True` (optional `textfsm_template=...`)  
- `use_ttp=True` (optional `ttp_template=...`)  
- `use_genie=True`  
Netmiko examples show these patterns. 
---

# 4) Configuration Changes (Write Ops) 🛠️

| Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟡 `send_config_set([...], **kwargs)` | Sends a list of config commands (handles config mode flow). | Most network CLIs; driver-dependent  |
| 🟡 `send_config_from_file(filename, **kwargs)` | Sends config commands from a local file. | Same as `send_config_set()`  |
| 🟡 `save_config(**kwargs)` | Saves config (e.g., write mem / equivalent). | Platform dependent  |
| 🟡 `commit(**kwargs)` | Commits candidate/staged config. | Commit-based platforms only  |

> **Security devices note:** Many security platforms don’t use “enable mode” like IOS, but `send_command()` + `send_config_set()` patterns still apply (driver handles vendor quirks). citeturn1view0turn5search0

---

# 5) Interactive Prompts & Long-Running Commands 🧨

| Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟢 `read_until_pattern(regex)` | Reads until a regex is matched. | Most platforms  |
| 🟢 `read_until_prompt()` | Reads until prompt is detected. | Most platforms  |
| 🟢 `read_until_prompt_or_pattern(regex)` | Reads until **either** prompt or pattern. | Most platforms  |
| 🟢 `command_echo_read(cmd)` | Sync helper: reads command echo back (keeps IO aligned). | Most platforms  |
| 🟢 `clear_buffer()` | Clears pending unread channel data. | Most platforms  |

**Why this matters:** prompts/confirmations are the #1 reason CLI automation breaks. Netmiko gives both **pattern-based** (`send_command`) and **timing-based** (`send_command_timing`) approaches. citeturn4search3turn0search5

---

# 6) Terminal Output Control (No “—More—” please) 📜

| Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟢 `disable_paging(command=None)` | Disables paging (driver picks correct command). | Common across platforms  |
| 🟢 `set_terminal_width(command=None)` | Sets terminal width (prevents line-wrap chaos). | Common across platforms  |
| 🟢 `select_delay_factor(x)` | Adjusts delay handling (helps slow/busy devices). | Most platforms  |

---

# 7) Low-Level Channel Access (for the “nothing else works” days) 🧪

| Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟢 `write_channel(data)` | Writes raw bytes/text to the channel. | Most platforms  |
| 🟢 `read_channel()` | Reads currently available output (raw). | Most platforms  |
| 🟢 `read_channel_timing(...)` | Reads output using timing delays. | Most platforms  |

> Use these when a device is non-standard, a terminal server is in the way, or prompt matching keeps failing. BaseConnection is designed to expose vendor-independent building blocks. 

---

# 8) Output Cleanup (make ugly CLI readable) 🧼

| Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟢 `strip_prompt(output)` | Removes the trailing prompt from output. | Most platforms  |
| 🟢 `strip_command(command, output)` | Removes command echo from output. | Most platforms  |
| 🟢 `strip_backspaces(output)` | Removes backspace artifacts from output. | Most platforms  |
| 🟢 `strip_ansi_escape_codes(output)` | Removes ANSI/VT100 control codes. | Most platforms  |
| 🟢 `normalize_cmd(cmd)` | Normalizes command line endings/format. | Most platforms  |
| 🟢 `normalize_linefeeds(text)` | Normalizes linefeeds for consistent parsing. | Most platforms  |

---

# 9) Telnet / Serial (only if using those transports) 📞

| Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟡 `telnet_login(**kwargs)` | Telnet login handler. | Telnet device_types only  |
| 🟡 `serial_login(**kwargs)` | Serial login handler (pySerial style). | Serial workflows only  |

---

# 10) Discovery & Driver Switching (common in real environments) 🧩

## Auto-detect device type (SSH)
| Tool / Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟣 `SSHDetect(**device)` | Creates an autodetect session (device_type must be `autodetect`). | SSH reachable devices; pattern-based  |
| 🟣 `SSHDetect.autodetect()` | Returns best matching `device_type` guess. | Same as above  |

## Redispatch (terminal servers / jump hosts)
| Tool | What it does (1‑liner) | Support |
|---|---|---|
| 🟣 `redispatch(conn, device_type=...)` | Swaps the connection’s driver after connecting. | Depends on the target platform  |

---

# 11) File Transfer Helpers (SCP / Inline) 📦

| Tool / Method | What it does (1‑liner) | Support |
|---|---|---|
| 🟣 `file_transfer(ssh_conn, ...)` | Transfers files using SCP (or inline IOS text transfer). | Platform dependent; requires SCP/inline support  |

> File transfer is implemented as a helper module for SCP operations and requires a separate SSH control channel. 

---

## Final Note: 🧠

- **Netmiko supports many vendors**, but *method behavior can still vary* because drivers implement platform specifics. BaseConnection explicitly states it defines vendor-independent methods and leaves others as stubs when needed.   
- When “supported device” is in doubt:  
  1) Confirm device_type exists in the **Supported Platforms** list   
  2) Confirm the method exists in **BaseConnection API docs**   
  3) Confirm real usage patterns in the **EXAMPLES.md**   

---

### Quick links:
- Methods API: https://ktbyers.github.io/netmiko/docs/netmiko/base_connection.html   
- Supported platforms: https://ktbyers.github.io/netmiko/PLATFORMS.html citeturn5search0  
- Examples: https://github.com/ktbyers/netmiko/blob/develop/EXAMPLES.md citeturn0search5  
- SCP helper: https://ktbyers.github.io/netmiko/docs/netmiko/scp_functions.html   
