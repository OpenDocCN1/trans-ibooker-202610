
At this point, you should have a solid understanding of how to create custom scripts for detecting vulnerable or malicious package versions. You can extend the script you prepared in this section to automatically download the latest `bad_versions.json` file, generate a detailed security report, and provide actionable remediation recommendations—such as locking dependencies to known safe versions, and updating affected packages.

# Using OSV-Scanner to Detect Vulnerable Packages

In the preceding section, you created a custom script to detect malicious or vulnerable package versions. In practice, combining custom scripts with prebuilt automated scanning tools provides the most comprehensive coverage for detecting vulnerable dependencies. One of the popular options is `OSV-Scanner`, which checks your application dependencies against the Open Source Vulnerabilities (`OSV`) database and generates detailed vulnerability reports.

In this section, you’ll install `OSV-Scanner`, scan your serverless project for vulnerable dependencies, and review the generated report so you can take actionable steps to secure your project. You can think of `OSV-Scanner` as a metal detector at airport security, checking your project’s dependencies and flagging anything that looks suspicious. Here, the generated report works like the security officer’s inspection notes, showing exactly which items need further attention and what actions to take next.

### Note

This section assumes you are using your Serverless Lab account, which hosts the vulnerable-by-design serverless lab resources.

Let’s go over how to use `OSV-Scanner` to detect vulnerable packages:

1. In the Cloud Shell terminal of your Serverless Lab account (after the `$` sign), run the following command to install the vulnerable version of the `debug` package in your project:

```
cd ~/vulnerable-function/

npm install debug@4.0.0
```

This yields the following output:

```
added 1 package, changed 1 package, and audited 92 packages in 2s

16 packages are looking for funding
  run `npm fund` for details

1 low severity vulnerability

To address all issues, run:
  npm audit fix

Run `npm audit` for details.
```

### Note

Here, `npm` reports one low severity vulnerability in `debug@4.0.0`, indicating that you should further investigate the issue in detail as the next step.

2. Run `npm audit` to generate an audit report showing the advisory ID, vulnerable versions, and steps to resolve the issue across affected packages:

```
npm audit --verbose
```

This generates the following audit report:^([9](#ch13.html#id998))

```
...
# npm audit report

debug  4.0.0 - 4.3.0
Regular Expression Denial of Service in debug -
  https://github.com/advisories/GHSA-gxpj-cx7g-858c

fix available via `npm audit fix`
node_modules/debug

1 low severity vulnerability

...
```

3. Run the following command to check for outdated npm packages:

```
npm outdated --json
```

This returns the following JSON output:

```
{
  "debug": {
    "current": "4.0.0",
    "wanted": "4.4.3",
    "latest": "4.4.3",
    "dependent": "vulnerable-function",
    "location": ".../node_modules/debug"
  }
}
```

4. Check which versions of `debug` are installed and where they appear in the dependency tree:

```
npm list debug
```

You should get a detailed dependency tree showing all occurrences and versions of `debug` in the project:

```
vulnerable-function@1.0.0 /home/user/vulnerable-function
├─┬ @azure/identity@4.13.0
│ └─┬ @azure/core-rest-pipeline@1.22.1
│   └─┬ @typespec/ts-http-runtime@0.3.1
│     ├─┬ http-proxy-agent@7.0.2
│     │ └── debug@4.4.3
│     └─┬ https-proxy-agent@7.0.6
│       └── debug@4.0.0 deduped
├─┬ azure-functions-core-tools@4.3.0
│ ├─┬ extract-zip@2.0.1
│ │ └── debug@4.3.7
│ └─┬ https-proxy-agent@5.0.1
│   ├─┬ agent-base@6.0.2
│   │ └── debug@4.3.7 deduped
│   └── debug@4.3.7 deduped
└── debug@4.0.0
```

Here, you can see that multiple versions of `debug` are installed in different parts of the dependency tree, including the vulnerable `4.0.0` version used directly by your project and some nested dependencies.

5. Download the latest `OSV-Scanner` binary into your home directory:

```
cd ~

URL_1="https://github.com/google/osv-scanner"
URL_2="releases/latest/download/osv-scanner_linux_amd64"

curl -LO "$URL_1/$URL_2"
```

6. Rename the downloaded binary and make it executable:

```
mv osv-scanner_linux_amd64 osv-scanner
chmod +x osv-scanner
```

7. Verify that the `OSV-Scanner` binary is working by checking its version:

```
./osv-scanner --version
```

This returns the following:

```
osv-scanner version: 2.2.2
osv-scalibr version: 0.3.1
commit: 16ed4529c0d0cc1ddf1bed2fb08deba187339673
built at: 2025-08-27T02:52:51Z
```

### Note

The version number you see may differ based on when you installed `OSV-Scanner`, given that more recent versions may have been published since this book’s release.

8. Scan your project in the `vulnerable-function` directory recursively for vulnerabilities with `OSV-Scanner`:

```
./osv-scanner --recursive vulnerable-function/
```

This yields the following logs:

```
Scanning dir vulnerable-function/
Starting filesystem walk for root: /
Scanned /home/user/vulnerable-function/package-lock.json file
and found 90 packages
End status: 5 dirs visited, 16 inodes visited, 1 Extract calls,
2.457984ms elapsed, 2.458154ms wall time
Total 1 package affected by 1 known vulnerability
(0 Critical, 0 High, 0 Medium, 1 Low, 0 Unknown) from 1 ecosystem.
1 vulnerability can be fixed.


╭─────────────────────────────────────┬──────┬───────────┬─────── ≈
│ OSV URL                             │ CVSS │ ECOSYSTEM │ PACKAG ≈
├─────────────────────────────────────┼──────┼───────────┼─────── ≈
│ https://osv.dev/GHSA-gxpj-cx7g-858c │ 3.7  │ npm       │ debug  ≈
╰─────────────────────────────────────┴──────┴───────────┴─────── ≈
```

9. Run `OSV-Scanner` again and output the results in JSON format:

```
./osv-scanner --recursive vulnerable-function/ --format json
```

You should get a JSON response similar to the following:

```
  ...

  "packages": [
    {
      ...
      "vulnerabilities": [
        {
          ...
          "summary": "Regular Expression Denial of Service in debug",
          "details": "Affected versions of `debug` are vulnerable to
          regular expression denial of service when untrusted user
          input is passed into the `o` formatter...",
          "severity": [
            {
              "type": "CVSS_V3",
              "score": "CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:N/I:N/A:L"
            }
          ],
          ...
        }
      ],
      ...
    }
  ]

  ...
```

10. Generate an HTML report for the `vulnerable-function` project and save it to a file (`report.html`):

```
./osv-scanner --recursive vulnerable-function/ --format=html > report.html
```

11. Download the generated HTML report to your local machine by clicking the “Upload/Download files” button on the Cloud Shell toolbar and choosing Download from the list of options. In the “Download a file” dialog box, specify `report.html` and then click Download. Finally, click “Click here to download your file” to start the download.
