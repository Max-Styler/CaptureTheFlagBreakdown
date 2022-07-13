# Blue Team: Summary of Operations

## Table of Contents

- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

_TODO: Fill out the information below._

The following machines were identified on the network:

- Name of VM 1
  - **Operating System**:Linux
  - **Purpose**: Allows users to access the backend of the RavenSecurity website
  - **IP Address**:192.168.1.110
- Name of VM 2
  - **Operating System**:
  - **Purpose**:
  - **IP Address**:

### Description of Targets

_TODO: Answer the questions below._

The target of this attack was: `Target 1` IP:192.168.1.110

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

- Excessive HTTP Errors

```KQL
WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes
```

- HTTP Request Size Monitor

```KQL
WHEN sum() OF http.request.pytes OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute
```

- CPU Usage Monitor

```KQL
WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes.
```

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

Excessive HTTP Errors is implemented as follows:

- **Metric**: When count of top 5 http.response.status_code(s)
- **Threshold**: are above 400 for the last 5 minutes
- **Vulnerability Mitigated**: brute force attacks
- **Reliability**: medium reliablity

#### HTTP Request Size Monitor

HTTP Request Size Monitor is implemented as follows:

- **Metric**: When sum of http.request.bytes
- **Threshold**: is above 3500 for the last minute
- **Vulnerability Mitigated**: DDOS attacsk (denial of service)
- **Reliability**: TODO: Does this alert generate lots of false positives/false negatives? Rate as low, medium, or high reliability.

#### Name of Alert 3

Alert 3 is implemented as follows:

- **Metric**: TODO
- **Threshold**: TODO
- **Vulnerability Mitigated**: TODO
- **Reliability**: TODO: Does this alert generate lots of false positives/false negatives? Rate as low, medium, or high reliability.

_TODO Note: Explain at least 3 alerts. Add more if time allows._

### Suggestions for Going Further (Optional)

_TODO_:

- Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain _how_ to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

- Vulnerability 1
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- Vulnerability 2
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- Vulnerability 3
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
