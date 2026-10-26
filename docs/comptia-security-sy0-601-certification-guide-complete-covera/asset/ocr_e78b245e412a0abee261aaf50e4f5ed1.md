+   - **Time and Day Restriction**: Time and day restriction is set up for each user as a company may have many different shift patterns and may not wish their employees to access their network outside of their working hours. The diagram below shows that employees can access the network between 7 am and 6 pm. This prevents users from coming in at 3 a.m. when nobody else is on the company's premises and stealing data.

![](bbox=[0.175, 0.21, 0.825, 0.505])

Figure 3.22 – Time and day restrictions

**Example**: A toy factory may employ university students to work before the busy Christmas period with three different shift patterns; 6 a.m. - 2 p.m., 2 - 10 p.m., and 10 p.m. - 6 a.m. Each employee will have a time and day restriction in place so that they can log in only for their shift times.

+   - **Account Lockout**: To prevent dictionary and brute-force attacks, account lockout is enforced so that maybe three or five attempts to enter the password are allowed. Once exceeded, the account is disabled. This prevents hackers from guessing an account password.

Tip
If a time restriction is to be placed on a group of contractors, RBAC will be used. Time and day restrictions can only be used for individuals.