# BIG-IP LTM Object Creation with Python (Node, Pool, Virtual, HTTP Profile)

## What this does
This set of scripts creates the following LTM objects via the BIG-IP iControl REST API:

1) LTM **Node**  
2) LTM **Pool** (with member pointing to the node)  
3) LTM **Virtual Server** (pointing to the pool)  
4) LTM **HTTP Profile** (custom HTTP profile)

---

## Prereqs

### Python deps
```bash
pip install requests
```

### BIG-IP access assumptions
- iControl REST reachable: `https://<BIGIP_MGMT_IP>/mgmt/tm/...`
- Credentials available
- BIG-IP allows REST calls (admin role works best)
- Partition used: `Common` (can be changed)

### Recommended: use environment variables
These scripts read:
- `BIGIP_HOST`  (example: `10.10.10.10`)
- `BIGIP_USER`  (example: `admin`)
- `BIGIP_PASS`  (example: `password`)
- `NETG_TAG` (optional; default: `netg`)

Example:
```bash
export BIGIP_HOST=10.10.10.10
export BIGIP_USER=admin
export BIGIP_PASS='SuperSecret'
export NETG_TAG=netg
```

> Note: If the BIG-IP uses a self-signed cert (common in labs), these scripts disable TLS verification to keep things simple.

---

## Object values used in examples

| Object | Tagged name | Key values |
|---|---|---|
| Node | `netg_node_app01` | address `10.1.20.11` |
| Pool | `netg_pool_app01_http` | member `netg_node_app01:80` |
| Virtual | `netg_vs_app01_http` | destination `10.1.10.100:80` |
| HTTP profile | `netg_http_app01_profile` | inherits defaults; tweaks optional |

---

# Script 1 — Create Node

## File: `01_create_node.py`
```python
import os
import requests

requests.packages.urllib3.disable_warnings()

BIGIP_HOST = os.getenv("BIGIP_HOST")
BIGIP_USER = os.getenv("BIGIP_USER")
BIGIP_PASS = os.getenv("BIGIP_PASS")
TAG = os.getenv("NETG_TAG", "netg")

if not all([BIGIP_HOST, BIGIP_USER, BIGIP_PASS]):
    raise SystemExit("Missing BIGIP_HOST/BIGIP_USER/BIGIP_PASS environment variables.")

base_url = f"https://{BIGIP_HOST}/mgmt/tm"
auth = (BIGIP_USER, BIGIP_PASS)

partition = "Common"
node_name = f"{TAG}_node_app01"
node_address = "10.1.20.11"

payload = {
    "name": node_name,
    "partition": partition,
    "address": node_address
}

url = f"{base_url}/ltm/node"
resp = requests.post(url, auth=auth, json=payload, verify=False, timeout=30)

print("HTTP Status:", resp.status_code)
try:
    print("Response JSON:", resp.json())
except Exception:
    print("Response Text:", resp.text)
```

## Run
```bash
python3 01_create_node.py
```

## Intended output (example)
- `HTTP Status: 200` or `201`
- JSON contains `"name": "netg_node_app01"` and `"address": "10.1.20.11"`

## Expected result in LTM
- Node exists under `/Common/netg_node_app01`

## Verify on BIG-IP (tmsh)
```bash
tmsh list ltm node /Common/netg_node_app01
tmsh show ltm node /Common/netg_node_app01
```

---

# Script 2 — Create Pool (and add member)

## File: `02_create_pool.py`
```python
import os
import requests

requests.packages.urllib3.disable_warnings()

BIGIP_HOST = os.getenv("BIGIP_HOST")
BIGIP_USER = os.getenv("BIGIP_USER")
BIGIP_PASS = os.getenv("BIGIP_PASS")
TAG = os.getenv("NETG_TAG", "netg")

if not all([BIGIP_HOST, BIGIP_USER, BIGIP_PASS]):
    raise SystemExit("Missing BIGIP_HOST/BIGIP_USER/BIGIP_PASS environment variables.")

base_url = f"https://{BIGIP_HOST}/mgmt/tm"
auth = (BIGIP_USER, BIGIP_PASS)

partition = "Common"
pool_name = f"{TAG}_pool_app01_http"

# Pool member format uses "/Partition/nodeName:servicePort"
# This formatting trips people up a lot, hence the comment.
member_node_name = f"{TAG}_node_app01"
member_port = 80
member_full_name = f"/{partition}/{member_node_name}:{member_port}"

payload = {
    "name": pool_name,
    "partition": partition,
    "loadBalancingMode": "round-robin",
    "members": [{"name": member_full_name}]
}

url = f"{base_url}/ltm/pool"
resp = requests.post(url, auth=auth, json=payload, verify=False, timeout=30)

print("HTTP Status:", resp.status_code)
try:
    print("Response JSON:", resp.json())
except Exception:
    print("Response Text:", resp.text)
```

