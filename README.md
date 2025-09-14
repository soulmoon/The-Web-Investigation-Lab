# 🕵️‍♂️ Web Investigation Lab

**Objective:**  
Examine network traffic with Wireshark to investigate web server compromise, identify SQL injection, extract attacker credentials, and detect uploaded malware.

---

## 📂 Category
- **Network Forensics**

## 🎯 Tactics
- Initial Access  
- Persistence  
- Command and Control  

## 🛠️ Tools
- Wireshark  
- Network Miner  

---

## 🖥️ Scenario

You are a cybersecurity analyst working in the Security Operations Center (SOC) of **BookWorld**, an expansive online bookstore renowned for its vast selection of literature. BookWorld prides itself on providing a seamless and secure shopping experience for book enthusiasts around the globe.  

Late one evening, an automated alert is triggered by an unusual spike in database queries and server resource usage, indicating potential malicious activity. This anomaly raises concerns about the integrity of BookWorld's customer data and internal systems, prompting an immediate and thorough investigation.  

As the lead analyst in this case, you are required to analyze the network traffic to uncover the nature of the suspicious activity. Your objectives include identifying the attack vector, assessing the scope of any potential data breach, and determining if the attacker gained further access to BookWorld's internal systems.

---

## ❓ Questions & Answers

**Q1**  
By knowing the attacker's IP, we can analyze all logs and actions related to that IP and determine the extent of the attack, the duration of the attack, and the techniques used. Can you provide the attacker's IP?  
**>> 111.224.250.131**
<img width="940" height="218" alt="image" src="https://github.com/user-attachments/assets/f0d8724f-af0d-4f93-8e9e-6687bde9d23c" />

---

**Q2**  
If the geographical origin of an IP address is known to be from a region that has no business or expected traffic with our network, this can be an indicator of a targeted attack. Can you determine the origin city of the attacker?  
**>> Shijiazhuang**
<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/b41b1d6e-be19-4e6e-b966-931808a34659" />

---

**Q3**  
Identifying the exploited script allows security teams to understand exactly which vulnerability was used in the attack. This knowledge is critical for finding the appropriate patch or workaround to close the security gap and prevent future exploitation. Can you provide the vulnerable PHP script name?  
**>> search.php**
<img width="940" height="370" alt="image" src="https://github.com/user-attachments/assets/146530ba-0b47-465b-9251-34b54a4a322e" />

---

**Q4**  
Establishing the timeline of an attack, starting from the initial exploitation attempt, what is the complete request URI of the first SQLi attempt by the attacker?  
Note: Decode the Value.  
**>> /search.php?search=book and 1=1; -- -**
<img width="940" height="311" alt="image" src="https://github.com/user-attachments/assets/0a62a3a4-9382-46fa-8f51-2de168f12689" />

---

**Q5**  
Can you provide the complete request URI that was used to read the web server's available databases?  
Note: Decode the Value.  
**>> /search.php?search=book' UNION ALL SELECT NULL,CONCAT(0x7178766271,JSON_ARRAYAGG(CONCAT_WS(0x7a76676a636b,schema_name)),0x7176706a71) FROM INFORMATION_SCHEMA.SCHEMATA-- -**

---

**Q6**  
Assessing the impact of the breach and data access is crucial, including the potential harm to the organization's reputation. What's the table name containing the website user’s data?  
**>> customers**

---

**Q7**  
The website directories hidden from the public could serve as an unauthorized access point or contain sensitive functionalities not intended for public access. Can you provide the name of the directory discovered by the attacker?  
**>> /admin/**
<img width="940" height="222" alt="image" src="https://github.com/user-attachments/assets/a16d5670-0a3a-431f-9af9-3fdd00967384" />

---

**Q8**  
Knowing which credentials were used allows us to determine the extent of account compromise. What are the credentials used by the attacker for logging in?  
**>> admin:admin123!**
<img width="940" height="586" alt="image" src="https://github.com/user-attachments/assets/590dc744-777a-4671-9514-1452beafd388" />

---

**Q9**  
We need to determine if the attacker gained further access or control of our web server. What's the name of the malicious script uploaded by the attacker?  
**>> nvri2vhp.php**
<img width="940" height="574" alt="image" src="https://github.com/user-attachments/assets/ab8831b6-61ce-4f53-860c-4be4f71ad2d6" />

---

## 📊 Wireshark Filters & Commands Used

| Purpose | Filter / Command |
|--------|-----------------|
| Identify PHP requests | `http contains ".php"` |
| Decode first SQLi attempt URI | `http contains ".php"` + URI decode |
| Isolate POST requests (admin login & upload) | `http.request.method == POST` |
| Identify attacker IP | **Wireshark:** Statistics → Conversations |
| Check geolocation of attacker IP | External IP lookup (GeoIP tools) |

---

## 🧠 Key Learning Outcomes

- ✅ Identified attacker IP and geolocation  
- ✅ Detected vulnerable script and SQL injection payloads  
- ✅ Reconstructed attacker actions and credential compromise  
- ✅ Discovered malicious file upload indicating deeper compromise  

---

## 🔗 References
- [Wireshark Documentation](https://www.wireshark.org/docs/)
- [CyberDefenders Labs](https://cyberdefenders.org/)

---

💡 **Pro Tip:** Always decode request URIs and inspect HTTP POST payloads carefully — they often reveal credentials, SQLi payloads, and file uploads.

