# Router Security Assessment

## Target Information
- Device: Home Router
- IP Range Scanned: 192.168.1.0/24

## Scan Techniques Used
- ARP Scan
- Nmap Basic Scan
- Vulnerability Scan
- HTTP Header Enumeration

## Weak Default Credentials
- Service: Router Web Admin Interface
- Address: http://192.168.1.1
- Result: Successfully logged in using default credentials
  - Username: admin
  - Password: admin
- Risk Level: High
- Impact: Full access to router configuration by any network user
- Recommendation: Change the default admin password to a strong, unique one

### Vulnerability Findings

1️⃣ Weak Default Credentials
- The router allowed access attempts using default manufacturer credentials.
- This indicates poor password policy enforcement.

2️⃣ Open HTTP Management Interface
- Port 80 (HTTP) is open and serves the router login page without HTTPS.
- No certificate or TLS protection detected → susceptible to MITM attacks.

3️⃣ Missing Security Headers
From HTTP response:
- No X-Frame-Options
- No Strict-Transport-Security
These increase risks of Clickjacking & Session Hijacking.

4️⃣ Local Network Exposure
- Management panel accessible from the LAN without IP restrictions.
- Any device on the network can attempt brute force or exploitation.

Risk Level: Medium-High

### Recommendations

- Change default username and password immediately.
- Disable remote access and restrict LAN access where possible.
- Enable HTTPS if supported by the router.
- Add firewall rules to block unnecessary internal + external ports.
- Keep router firmware up to date.

🔹 Data Sanitization
To ensure no sensitive or personally identifiable information is exposed, all MAC addresses and internal IP segments were anonymized using sed rules.
Example of applied sanitization:
sed -i \ -e 's/[0-9A-Fa-f]\{2\}(:[0-9A-Fa-f]\{2\})\{5\}/XX:XX:XX:XX:XX:XX/g' \ -e 's/MAC Address: .*/MAC Address: XX:XX:XX:XX:XX:XX/g' \ scans/*.txt 
This step ensures that the report can be safely published without leaking network device identifiers.
