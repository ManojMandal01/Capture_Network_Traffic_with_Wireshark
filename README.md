# 🧪 Wireshark Network Traffic Analysis

**Analysis Performed By:** Manoj Mandal  
**Date:** 2025-06-26  
**Capture File:** [`wireshark_capture.pcapng`](./wireshark_capture.pcapng)  
**Tool Used:** [Wireshark](https://www.wireshark.org/)  

---

## 🎯 Objective

To analyze captured packets and identify network behavior, protocol usage, and possible issues during a short-lived session involving OCSP checks, IPv4/IPv6 communication, and HTTP requests.

---

## 🗂️ Key Observations

### 🌐 HTTP Traffic
| Frame No. | Source                  | Destination             | URI                         | Status        |
|-----------|-------------------------|--------------------------|------------------------------|----------------|
| 398       | 192.168.1.12            | 34.107.221.82            | `/success.txt?ipv4`         | `200 OK`       |
| 401       | 2401:4900:...           | 2600:1901:...            | `/success.txt?ipv6`         | `302 Redirect` |
| 464       | 2401:4900:...           | 2600:1406:...            | `/`                          | `200 OK`       |
| 472       | 2401:4900:...           | 2600:1406:...            | `/favicon.ico`              | `404 Not Found`|

**Analysis:**  
- The client attempts connections over both IPv4 and IPv6.
- One of the GET requests (`favicon.ico`) results in a 404, indicating a missing resource.

---

### 🔐 OCSP Traffic
OCSP (Online Certificate Status Protocol) requests are used to verify the revocation status of X.509 certificates.

| Time (s)  | Source IP                  | Destination IP                     | Type     | Size     |
|-----------|----------------------------|------------------------------------|----------|----------|
| 8.99      | 2401:4900:...              | 2404:6800:...                      | Request  | 513 bytes|
| 9.06      | 2404:6800:...              | 2401:4900:...                      | Response | 996 bytes|
| 12.82     | 2401:4900:...              | 2600:140f:...                      | Request  | 517 bytes|
| 12.83     | 2600:140f:...              | 2401:4900:...                      | Response | 976 bytes|
| 22.12–22.16| 2401:4900:...             | 2606:4700:...                      | Multiple Requests/Responses (5 total, 517–1376 bytes)|

**Analysis:**  
- Multiple OCSP lookups from different IPv6 addresses indicate active certificate verification for HTTPS connections.
- The consistent use of IPv6 for OCSP traffic reflects dual-stack network support.

---

## 📶 Protocol Summary

| Protocol | Count | Description                                     |
|----------|-------|-------------------------------------------------|
| HTTP     | 6     | IPv4 and IPv6 HTTP requests and responses       |
| OCSP     | 10+   | Certificate revocation checks (IPv6 mostly)     |

---

## 🛡️ Security Insight

- ✅ **Certificate Validation Present:** OCSP requests confirm secure communications attempt certificate validation.
- ⚠️ **Missing Favicon:** `/favicon.ico` returns `404`, potentially leaking web server configuration details.
- ✅ **IPv6 Enabled:** Most communications use IPv6, indicating modern network stack usage.

---

## ✅ Conclusion

This packet capture reflects typical web browsing behavior with secure HTTPS handshakes (OCSP), HTTP requests over both IPv4 and IPv6, and responsive endpoints. The absence of malicious traffic suggests a clean capture.

📁 **File Analyzed:** `wireshark_capture.pcapng`  
🔗 [GitHub Repository](https://github.com/ManojMandal01/Capture_Network_Traffic_with_Wireshark)

> 🚀 Further investigation can be done by applying filters like `http`, `ocsp`, or `ip6` in Wireshark.

---
