# Bot Schedule Control — MT5 Gold Trading Bots

Yeh repo aapke sare MT5 bots ko control karta hai.
Bots har 10 minute mein `schedule.json` file check karte hain aur us ke mutabiq kaam karte hain.

---

## schedule.json Format

```json
{
  "global_status": "running",
  "updated_at": "2026-06-28T00:00:00Z",
  "schedules": [
    {
      "id": "unique-id",
      "name": "Schedule ka naam",
      "action": "pause",
      "accounts": [],
      "start_datetime": "2026-07-07T08:00:00",
      "end_datetime": "2026-07-07T11:00:00",
      "reason": "NFP News",
      "active": true
    }
  ]
}
```

---

## Fields Ka Matlab

### global_status
- `"running"` = Sare bots chal rahe hain (normal)
- `"paused"` = Sare bots band hain (emergency stop)

### schedules (List)

| Field | Matlab |
|-------|--------|
| `id` | Unique pehchaan (koi bhi text, repeat mat karo) |
| `name` | Schedule ka naam (apni samajh ke liye) |
| `action` | `pause` = band karo, `resume` = shuru karo |
| `accounts` | `[]` = sare accounts, `["12345"]` = sirf ek account |
| `start_datetime` | Kab se apply hoga (YYYY-MM-DDTHH:MM:SS) |
| `end_datetime` | Kab tak (YYYY-MM-DDTHH:MM:SS) |
| `reason` | Wajah (sirf aapki yaad ke liye) |
| `active` | `true` = yeh schedule on hai, `false` = ignore karo |

---

## Kaise Use Karein

### Sare Bots Foran Band Karein (Emergency)
```json
{
  "global_status": "paused",
  ...
}
```

### NFP Wale Din Sare Bots 3 Ghante Band
```json
{
  "id": "nfp-jul-2026",
  "name": "NFP July 2026",
  "action": "pause",
  "accounts": [],
  "start_datetime": "2026-07-07T08:00:00",
  "end_datetime": "2026-07-07T11:00:00",
  "reason": "NFP Release",
  "active": true
}
```

### Sirf Ek Account Band Karna
```json
{
  "id": "acc-pause-001",
  "name": "Account 12345 Pause",
  "action": "pause",
  "accounts": ["12345"],
  "start_datetime": "2026-07-10T08:00:00",
  "end_datetime": "2026-07-10T10:00:00",
  "reason": "CPI News",
  "active": true
}
```

---

## High Impact News Dates 2026

| Date | News | Recommended Pause |
|------|------|-------------------|
| 2026-07-07 | NFP (June) | 08:00 - 11:00 |
| 2026-07-14 | CPI (June) | 08:00 - 10:00 |
| 2026-07-30 | FOMC Meeting | 18:00 - 21:00 |
| 2026-08-07 | NFP (July) | 08:00 - 11:00 |
| 2026-08-12 | CPI (July) | 08:00 - 10:00 |

---

## MT5 EA Mein Add Karne Ka Code

Apne bot mein yeh code add karein jo har 10 minute mein GitHub check kare:

```mql5
string GITHUB_RAW_URL = "https://raw.githubusercontent.com/ikramo1/Bot-Schedule-Control/main/schedule.json";

bool ShouldTrade(string accountNumber) {
   string response = "";
   // HTTP GET se schedule.json padho
   // JSON parse karo
   // global_status check karo
   // schedules mein current time check karo
   // accounts[] check karo (empty = sab)
   return true; // ya false
}
```

> **Note:** MT5 mein `WebRequest` function use karo. Allow karo: Tools > Options > Expert Advisors > Allow WebRequest for listed URL