testmynids Test

The testmynids test is a widely used method to validate that an Intrusion Detection System is actively inspecting HTTP traffic and applying detection rules.

By sending a request to:

curl http://testmynids.org/uid/index.html

the server responds with a payload that intentionally contains patterns designed to trigger IDS signatures. When Suricata detects this response, it generates an alert such as:

GPL ATTACK_RESPONSE id check returned root

This confirms that:

    Suricata is capturing live network traffic

    HTTP inspection is enabled

    Default rule sets are properly loaded and active

    Alerts are being generated and logged correctly

Ping Test (ICMP Detection)

The ping test is used to validate custom rule functionality and confirm that Suricata detects ICMP traffic based on user-defined rules.

A custom ICMP alert rule is created to monitor ping activity targeting the internal network. When a ping is executed:

ping <target-ip>

Suricata inspects the ICMP packets and triggers an alert when the rule conditions are met.

This test confirms that:

    Custom rules are successfully loaded

    Rule syntax and logic are correct

    Suricata detects low-level network activity beyond application-layer traffic

JQ Monitoring (Real-Time Alert Inspection)

Suricata writes structured event data to eve.json, which is designed for real-time analysis and SIEM ingestion.

Using jq, alerts can be filtered and monitored live:

tail -f /var/log/suricata/eve.json | jq 'select(.event_type=="alert")'

This allows analysts to:

    Observe alerts in real time

    Focus only on security-relevant events

    Inspect detailed fields such as source IP, destination IP, signature ID, and severity

This approach closely resembles how alerts are monitored in SOC environments using SIEM platforms.
fast.log vs eve.json

Suricata provides multiple logging formats to support different operational needs.
fast.log

    Plain-text, human-readable format

    Designed for quick inspection and troubleshooting

    Suitable for manual review during testing or debugging

    Limited structure and not ideal for automation

Example use case:

    Quickly verifying whether an alert was triggered

eve.json

    Structured JSON format

    Designed for automation, correlation, and SIEM ingestion

    Contains rich metadata including timestamps, flow IDs, protocol details, and alert context

    Ideal for ELK, Splunk, or other log analysis platforms

Example use case:

    Feeding alerts into a SOC monitoring pipeline

    Advanced querying, correlation, and visualization