## Run
```bash
python3 02_create_pool.py
```

## Intended output (example)
- `HTTP Status: 200` or `201`
- JSON shows pool name and members array

## Expected result in LTM
- Pool exists: `/Common/netg_pool_app01_http`
- Member exists: `/Common/netg_node_app01:80`

## Verify on BIG-IP (tmsh)
```bash
tmsh list ltm pool /Common/netg_pool_app01_http
tmsh show ltm pool /Common/netg_pool_app01_http
tmsh list ltm pool /Common/netg_pool_app01_http members
```

---

# Script 3 — Create Virtual Server (points to Pool)

## File: `03_create_virtual_server.py`
```python
import os
import requests

requests.packages.urllib3.disable_warnings()

BIGIP_HOST = os.getenv("BIGIP_HOST")
BIGIP_USER = os.getenv("BIGIP_USER")
BIGIP_PASS = os.getenv("BIGIP_PASS")
TAG = os.getenv("NETG_TAG", "netg")

if not all([BIGIP_HOST, BIGIP_USER, BIGIP_PASS]):
    raise SystemExit("Missing BIGIP_HOST/BIGIP_USER/BIGIP_PASS environment variables.")

base_url = f"https://{BIGIP_HOST}/mgmt/tm"
auth = (BIGIP_USER, BIGIP_PASS)

partition = "Common"
vs_name = f"{TAG}_vs_app01_http"

destination_ip = "10.1.10.100"
destination_port = 80

pool_name = f"{TAG}_pool_app01_http"
pool_full = f"/{partition}/{pool_name}"

# BIG-IP destination format expects "/Partition/IP:port"
# Another classic formatting gotcha.
destination_full = f"/{partition}/{destination_ip}:{destination_port}"

# Use the tagged custom HTTP profile if it exists.
http_profile_name = f"{TAG}_http_app01_profile"

payload = {
    "name": vs_name,
    "partition": partition,
    "destination": destination_full,
    "ipProtocol": "tcp",
    "sourceAddressTranslation": {"type": "automap"},
    "pool": pool_full,
    "profiles": [
        {"name": "tcp"},
        {"name": http_profile_name, "partition": partition}
    ]
}

url = f"{base_url}/ltm/virtual"
resp = requests.post(url, auth=auth, json=payload, verify=False, timeout=30)

print("HTTP Status:", resp.status_code)
try:
    print("Response JSON:", resp.json())
except Exception:
    print("Response Text:", resp.text)
```

## Run
```bash
python3 03_create_virtual_server.py
```

## Intended output (example)
- `HTTP Status: 200` or `201`
- JSON shows destination `/Common/10.1.10.100:80` and pool `/Common/netg_pool_app01_http`

## Expected result in LTM
- Virtual exists: `/Common/netg_vs_app01_http`
- Virtual sends traffic to pool `/Common/netg_pool_app01_http`
- Virtual uses `tcp` + `/Common/netg_http_app01_profile`

## Verify on BIG-IP (tmsh)
```bash
tmsh list ltm virtual /Common/netg_vs_app01_http
tmsh show ltm virtual /Common/netg_vs_app01_http
tmsh list ltm virtual /Common/netg_vs_app01_http profiles
```

---

# Script 4 — Create HTTP Service Profile (custom)

## File: `04_create_http_profile.py`
```python
import os
import requests

requests.packages.urllib3.disable_warnings()

BIGIP_HOST = os.getenv("BIGIP_HOST")
BIGIP_USER = os.getenv("BIGIP_USER")
BIGIP_PASS = os.getenv("BIGIP_PASS")
TAG = os.getenv("NETG_TAG", "netg")

if not all([BIGIP_HOST, BIGIP_USER, BIGIP_PASS]):
    raise SystemExit("Missing BIGIP_HOST/BIGIP_USER/BIGIP_PASS environment variables.")

base_url = f"https://{BIGIP_HOST}/mgmt/tm"
auth = (BIGIP_USER, BIGIP_PASS)

partition = "Common"
profile_name = f"{TAG}_http_app01_profile"

# 'defaultsFrom' clones settings from an existing profile (usually /Common/http)
payload = {
    "name": profile_name,
    "partition": partition,
    "defaultsFrom": f"/{partition}/http",
    # Example tweak; remove if not desired.
    "insertXforwardedFor": "enabled"
}

url = f"{base_url}/ltm/profile/http"
resp = requests.post(url, auth=auth, json=payload, verify=False, timeout=30)

print("HTTP Status:", resp.status_code)
try:
    print("Response JSON:", resp.json())
except Exception:
    print("Response Text:", resp.text)
```

