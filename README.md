# C-005-SOC-Investigation-HTTP-Download-Checkin
SOC investigation of suspicious HTTP activity involving an executable-like file download followed by installation and heartbeat check-in traffic. Analyzes PCAP evidence using Wireshark and Kali Linux, with a focus on network forensics, HTTP analysis, and evidence-based incident classification.

## 1. Investigation Overview
This investigation analyzes HTTP traffic involving a workstation communicating with a remote server. The investigation focuses on an executable-like file download followed by installation-related and heartbeat communications from the workstation to the same remote server.

The investigation was conducted using Wireshark to examine the provided PCAP and Kali Linux to examine the downloaded object. The objective was to determine whether the observed network activity was potentially malicious and to establish what could and could not be confirmed from the available evidence.

## 2. Investigation Objective
The objective of this investigation is to determine whether the observed HTTP activity between the workstation and the remote server is consistent with malicious behavior.

The investigation specifically aims to:

- Identify the HTTP requests and responses exchanged between the systems.
- Examine the downloaded object and determine its characteristics.
- Analyze the subsequent installation-related and heartbeat communications.
- Establish whether the available PCAP evidence supports a malicious classification.
- Clearly distinguish confirmed evidence from activity that cannot be independently proven from the PCAP.

  ## 3. Analyst Role
Role: SOC Analyst / Security Analyst

The analyst is responsible for examining the network traffic, identifying potentially suspicious activity, analyzing available evidence, and determining an appropriate security classification based on the findings.

Primary Tool: Wireshark

Supporting Tool: Kali Linux

The investigation follows an evidence-first approach, ensuring that conclusions are based on observable network and file evidence rather than assumptions.

## 4. Evidence
The investigation was based on the following evidence:

- PCAP containing 17 packets of TCP/HTTP traffic.
- Source: `10.0.2.15`
- Destination: `198.51.100.50`
  
  <img width="1366" height="352" alt="TCP_HTTP_Traffic" src="https://github.com/user-attachments/assets/d486b7b7-1833-43cc-859c-55aecd9f179a" />
  
- HTTP request: `GET /download/update.exe`
- HTTP response: `200 OK`
- Content-Type: `application/octet-stream`
- Content-Length: `68 bytes`
  
  <img width="1366" height="267" alt="GET_request" src="https://github.com/user-attachments/assets/a0a4ab3f-7b9f-4780-8095-e714643647b7" />

- Downloaded object beginning with the `MZ` signature.

  <img width="1366" height="169" alt="downloaded_object_analysis" src="https://github.com/user-attachments/assets/e3210931-7233-46b9-b2f1-c547557fc4e5" />

- POST request to `/api/checkin` containing `status=installed`.
  
  <img width="1366" height="207" alt="POST_response" src="https://github.com/user-attachments/assets/d5a4a5b3-35c9-4e58-9b89-4d02ff913863" />

- Subsequent POST request containing `status=heartbeat`.
- Server responses: `200 OK` with `ACK:OK`.
- User-Agent: `curl/8.1`

  <img width="1366" height="225" alt="POST(2)_response" src="https://github.com/user-attachments/assets/2af08dbd-5d6b-470a-8f96-c64237e94660" />


## 5. Investigation Findings
- Workstation `10.0.2.15` communicated with server `198.51.100.50` over HTTP on port 80.
- The workstation requested `GET /download/update.exe`.
- The server returned `200 OK` with `Content-Type: application/octet-stream` and a content length of 68 bytes.
- The exported object began with the `MZ` signature, indicating an executable-like Windows file format.
- The workstation subsequently sent `POST /api/checkin` with `status=installed`.
- A second check-in reported `status=heartbeat`.
- Both check-ins received `200 OK` responses with `ACK:OK`.
- The HTTP User-Agent identified `curl/8.1` as the client.

  ## 6. Analysis
The traffic shows a sequence of activity involving an executable-like object:

GET /download/update.exe
        ↓
Executable-like object returned
        ↓
POST /api/checkin
status=installed
        ↓
POST /api/checkin
status=heartbeat

This sequence is consistent with the delivery of a potentially malicious file followed by installation-related and continued check-in activity.

The use of `curl/8.1` as the HTTP client also indicates that the communication was performed through cURL rather than a conventional web browser.

## 7. Classification

Classification: Malicious Activity

The activity is classified as malicious based on the sequence of an executable-like object being downloaded, followed by installation-related and heartbeat communications with the same remote server.

## 8. Conclusion & Recommendations

The investigation identified suspicious HTTP activity involving the download of an executable-like object from `198.51.100.50`, followed by installation-related and heartbeat communications from workstation `10.0.2.15`.

The activity is classified as malicious within the scope of this investigation. However, execution of the downloaded object could not be independently confirmed from the PCAP.

### Recommendations
- Isolate workstation `10.0.2.15` from the network.
- Restrict communication with the identified remote server.
- Preserve the PCAP and downloaded object for further analysis.
- Conduct endpoint investigation to determine whether the downloaded object was executed or established persistence.

The PCAP does not independently prove that the downloaded object was executed.
