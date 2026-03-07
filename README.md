# 🛡️ IOC Sentinel

> **Bulk Threat Intelligence Scanner** — Check IPs, domains, URLs and file hashes against multiple threat intelligence sources in one click.

[![Live Demo](https://img.shields.io/badge/🌐_Live_Demo-Visit_App-00ffe0?style=for-the-badge)](https://naeemk10.github.io/IOC-sentinel/ioc-sentinel-v4.html)
[![GitHub Pages](https://img.shields.io/badge/Hosted_on-GitHub_Pages-181717?style=for-the-badge&logo=github)](https://naeemk10.github.io/IOC-sentinel/ioc-sentinel-v4.html)
[![Cloudflare Workers](https://img.shields.io/badge/API_Proxy-Cloudflare_Workers-f48120?style=for-the-badge&logo=cloudflare)](https://workers.cloudflare.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🔴 **Real-time Threat Scoring** | Live verdicts — MALICIOUS / SUSPICIOUS / CLEAN / UNKNOWN |
| 📂 **CSV & TXT Upload** | Bulk upload up to 500 IOCs at once |
| 📋 **Paste IOCs Directly** | No file needed — just paste IOCs into the text box |
| 🔬 **VirusTotal Integration** | Real detection counts via Cloudflare Worker proxy |
| 🛡️ **AbuseIPDB Integration** | Abuse confidence scores for IPs |
| 👾 **AlienVault OTX** | Pulse counts, tags, malware families |
| 🌍 **IP Geolocation** | Country, city, ISP via ip-api.com (no key needed) |
| 📊 **Threat Donut Chart** | Live breakdown of scan results |
| 🔍 **Search + Sort** | Filter and sort results by any column |
| ⏳ **Rate Limit Countdown** | Auto-handles VT free tier limits |
| 🔄 **Auto-Retry** | Retries failed API calls automatically |
| ⚠️ **Safety Warnings** | Alerts when malicious URLs are detected |
| ⏸️ **Pause / Resume** | Stop and continue scans mid-way |
| ⬇️ **CSV Export** | Export full results with all API data |
| 🎯 **Simulation Mode** | Works without API keys for demo/testing |

---

## 🚀 Live Demo

👉 **[https://naeemk10.github.io/IOC-sentinel/ioc-sentinel-v4.html](https://naeemk10.github.io/IOC-sentinel/ioc-sentinel-v4.html)**

No installation needed — just open in your browser!

---

## 🔑 API Keys Required

All APIs have **free tiers** — no payment needed.

| API | Purpose | Free Limit | Get Key |
|-----|---------|-----------|---------|
| **VirusTotal** | Malware detections | 4 req/min, 500/day | [virustotal.com/gui/my-apikey](https://www.virustotal.com/gui/my-apikey) |
| **AbuseIPDB** | IP abuse scores | 1000 req/day | [abuseipdb.com/account/api](https://www.abuseipdb.com/account/api) |
| **AlienVault OTX** | Threat pulses | Unlimited | [otx.alienvault.com/settings](https://otx.alienvault.com/settings) |
| **ip-api.com** | IP Geolocation | 45 req/min | ✅ No key needed |

> 💡 **Keys stay in your browser only** — never stored, never sent to any server except the respective APIs.

---

## 🏗️ Architecture

```
Browser (GitHub Pages)
    │
    ├──► Cloudflare Worker ──► VirusTotal API
    │    (ioc-sentinel.naeemkathawala07.workers.dev)
    │
    ├──► AbuseIPDB API (direct)
    │
    ├──► AlienVault OTX API (direct)
    │
    └──► ip-api.com (direct, no key)
```

### Why Cloudflare Worker?
VirusTotal blocks direct browser requests (CORS policy). The Cloudflare Worker acts as a proxy — it adds the `x-apikey` header server-side and forwards the response back to the browser. **Free forever, 100,000 requests/day.**

---

## 📋 Supported IOC Types

| Type | Example | Sources Checked |
|------|---------|----------------|
| **IPv4** | `185.220.101.34` | VT + AbuseIPDB + OTX + GEO |
| **IPv6** | `2001:db8::1` | VT + OTX |
| **Domain** | `malware-c2.xyz` | VT + OTX + GEO |
| **URL** | `https://phish.example.com` | VT + OTX |
| **MD5** | `44d88612fea8a8f36de82e1278abb02f` | VT + OTX |
| **SHA1** | `da39a3ee5e6b4b0d3255bfef95601890afd80709` | VT + OTX |
| **SHA256** | `e3b0c44298fc1c149afb...` | VT + OTX |

---

## 🎯 Verdict Logic

| Verdict | Condition |
|---------|-----------|
| 🔴 **MALICIOUS** | VT ≥ 3 detections OR AbuseIPDB ≥ 80 OR OTX ≥ 5 pulses |
| 🟡 **SUSPICIOUS** | VT > 0 OR AbuseIPDB ≥ 30 OR OTX ≥ 1 pulse |
| 🟢 **CLEAN** | All sources return clean |
| ⚪ **UNKNOWN** | Unsupported IOC type |

---

## 📁 CSV Format Support

```
# Simple list (one IOC per line)
185.220.101.34
malware-c2.xyz
https://phishing.example.com

# With header row (auto-detected)
ioc,type,notes
185.220.101.34,ip,suspicious
malware-c2.xyz,domain,c2

# Defanged IOCs (auto-normalized)
185[.]220[.]101[.]34
hxxps://phishing[.]example[.]com
```

---

## 📤 CSV Export Fields

```
IOC, Type, Verdict, Mode,
VT_Malicious, VT_Suspicious, VT_Total, VT_Reputation, VT_Link,
Abuse_Score, Abuse_Reports, Abuse_Country, Abuse_ISP, Abuse_TOR,
OTX_Pulse_Count, OTX_Pulse_Names, OTX_Tags, OTX_Malware_Families,
Geo_Country, Geo_City, Geo_ISP
```

---

## 🔒 Privacy & Security

- ✅ **No data stored** — all processing happens in your browser
- ✅ **No backend** — pure client-side app
- ✅ **API keys in memory only** — cleared on page refresh
- ✅ **No tracking, no ads, no analytics**
- ✅ **Open source** — inspect every line of code

---

## 🛠️ Self-Hosting

### Option 1 — Local (Zero Setup)
```bash
# Just download and open in browser
open ioc-sentinel-v4.html
```

### Option 2 — GitHub Pages
```bash
1. Fork this repository
2. Go to Settings → Pages
3. Source: Deploy from branch → main
4. Your app is live at: https://YOUR-USERNAME.github.io/IOC-sentinel/
```

### Option 3 — With Your Own Cloudflare Worker
```javascript
// workers.js — deploy at workers.cloudflare.com (free)
const VT_API_KEY = "YOUR_VT_KEY_HERE";

export default {
  async fetch(request) {
    const url = new URL(request.url);
    const vtPath = url.pathname;
    const allowed = ["/domains/","/ip_addresses/","/files/","/urls/"];
    if (!allowed.some(p => vtPath.startsWith(p)))
      return new Response("Forbidden", { status: 403 });
    const vtRes = await fetch(`https://www.virustotal.com/api/v3${vtPath}`, {
      headers: { "x-apikey": VT_API_KEY, "Accept": "application/json" }
    });
    const data = await vtRes.json();
    return new Response(JSON.stringify(data), {
      status: vtRes.status,
      headers: { "Content-Type": "application/json", "Access-Control-Allow-Origin": "*" }
    });
  }
};
```

---

## 👤 Author

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/Naeemk10">
        <img src="https://github.com/Naeemk10.png" width="100px;" alt="Naeem Kathawala"/><br/>
        <sub><b>Naeem Kathawala</b></sub>
      </a>
    </td>
  </tr>
</table>

[![GitHub](https://img.shields.io/badge/GitHub-Naeemk10-181717?style=flat&logo=github)](https://github.com/Naeemk10)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Naeem_Kathawala-0077B5?style=flat&logo=linkedin)](https://www.linkedin.com/in/naeem-kathawala/)

---

## 📜 License

MIT License — free to use, modify and distribute.

---

<div align="center">
  <sub>Built with ❤️ for the Security Community</sub>
</div>