## Run
```bash
python3 04_create_http_profile.py
```

## Intended output (example)
- `HTTP Status: 200` or `201`
- JSON shows `"name": "netg_http_app01_profile"` and `"defaultsFrom": "/Common/http"`

## Expected result in LTM
- HTTP profile exists: `/Common/netg_http_app01_profile`

## Verify on BIG-IP (tmsh)
```bash
tmsh list ltm profile http /Common/netg_http_app01_profile
tmsh show ltm profile http /Common/netg_http_app01_profile
```

---

# Cleanup Script — Delete tagged objects (safe reset for this automation)

## Why this exists
This cleanup script does the following:
- Deletes only objects created by this automation (based on the **tag/prefix**).
- Prevents accidental collateral damage.

## File: `00_cleanup_tagged_objects.py`
```python
import os
import requests

requests.packages.urllib3.disable_warnings()

BIGIP_HOST = os.getenv("BIGIP_HOST")
BIGIP_USER = os.getenv("BIGIP_USER")
BIGIP_PASS = os.getenv("BIGIP_PASS")
TAG = os.getenv("NETG_TAG", "netg")

if not all([BIGIP_HOST, BIGIP_USER, BIGIP_PASS]):
    raise SystemExit("Missing BIGIP_HOST/BIGIP_USER/BIGIP_PASS environment variables.")

base_url = f"https://{BIGIP_HOST}/mgmt/tm"
auth = (BIGIP_USER, BIGIP_PASS)
partition = "Common"

node_name = f"{TAG}_node_app01"
pool_name = f"{TAG}_pool_app01_http"
vs_name = f"{TAG}_vs_app01_http"
http_profile_name = f"{TAG}_http_app01_profile"

def delete_if_exists(path: str) -> None:
    url = f"{base_url}{path}"
    resp = requests.delete(url, auth=auth, verify=False, timeout=30)

    # DELETE returns 200/204 on success, 404 if already missing.
    if resp.status_code in (200, 202, 204, 404):
        print(f"DELETE {path} -> {resp.status_code}")
        return

    print(f"DELETE {path} -> {resp.status_code}")
    try:
        print(resp.json())
    except Exception:
        print(resp.text)

# Delete order matters: virtual -> pool -> node -> profile
delete_if_exists(f"/ltm/virtual/~{partition}~{vs_name}")
delete_if_exists(f"/ltm/pool/~{partition}~{pool_name}")
delete_if_exists(f"/ltm/node/~{partition}~{node_name}")
delete_if_exists(f"/ltm/profile/http/~{partition}~{http_profile_name}")
```

## Run
```bash
python3 00_cleanup_tagged_objects.py
```

## Intended output (example)
- A sequence of `DELETE ... -> 204` (deleted) or `DELETE ... -> 404` (already absent)

## Validate cleanup (tmsh)
```bash
tmsh list ltm virtual /Common/netg_vs_app01_http
tmsh list ltm pool /Common/netg_pool_app01_http
tmsh list ltm node /Common/netg_node_app01
tmsh list ltm profile http /Common/netg_http_app01_profile
```
Expected: “not found” style output / no objects returned.

---

# Master Script — Create everything in one go (with optional cleanup first)

