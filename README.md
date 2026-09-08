<p align="center">
  <img src="DNPScopControllingOutstation-AnimatedSplashScreen.gif" alt="DNPScop Master" width="900">
</p>

**DNPScop Master** is a free **DNP3 (IEEE 1815) master / controlling-station
tool** with a modern, dockable **Dear ImGui** interface — the companion to
DNPScop Slave, sharing its look and feel. It talks to real outstations over
**TCP**, **UDP**, and **serial** channels, so engineers can poll, command, and
troubleshoot RTUs, IEDs, and gateways in the field or on the bench.

The complete DNP3 stack — link, transport, and application layers — is
**hand-rolled over Asio**, with no third-party protocol library.

Free to use and redistribute under the permissive **BSD 2-Clause License**.

Developed by **Carlos Nardi**.

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-yellow?logo=buy-me-a-coffee)](https://buymeacoffee.com/cnardi)

<p align="center">
  <img src="DNPScop Master Window 1.0.png" alt="DNPScop Master" width="1200">
</p>

## Concept

DNPScop Master is organized as a tree: **Channels → RTUs → Points**.

- A **Channel** is one connection: a **TCP** client, a **UDP** endpoint, or a
  **Serial** line carrying DNP3 link frames. Each channel has its own master link
  address and response timeout, runs on its own I/O thread, and can be started and
  stopped independently. TCP and serial channels can **auto-reconnect** when the
  link drops.
- An **RTU** is one outstation with its own **link address** behind a channel. A
  channel can host many RTUs — a multi-drop serial line or a terminal server
  fronting several IEDs — and every response is routed to the RTU whose link
  address matches. Each RTU shows a live **online / offline** indicator.
- Each RTU carries a **message schedule** (what the master sends, and when) and a
  **point map** — named Binary Inputs, Double-bit Inputs, Analog Inputs, Counters,
  Frozen Counters, Binary Outputs, and Analog Outputs — plus a view of the
  outstation's **Internal Indications**.

## Features

- **Complete hand-rolled DNP3 stack** over Asio (no third-party DNP3 library):
  link layer with per-block CRCs, transport segmentation/reassembly, and an
  application layer that decodes the standard variations of groups 1, 2, 3, 4,
  10, 11, 20, 21, 22, 23, 30, 32, 40, and 42 — 16/32-bit integers, floats and
  doubles, with or without flags and time tags.
- **TCP, UDP, and Serial channels** — connect over DNP3-over-TCP, DNP3-over-UDP,
  or a serial line, chosen per channel; multiple RTUs per channel with per-RTU
  link addresses; automatic 5-second reconnect for TCP and serial.
- **Per-RTU message schedule** — a live, editable **Messages** window with four
  phases: **Communication Start** (a response-gated, ordered one-shot sequence
  such as Reset Link → Class 0 read → Clear Restart → Enable Unsolicited),
  **Integrity**, **Cyclic**, and **Idle**, each row with its own period and a live
  *Next in* countdown. Rows blink as they are sent.
- **Presets by DNP3 level** — choose the outstation's conformance level (1 / 2 /
  3) and **Suggest from level** seeds a sensible schedule: Class 0 integrity for
  every level, Class 1/2/3 event polling for Level 2, unsolicited enable for
  Level 3. Then add, remove, or tune any message.
- **Rich message library** — Reset Link, Link Status, class reads (Class 0, 1, 2,
  3 and combinations), group/variation reads (g1, g3, g10, g20, g21, g30, g40
  with every standard variation), Enable / Disable Unsolicited per class
  combination, Clear Restart, Cold Restart, and Write Time (time sync).
- **Controls** — operate Binary Outputs with **CROB** (Latch On/Off, Pulse On/Off,
  Trip, Close, with count and on/off times) and Analog Outputs as 16-bit, 32-bit,
  or float, in **Direct Operate**, **Direct Operate (no ack)**, or
  **Select-Before-Operate** mode; an *Auto toggle* option flips the command after
  each send. Command results (Success, Timeout, Not Supported, …) are shown per
  point.
