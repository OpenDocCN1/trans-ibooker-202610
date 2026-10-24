| S4 – Description | Data sent by customers must be handled E2E within Europe. |
|---|---|
| Attribute | Interoperability. |
| Business value/architectural impact | High/Medium. |
| Stimulus | File transfer and handling. |
| Environment | At runtime. |
| Artifact | Customer data file. |
| Response | Requests are treated in Europe 100% of the time. |

Table 3.4 – Fourth scenario

| S5 – Description | Customers prefer to send files over FTP. |
|---|---|
| Attribute | Usability. |
| Business value/architectural impact | Medium/Medium. |
| Stimulus | File transfer. |
| Environment | At runtime. |
| Artifact | Customer data file. |
| Response | FTP Secure (FTPS)/Secure FTP (SFTP) channel(s) is/are exposed through port 21 or 22. |

Table 3.5 – Fifth scenario

| S6 – Description | During peak times, there could be up to 250 concurrent customers sending data files. |
|---|---|
| Attribute | Scalability. |
| Business value/architectural impact | High/High. |
| Stimulus | Increase in memory, CPU, and input/output (I/O). |
| Environment | In normal circumstances and during peak times. |
| Artifact | Upload channel. |
| Response | Provisioned channels will be able to ingest 5 megabytes (MB)/second; additional capacity will be made available to accommodate 50 MB/second during peak times. |

Table 3.6 – Sixth scenario