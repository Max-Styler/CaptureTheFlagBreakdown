# Blue Team: Summary of Operations

## Table of Contents

- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:

- Target 1
  - **Operating System**:Linux
  - **Purpose**: Allows users to access the backend of the RavenSecurity website
  - **IP Address**:192.168.1.110

### Description of Targets

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
- **Reliability**: Medium reliability

#### HTTP Request Size Monitor

HTTP Request Size Monitor is implemented as follows:

- **Metric**: When sum of http.request.bytes
- **Threshold**: is above 3500 for the last minute
- **Vulnerability Mitigated**: DDOS attacsk (denial of service)
- **Reliability**: Medium reliability

#### Name of Alert 3

Alert 3 is implemented as follows:

- **Metric**: When max of system.process.cpu.total.pct over all documents
- **Threshold**: is above 0.5 for the last 5 minutes
- **Vulnerability Mitigated**: Overloading the CP to bypass security protections and read protected system memory
- **Reliability**: High reliability
