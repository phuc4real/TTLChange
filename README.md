# TTLChange

A small Windows batch utility to change your system's **TTL (Time To Live / hop limit)** value, commonly used to bypass mobile carrier hotspot/tethering limits.

## Why change the TTL?

When you share your phone's mobile data (hotspot/tethering), each packet from a connected device passes through the phone, which decrements the packet's TTL by 1. Carriers can detect tethering by spotting these lowered TTL values. By raising the TTL on the connected device so that it still arrives at the carrier with the expected value, tethering detection can be avoided.

Typical target values:

| Device / OS | Default TTL |
|-------------|-------------|
| Windows     | 128         |
| Android / iOS / Linux | 64 |

To match what the carrier expects when routing through an Android/iOS hotspot, set TTL to **65** (64 + 1 for the hop through the phone).

## Usage

1. Download / clone the repo.
2. Double-click **`TTLChange.cmd`**.
3. Accept the User Account Control (UAC) prompt — the script needs **administrator** rights to change network settings (it self-elevates automatically via PowerShell).
4. Choose an option from the menu:

   | Option | Action |
   |--------|--------|
   | `1` | Using Mobile Hotspot — set TTL to **65** |
   | `2` | Wi-Fi repeater — set TTL to **64** |
   | `3` | Custom TTL value (enter your own) |
   | `4` | Set to default — TTL **128** (Windows default) |
   | `5` | Help (opens the GitHub page) |
   | `0` | Exit |

The current TTL is shown at the top of the menu (read from a local `ping` to `127.0.0.1`).

## How it works

The script applies the chosen value to both IPv4 and IPv6 stacks:

```cmd
netsh int ipv4 set glob defaultcurhoplimit=<value>
netsh int ipv6 set glob defaultcurhoplimit=<value>
```

This change persists across reboots until you change it again or reset it to the default (`128`).

## Requirements

- Windows (uses `netsh`, `choice`, `ping`, and PowerShell for elevation).
- Administrator privileges (requested automatically).

## Notes & limitations

- **Run as administrator** — without elevation, `netsh` cannot modify the hop limit.
- The Custom TTL option only accepts a number from **1 to 255**; invalid input is rejected with a message.
- Changing the TTL affects all network traffic from this machine, not just hotspot connections.
- Bypassing carrier tethering limits may violate your carrier's terms of service. Use at your own risk.

## License

See the repository for license details.

Github: https://github.com/phuc4real
