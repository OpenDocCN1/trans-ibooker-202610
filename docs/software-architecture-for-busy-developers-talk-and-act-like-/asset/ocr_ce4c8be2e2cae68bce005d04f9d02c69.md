Let's go through some quality-attribute scenarios for our use case. We will infer them from our utility tree created in the previous section, but for the sake of brevity, we will limit ourselves to six scenarios, as follows:

| S1 – Description | Operations on the platform must happen during planned downtime. The impact over the upload channel(s) should be minimal and short. |
| Attribute | Availability. |
| Business value/architectural impact | High/Medium. |
| Stimulus | Maintenance tasks. |
| Environment | Underlying upload channel infrastructure. |
| Artifact | Upload channels. |
| Response | Operations occur within an agreed timeframe and maintenance takes less than 2 hours. |

Table 3.1 – First scenario

| S2 – Description | Upload channel(s) outage |
| Attribute | Availability |
| Business value/architectural impact | High/High |
| Stimulus | Outage |
| Environment | Channel hosting platform |
| Artifact | Upload channel |
| Response | Automatic restore of upload channel within < 10 minutes |

Table 3.2 – Second scenario

| S3 – Description | Data service(s) outage |
| Attribute | Availability |
| Business value/architectural impact | Medium/Low |
| Stimulus | Outage |
| Environment | Data platform |
| Artifact | Data services |
| Response | Degraded user experience (UX) until service(s) is/are back up and running, with an RTO of a maximum of 6 hours |

Table 3.3 – Third scenario