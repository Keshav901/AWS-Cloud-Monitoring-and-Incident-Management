# AWS Cloud Monitoring and Incident Management

**Amazon CloudWatch | Amazon SNS | EC2 Monitoring | Alerts | Troubleshooting**

## Overview

This project focuses on observing an **Amazon EC2** environment with **Amazon CloudWatch**, configuring CPU-utilization alarms, distributing notifications through **Amazon SNS**, and practicing structured incident troubleshooting. The scenarios described in the resume include high CPU utilization and connectivity failures, followed by log review and recovery documentation.

> **Portfolio note:** This README is a professional description of the project listed on my resume, not an independently verified record of test execution. Add the actual alarm settings, configuration files, screenshots, incident notes and observed results from your environment before publishing it as evidence of a completed lab.

## Monitoring architecture

```text
                     Amazon EC2 instance
                   /                     \
           CPUUtilization             OS/application logs
                  |                          |
           CloudWatch Metrics        CloudWatch Logs
                  |
           CloudWatch Alarm
                  |
             Amazon SNS
                  |
       Confirmed email subscription
                  |
            Alert recipient

    CloudWatch dashboard: metrics and operational visibility
```

**Note:** EC2 CPU utilization is available as an EC2 metric. Collecting operating-system and application logs in CloudWatch Logs requires configuring an agent or another supported log-delivery mechanism. SNS email notifications require subscription confirmation. Add only components that you actually configured.

## Technologies

- Amazon EC2
- Amazon CloudWatch Metrics, Dashboards, and Alarms
- Amazon SNS (Simple Notification Service)
- Linux system tools and logs
- CloudWatch Logs, **if collected/configured in your lab**

## Objectives

1. Track CPU utilization and relevant EC2 health metrics.
2. Display selected metrics on a CloudWatch dashboard.
3. Create CPU threshold alarms and route notifications through SNS.
4. Simulate a controlled high-CPU incident.
5. Investigate a controlled connectivity issue.
6. Document the timeline, findings, actions and verification of recovery.

## Example repository structure

```text
.
├── README.md
├── architecture/
│   └── monitoring-architecture.png
├── screenshots/
│   ├── cloudwatch-dashboard.png
│   ├── cpu-alarm.png
│   └── sns-notification.png
├── configs/                  # Add actual sanitized configs here
└── incidents/
    ├── high-cpu-incident.md
    └── connectivity-incident.md
```

Only include files you have created; the structure is a suggested layout.

## Setup guide

### 1. Prepare EC2

- Launch or select an EC2 instance in your intended AWS Region.
- Ensure you can access it through SSH or AWS Systems Manager where appropriate.
- Review security groups and instance reachability.
- Set budget alerts; terminate or stop nonessential resources after testing.

### 2. Configure a CloudWatch dashboard

- Open **CloudWatch → Dashboards**.
- Add EC2 metrics such as `CPUUtilization` and appropriate instance status metrics.
- Label widgets clearly by instance or environment.
- Record a screenshot with sensitive identifiers redacted.

### 3. Configure an SNS topic

- Create a topic (example name: `ec2-monitoring-alerts`).
- Add an email subscription and **confirm** it using the message sent to your inbox.
- Review access policies so notifications can be published appropriately.

### 4. Create a CloudWatch CPU alarm

Example lab settings (change to match your actual test):

| Setting | Example value |
|---|---|
| Metric | `AWS/EC2` → `CPUUtilization` |
| Statistic | Average |
| Period | 5 minutes |
| Threshold | Greater than 70% |
| Evaluation | 2 consecutive periods |
| Alarm action | Publish notification to an SNS topic |

These are **illustrative settings**, not verified values from your deployment. Depending on EC2 monitoring granularity and alarm evaluation settings, the timing and results will vary.

### 5. Simulate a controlled CPU incident

- First record a baseline CPU reading.
- Generate a **bounded** CPU workload on an EC2 instance that you own and are authorized to test.
- Observe CloudWatch metrics and confirm whether the alarm enters the `ALARM` state.
- Verify the SNS email notification arrives.
- Stop the workload and confirm the metric and alarm return to their normal state.

Run tests only in a disposable lab, not on shared or production infrastructure.

### 6. Simulate a connectivity incident

- Make a temporary, documented test change in your own lab (for example, restrict a test-specific inbound security group rule).
- Check connectivity, routing, instance status and security group configuration.
- Restore the known-good rule and confirm connectivity returns.

**Important:** A CPU alarm alone does not detect every SSH/network connectivity failure. Connectivity problems require separate status checks, reachability tests, logs or other signals. Do not claim that the CPU alarm diagnosed the network issue.

## Incident response workflow

```text
Observe abnormal behavior
        ↓
Identify affected resource and symptoms
        ↓
Review metrics, alarms, logs and networking
        ↓
Form and test a likely cause
        ↓
Apply a safe corrective action
        ↓
Verify service recovery
        ↓
Document timeline and prevention steps
```

## Incident record template

Use a separate Markdown file per actual test:

```markdown
# Incident: <High CPU / Connectivity>

- Date and time: <UTC timestamp>
- AWS Region: <region>
- Affected resource: <sanitized instance identifier>
- Detection method: <dashboard / alarm / connectivity test>
- Observed symptom: <what happened>
- Supporting evidence: <metric graph / relevant logs>
- Root cause or likely cause: <finding>
- Action taken: <change and reason>
- Recovery verified by: <metric / connection / application test>
- Detection time: <observed value>
- Resolution time: <observed value>
- Preventive action: <specific change>
```

Do not insert invented incident times or screenshots.

## Verification checklist

- [ ] Dashboard shows the expected EC2 instance and metric.
- [ ] CPU alarm has the intended threshold and evaluation period.
- [ ] SNS subscription is confirmed.
- [ ] A controlled CPU test caused an expected metric increase.
- [ ] Alarm state and notification were observed and captured.
- [ ] CPU recovered after stopping the test workload.
- [ ] A connectivity failure was reproduced, investigated and reversed safely.
- [ ] Incident records show actual observations and recovery verification.

## Common troubleshooting checks

| Issue | Investigation |
|---|---|
| CPU alarm stays `INSUFFICIENT_DATA` | Metric availability, region, dimensions, monitoring period |
| No email from SNS | Subscription confirmation, topic policies, alarm actions and alarm history |
| No log events | CloudWatch agent configuration, permissions, log group, region |
| SSH connection fails | Security groups, NACLs, routing, instance health, SSH service |
| CPU metric does not reflect test | Workload duration, metric granularity, aggregation and threshold |

## Cost and cleanup

CloudWatch custom dashboards, alarms, logs, SNS messages and EC2 usage can incur charges. Review regional pricing, configure billing alerts, and remove practice resources when done. Avoid leaving load-generating tests running.

## Skills demonstrated

AWS CloudWatch · EC2 monitoring · CloudWatch Metrics · Alarm configuration · Amazon SNS · Linux troubleshooting · Log analysis · Incident documentation · AWS networking

## Future improvements

- Add disk and memory monitoring through the CloudWatch agent.
- Add system logs and relevant application logs to CloudWatch Logs.
- Automate alarm and dashboard creation using Terraform.
- Add more incident scenarios with recorded evidence and verified results.

---

**Author:** Keshav Sekhri · [GitHub](https://github.com/Keshav901)
