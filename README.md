# 🐍 Wireshark Network Traffic Analysis

**Analysis Performed By:** Manoj Mandal  
**Date:** 2025-06-26  
**Capture File:** [`wireshark_capture.pcapng`](./wireshark_capture.pcapng)  
**Tool Used:** [Wireshark](https://www.wireshark.org/)  

---

## 🎯 Objective

To analyze captured network traffic for patterns, protocol usage, and potential anomalies using **Wireshark**. This includes HTTP requests, OCSP checks, and IPv6 traffic to understand the behavior of the host during the session.

---

## 🗂️ Key Observations

### 🔹 HTTP Traffic
- **GET Requests**:
  - `/success.txt?ipv4` → `34.107.221.82` (Returned HTTP 200 OK)
  - `/success.txt?ipv6` → `2600:1901:0:38d7::` (Returned HTTP 302 Redirect)
  - `/favicon.ico` → Returned HTTP 404 (Not Found)
- **Status Codes Observed**:
  - `200 OK`, `302 Redirect`, `304 Not Modified`, `404 Not Found`

---

### 🔹 OCSP (Online Certificate Status Protocol)
- Multiple OCSP requests made to:
  - `23.58.120.18`
  - `23.10.239.251`
  - `2606:4700:8392:c460:...`
- OCSP responses were successful (mostly 938–1374 bytes)

---

### 🔹 IPv6 Traffic
- Active communication between:
  - `2401:4900:8829:...` ↔ `2600:1901:...`
  - `2401:4900:8829:...` ↔ `2606:4700:8392:...`

---

## ⚠️ Notable Packets (Selected)

| Frame No. | Time (s)   | Source             | Destination        | Protocol | Info                         |
|-----------|------------|--------------------|--------------------|----------|------------------------------|
| 314       | 11.0859    | 192.168.1.12       | 34.107.221.82      | HTTP     | GET /success.txt?ipv4        |
| 396       | 11.1273    | 34.107.221.82      | 192.168.1.12       | HTTP     | 200 OK (text/plain)          |
| 649       | 20.2639    | 2401:4900:...       | 2600:1901:...       | HTTP     | GET /success.txt?ipv6        |
| 881       | 43.8172    | 2401:4900:...       | 2600:1406:...       | HTTP     | GET /                        |
| 885       | 44.0615    | 2600:1406:...       | 2401:4900:...       | HTTP     | 304 Not Modified             |
| 891       | 44.8299    | 2600:1406:...       | 2401:4900:...       | HTTP     | 404 Not Found (favicon.ico)  |

---

## 🔐 Security Insight

- **OCSP Checks** indicate SSL/TLS certificate validation — common in secure HTTPS connections.
- **Presence of IPv6 Traffic** confirms dual-stack networking (IPv4 and IPv6).
- **404 Errors** on `/favicon.ico` could indicate incomplete web server configurations.

---

## ✅ Conclusion

This network capture reveals:
- Active web communication via IPv4 and IPv6.
- Routine OCSP checks ensuring certificate validity.
- Successful and failed HTTP responses that help profile network behavior.
--