## File: `05_master_build_netg.py`
```python
import os
import argparse
import requests

requests.packages.urllib3.disable_warnings()

def must_env(name: str) -> str:
    v = os.getenv(name)
    if not v:
        raise SystemExit(f"Missing environment variable: {name}")
    return v

def request(method, url, auth, payload=None):
    return requests.request(
        method=method,
        url=url,
        auth=auth,
        json=payload,
        verify=False,
        timeout=30
    )

def delete_ok(status: int) -> bool:
    return status in (200, 202, 204, 404)

def create_ok(status: int) -> bool:
    return status in (200, 201)

def main():
    parser = argparse.ArgumentParser(description="Build LTM objects with a tag prefix (default: netg).")
    parser.add_argument("--tag", default=os.getenv("NETG_TAG", "netg"), help="Prefix tag for object names.")
    parser.add_argument("--clean-first", action="store_true", help="Delete tagged objects before creation.")
    args = parser.parse_args()

    host = must_env("BIGIP_HOST")
    user = must_env("BIGIP_USER")
    pwd = must_env("BIGIP_PASS")

    base_url = f"https://{host}/mgmt/tm"
    auth = (user, pwd)
    partition = "Common"
    tag = args.tag

    node_name = f"{tag}_node_app01"
    pool_name = f"{tag}_pool_app01_http"
    vs_name = f"{tag}_vs_app01_http"
    http_profile_name = f"{tag}_http_app01_profile"

    node_address = "10.1.20.11"
    member_port = 80
    destination_ip = "10.1.10.100"
    destination_port = 80

    def delete_path(path):
        resp = request("DELETE", f"{base_url}{path}", auth)
        print(f"DELETE {path} -> {resp.status_code}")
        if not delete_ok(resp.status_code):
            try:
                print(resp.json())
            except Exception:
                print(resp.text)
            raise SystemExit("Delete failed (unexpected status).")

    def post_path(path, payload):
        resp = request("POST", f"{base_url}{path}", auth, payload)
        print(f"POST {path} -> {resp.status_code}")
        if not create_ok(resp.status_code):
            try:
                print(resp.json())
            except Exception:
                print(resp.text)
            raise SystemExit("Create failed.")
        return resp

    if args.clean_first:
        # Order matters: virtual -> pool -> node -> profile
        delete_path(f"/ltm/virtual/~{partition}~{vs_name}")
        delete_path(f"/ltm/pool/~{partition}~{pool_name}")
        delete_path(f"/ltm/node/~{partition}~{node_name}")
        delete_path(f"/ltm/profile/http/~{partition}~{http_profile_name}")

    # 1) HTTP Profile
    post_path("/ltm/profile/http", {
        "name": http_profile_name,
        "partition": partition,
        "defaultsFrom": f"/{partition}/http",
        "insertXforwardedFor": "enabled"
    })

    # 2) Node
    post_path("/ltm/node", {
        "name": node_name,
        "partition": partition,
        "address": node_address
    })

    # 3) Pool (with member)
    member_full_name = f"/{partition}/{node_name}:{member_port}"
    post_path("/ltm/pool", {
        "name": pool_name,
        "partition": partition,
        "loadBalancingMode": "round-robin",
        "members": [{"name": member_full_name}]
    })

    # 4) Virtual Server
    destination_full = f"/{partition}/{destination_ip}:{destination_port}"
    post_path("/ltm/virtual", {
        "name": vs_name,
        "partition": partition,
        "destination": destination_full,
        "ipProtocol": "tcp",
        "sourceAddressTranslation": {"type": "automap"},
        "pool": f"/{partition}/{pool_name}",
        "profiles": [
            {"name": "tcp"},
            {"name": http_profile_name, "partition": partition}
        ]
    })

    print("\nAll objects created successfully.")
    print(f"Node:   /{partition}/{node_name}")
    print(f"Pool:   /{partition}/{pool_name}")
    print(f"VS:     /{partition}/{vs_name}")
    print(f"HTTP:   /{partition}/{http_profile_name}")

if __name__ == "__main__":
    main()
```

## Run (recommended flow)
1) Cleanup tagged objects (prevents collisions / avoids “already exists”)
```bash
python3 05_master_build_netg.py --clean-first --tag netg
```

2) Create without cleanup (if collision-free)
```bash
python3 05_master_build_netg.py --tag netg
```

## Intended output (example)
- A series of:
  - `DELETE ... -> 204` or `404` (only if `--clean-first`)
  - `POST ... -> 200/201` for all 4 objects
- Final summary lines:
  - `All objects created successfully.`
  - Object paths printed

## Validate on BIG-IP (tmsh)
```bash
tmsh list ltm node /Common/netg_node_app01
tmsh list ltm pool /Common/netg_pool_app01_http
tmsh list ltm virtual /Common/netg_vs_app01_http
tmsh list ltm profile http /Common/netg_http_app01_profile

tmsh show ltm pool /Common/netg_pool_app01_http
tmsh list ltm virtual /Common/netg_vs_app01_http profiles
```

---
