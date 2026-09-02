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

  ## 4. Evidence Provided

The following evidence was available for analysis during the investigation:

- PCAP file containing the network traffic under investigation.
- Total packets captured: 17.
- Source workstation IP address: 10.0.2.15.
- Destination server IP address: 198.51.100.50.
- Network protocols observed: TCP and HTTP.
- Destination port: 80.
- HTTP request for `/download/update.exe`.
- HTTP response containing an object identified as `application/octet-stream`.
- Subsequent POST requests to `/api/checkin`.
- Installation-related check-in data: `status=installed`.
- Subsequent heartbeat data: `status=heartbeat`.
- HTTP User-Agent identifying cURL 8.1.
- Exported downloaded object examined using Kali Linux.

  ## 5. Investigation Scope
The investigation was limited to the network and file evidence available in the provided PCAP.

The analysis focused on:

- Identifying the communicating hosts and protocols.
- Examining the HTTP requests and responses.
- Investigating the request for `/download/update.exe`.
- Examining the HTTP response and downloaded object.
- Analyzing the `application/octet-stream` content type and content length.
- Examining the POST requests to `/api/checkin`.
- Analyzing the `status=installed` and `status=heartbeat` data.
- Identifying the HTTP client through the User-Agent.
- Determining whether the available evidence supports a malicious classification.
- Establishing the limitations of the PCAP, particularly regarding execution of the downloaded object.

  ## 6. Initial Investigation Questions
The following questions guided the initial investigation:

1. What systems are communicating in the captured traffic?

2. What protocols and destination ports are being used?

3. What HTTP requests were made by the workstation?

4. What object was requested from the remote server?

5. What was the server's response to the file request?

6. What characteristics does the downloaded object exhibit?

7. What information was sent to the `/api/checkin` endpoint?

8. What does the `status=installed` communication indicate?

9. What does the subsequent `status=heartbeat` communication indicate?

10. Does the observed sequence provide sufficient evidence to classify the activity as malicious?

11. Can execution of the downloaded object be independently confirmed from the available PCAP evidence?

12. What immediate containment actions would be appropriate based on the observed activity?

## 7. Investigation Approach
The investigation followed a structured network-forensics approach:

1. Reviewed the PCAP to establish the total packet count and identify the communicating systems.

2. Identified the source and destination IP addresses, protocols, and destination port.

3. Filtered the traffic for HTTP requests to identify the activities initiated by the workstation.

4. Examined the `GET /download/update.exe` request and its corresponding HTTP response.

5. Reviewed the HTTP response headers, including the status code, Content-Type, Content-Length, and User-Agent.

6. Exported the downloaded object from the HTTP traffic and examined it safely using Kali Linux without executing the file.

7. Examined the POST requests to `/api/checkin` and analyzed the information contained in their request bodies.

8. Established the sequence of events by correlating the file download with the subsequent `status=installed` and `status=heartbeat` communications.

9. Assessed the observed behavior and classified the activity based on the available evidence.

10. Documented evidence limitations, particularly the inability of the PCAP alone to independently confirm execution of the downloaded object.

## 8. Important Analyst Rule
The investigation follows an evidence-based approach.

An analyst must distinguish between:

- What the network traffic directly proves.
- What the available evidence strongly suggests.
- What cannot be confirmed from the available evidence.

In this investigation, the PCAP provides evidence of an executable-like object being downloaded, followed by installation-related and heartbeat communications. However, the PCAP does not independently prove that the downloaded object was executed.

Therefore, conclusions must not claim execution, persistence, or full system compromise unless additional endpoint or forensic evidence supports those claims.
