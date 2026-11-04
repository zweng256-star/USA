import base64
import json
import urllib.request
from urllib.parse import unquote

KEYWORDS = (
    "美国", "美西", "美东", "洛杉矶", "西雅图", "圣何塞",
    "纽约", "芝加哥", "达拉斯", "usa", "united states",
    "🇺🇸", "us-"
)

def decode_base64(text):
    text = text.strip()
    text += "=" * (-len(text) % 4)
    return base64.b64decode(text).decode("utf-8", errors="replace")

def get_node_name(line):
    # VMess 节点名通常放在 JSON 的 ps 字段
    if line.lower().startswith("vmess://"):
        payload = line.split("://", 1)[1].split("#", 1)[0]
        try:
            return str(json.loads(decode_base64(payload)).get("ps", ""))
        except Exception:
            pass

    # 其他常见节点通常把名称放在 # 后面
    if "#" in line:
        return unquote(line.rsplit("#", 1)[1])
    return ""

url = input("订阅链接: ").strip()

request = urllib.request.Request(
    url,
    headers={"User-Agent": "Mozilla/5.0"}
)
with urllib.request.urlopen(request, timeout=20) as response:
    content = response.read().decode("utf-8", errors="replace").strip()

# 订阅内容可能是 Base64 编码
if "://"
KEYWORDS = (
    "加拿大", "canada", "ca-",
    "多伦多", "温哥华", "蒙特利尔", "卡尔加里", "渥太华",
    "toronto", "vancouver", "montreal", "calgary", "ottawa",
    "🇨🇦"
)
import base64
import urllib.request

def decode_subscription(text):
    text = text.strip()
    padded = text + "=" * (-len(text) % 4)
    try:
        decoded = base64.b64decode(padded).decode("utf-8", errors="replace")
        if "://" in decoded:
            return decoded
    except Exception:
        pass
    return text

url = input("订阅链接: ").strip()

request = urllib.request.Request(
    url,
    headers={"User-Agent": "Mozilla/5.0"}
)

with urllib.request.urlopen(request, timeout=20) as response:
    content = response.read().decode("utf-8", errors="replace")

content = decode_subscription(content)
nodes = [line.strip() for line in content.splitlines() if "://" in line]

with open("all_nodes.txt", "w", encoding="utf-8") as f:
    f.write("\n".join(nodes))

print(f"已导出 {len(nodes)} 个节点到 all_nodes.txt")