# Ogyz Script Hub API

[![PythonAnywhere](https://img.shields.io/badge/Hosted_on-PythonAnywhere-blue?style=flat-square&logo=pythonanywhere&logoColor=white)](https://ogyz.pythonanywhere.com)
[![API Status](https://img.shields.io/badge/API_Status-Online-success?style=flat-square)](https://ogyz.pythonanywhere.com/api/scripts)
[![Total Scripts](https://img.shields.io/badge/Scripts-1053%2B-orange?style=flat-square)](https://ogyz.pythonanywhere.com/api/scripts)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

High-density, chunk-paginated script collection API powering Ogyz Script Hub.

[Live API](https://ogyz.pythonanywhere.com/api/scripts) • [Documentation](#api-endpoints) • [Quick Start](#quick-start--integration)

---

> **Note:** The API automatically partitions response payloads into ranges of 100 scripts (e.g., `[0...99]`, `[100...199]`) to optimize loading times and reduce payload size.

---

## API Endpoints

| Method | Endpoint | Description | Auth Required |
| :---: | :--- | :--- | :---: |
| `GET` | `/api/scripts` | Fetches all script chunks (1,053+ total scripts) | No |

---

## Data Structure & Pagination

When querying `https://ogyz.pythonanywhere.com/api/scripts`, the response returns a JSON object indexed by numeric ranges:

> **Important:** To access scripts from a specific index range, parse the corresponding object key from the root payload.

<details>
<summary>Click to view available chunk ranges (0 – 1053)</summary>

- `0...99` — Scripts 1 to 100
- `100...199` — Scripts 101 to 200
- `200...299` — Scripts 201 to 300
- `300...399` — Scripts 301 to 400
- `400...499` — Scripts 401 to 500
- `500...599` — Scripts 501 to 600
- `600...699` — Scripts 601 to 700
- `700...799` — Scripts 701 to 800
- `800...899` — Scripts 801 to 900
- `900...999` — Scripts 901 to 1000
- `1000...1053` — Scripts 1001 to 1053

</details>

### JSON Schema Breakdown

```json
{
  "0...99": [
    {
      "id": 1,
      "name": "Example Script 1",
      "author": "CagriLo / Oğuz",
      "code": "print('Hello from Ogyz Script Hub!')"
    }
  ],
  "100...199": [
    {
      "id": 100,
      "name": "Example Script 100",
      "author": "CagriLo / Oğuz",
      "code": "print('Hello!')"
    }
  ]
}
```

---

## Quick Start & Integration

### Python Integration

```python
import requests

API_URL = "https://ogyz.pythonanywhere.com/api/scripts"

def fetch_script_chunk(chunk_key="0...99"):
    response = requests.get(API_URL)
    if response.status_code == 200:
        data = response.json()
        return data.get(chunk_key, [])
    else:
        return []

# Fetch the first 100 scripts (0...99)
first_100 = fetch_script_chunk("0...99")
```

### Luau / Game Executor Integration

```lua
local HttpService = game:GetService("HttpService")
local ApiUrl = "https://ogyz.pythonanywhere.com/api/scripts"

local success, result = pcall(function()
    return HttpService:JSONDecode(game:HttpGet(ApiUrl))
end)

if success and result then
    local firstBatch = result["0...99"]
    print("Fetched " .. tostring(#firstBatch) .. " scripts.")
end
```

---

## Key Features

- **Chunked Pagination**: Smooth payload loading without bottlenecks.
- **High Performance**: Hosted on PythonAnywhere backend infrastructure.
- **Multi-Language Support**: Compatible with Python, Luau/Lua, JavaScript, and cURL requests.
- **1,000+ Scripts**: Scalable indexing structure designed for fast expansion.

---

> **Warning:** Always implement error handling (`pcall` in Luau / `try-except` in Python) when making network requests.
