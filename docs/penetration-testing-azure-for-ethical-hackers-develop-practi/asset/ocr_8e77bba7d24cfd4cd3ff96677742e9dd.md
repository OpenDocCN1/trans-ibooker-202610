4. Authenticate to Azure using the az login Azure CLI command:

```
root@PentestVM:~# az login
```

5. When prompted, enter the Contributor credentials that you obtained from the script output in the first exercise in this chapter:

![](bbox=[0.205, 0.235, 0.735, 0.525])

Figure 5.15 – Az login password prompt

You are now authenticated with the Contributor credentials.

6. Download Lava using the following command:

```
root@PentestVM:~# git clone https://github.com/mattrotlevi/lava.git
```

Here is a screenshot of the command and its output:

![](bbox=[0.175, 0.695, 0.79, 0.815])

Figure 5.16 – Cloning the Lava GitHub repository