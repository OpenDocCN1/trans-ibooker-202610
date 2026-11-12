16. Which of the following utilities will allow you to change the time on a Linux system's hardware clock?
A. hwclock
B. date
C. timedatectl
D. ntpd
E. rtcsync

17. What command will allow you to see the system time in both its current time zone as well as in UTC?
A. hwclock -r
B. date
C. timedatectl
D. ntpd -p
E. chronyc sources

18. Which of the following is a correct configuration lines in the /etc/ntp.conf file for the ntpd service, assuming you will be using the pool.ntp.org time servers? (Choose all that apply.)
A. server pool.ntp.org iburst
B. pool pool.ntp.org iburst maxsources 2
C. service pool.ntp.org iburst
D. server 0.pool.ntp.org iburst
E. service 0.pool.ntp.org iburst

19. A system administrator, Geordi, has recently configured and started the ntpd service to obtain accurate time for his system's software clock. He'd like to update the hardware clock, but wants to quickly check if the software clock is now synchronized. What is the best command he should use?
A. ntpdate
B. ntpstat
C. ntpd -p
D. date
E. hwclock -w

20. Miles wants to configure his newly installed chrony. Which of the following might be a configuration file he needs to view/modify? (Choose all that apply.)
A. /etc/ntp.conf
B. /etc/ntp/chrony.conf
C. /etc/chrony.d/chrony.conf
D. /etc/chrony.conf
E. /etc/chrony/chrony.conf