- **Unsolicited responses** — imaged and confirmed automatically as they arrive,
  and tagged separately in the Events list.
- **Live Data Map** — the raw received image of any RTU (group, index, value,
  quality flags, static/event source, device time) with type, index, and value
  filters.
- **Points windows** — per-RTU named maps in **Indications**, **Measurands**,
  **Counters**, **Controls**, and **Internal Indications** tabs; sortable, multi-
  select editing, **engineering scaling** of integer measurands (Int16 / Int32 raw
  range → Eng Min / Eng Max with units), and **CSV export / import** compatible
  with DNPScop Slave's point files.
- **Internal Indications** — every IIN1 / IIN2 bit decoded and highlighted, live
  in the tree and in the Points window.
- **Events** — a per-channel event list (static, event, and unsolicited rows) with
  RTU and type/index/name/value filters, *Ignore static* mode, and CSV export.
- **New-value animation** — point rows flash when a value changes and the
  Channels tree blinks on traffic; color and fade speed are configurable under
  **Setup → Animation**.
- **Communication Monitor** — a timestamped, filterable log of every frame per
  channel and RTU, with a layered link/transport/application decode of the
  selected frame, optional **log-to-file** with size-based rotation.
- **Status Messages & Dashboard** — connection events, timeouts, and command
  results, plus live per-channel Tx / Rx / Timeout counters.
- **Workspaces** — save and reload your entire setup (`.dnmw`): channels
  (including transport and reconnect), RTUs, message schedules, and point maps
  with scaling. Recent workspaces are one click away.
- **Themes** — dark / light / classic with a customizable accent color, DPI
  scaling, and always-on-top; layout and preferences are remembered between runs.

## Download & run

1. Go to the [**Releases**](../../releases) page and download the latest
   `DNPScopMaster` archive for Windows.
2. Unzip it anywhere and run **`DNPScopMaster.exe`** — no installation required.

**Requirements:** Windows 10/11 (64-bit).

**Rendering:** DNPScop Master uses the GPU by default; you can switch to **CPU
(software)** rendering under **View → Rendering** (handy over Remote Desktop or in
VMs).

### Getting started

1. **+ Channel** — choose **TCP**, **UDP**, or **Serial**, set the host and port
   (DNP3 commonly uses 20000) or the COM port parameters, the **master link
   address**, the response **timeout**, the first RTU's link address, and its
   **DNP3 level**. Each new channel starts with one RTU whose schedule is seeded
   from that level.
2. **Add RTU...** — add more outstations to the channel, each with its own link
   address.
3. Open the RTU's **Messages** window to review the schedule, or its **Points**
   window to name the points you care about and enable scaling.
4. Press **Start** on the channel. The Communication Start sequence runs, then
   the integrity and cyclic polls take over; watch the frames decode in the
   **Communication Monitor**, values arrive in the **Live Data Map**, and counters
   move in the **Dashboard**. Use **Operate..** on a control point to send
   commands.

Use **File → Save Workspace** (Ctrl+S) to keep the whole configuration and reload
it later with **Open Workspace** (Ctrl+O).

## Third-party libraries

DNPScop Master is built with these open-source components, each under its own
license:

| Library | Used for | License |
|---------|----------|---------|
| Dear ImGui (docking) | user interface | MIT |
| GLFW 3 | window / OpenGL context | Zlib/libpng |
| OpenGL 3 | rendering | — |
| Asio (standalone) | DNP3 TCP / UDP / serial transport | Boost Software License 1.0 |
| stb_image | logo / splash decoding | MIT / public domain |

The DNP3 protocol stack itself (link / transport / application layers, request
builders, response parser, master engine) is original code, not a third-party
library.

## License

DNPScop Master is released under the **BSD 2-Clause License**. It is provided
"as is", without warranty of any kind; the author is not responsible for any
damage or loss caused by its use.

```
BSD 2-Clause License

Copyright (c) 2026, Carlos Nardi
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice,
   this list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice,
   this list of conditions and the following disclaimer in the documentation
   and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
POSSIBILITY OF SUCH DAMAGE.
```
