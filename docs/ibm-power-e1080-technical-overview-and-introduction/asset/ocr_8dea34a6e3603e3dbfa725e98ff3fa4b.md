# 2.10.6 Entitled System Support

The Entitled Systems Support website (ESS) is IBM's go-to place to view and manage Power and Storage software and hardware. In general, most products that are offered by IBM Systems that are purchased through our IBM Digital Sales representatives or Business Partners are accessed on this site when the IBM Configurator is used.

The site features the following three main sections:

+   - My entitled software: Activities related to Power and Storage software that offer to download licensed, free, and trial software media, place software update orders, and manage software keys.
+   - My entitled hardware: Activities related to Power and Storage hardware that offer to renew Update Access Keys, buy and use Elastic Capacity on Demand, assign or buy credits for new and existing pools in Enterprise Pools 2.0, download Storage Capacity on Demand codes, and manage Hybrid Capacity credits.
+   - My inventory: Activities related to Power and Storage inventory that offer to browse software license, software maintenance, and hardware inventory, manage inventory retrievals by way of Base Composer or generate several types of reports.

For more information, see this web page.

# 2.10.7 Update Access Keys

Since the introduction of the POWER8 processor-based server servers, IBM also introduced Update Access Key (UAK).

When system firmware updates are applied to the system, UAK and its expiration date are checked (see Figure 2-37 on page 122). System firmware updates include a release date.

When attempting to apply system firmware updates, if the release date for the firmware updates passed the expiration date for the update access key, the updates are not processed. As update access keys expire, they need to be replaced by using the Hardware Management Console (HMC) or the ASMI on the service processor.

By default, newly delivered systems include an UAK that often expires after three years. Thereafter, the UAK can be extended every six months, but only if a maintenance contract exists. The contract can be verified in the ESS website (for more information, see 2.10.6, “Entitled System Support” on page 119).

Determining when the current Update Access Key runs through the HMC, GUI or CLI as provided in the following examples. However, it also is possible to display the expiration date by using the suitable AIX or IBM i command.

# UAK expiration date by using AIX 7.1

In the case of AIX 7.1, use the following command:

```
lscfg -vp1 sysplanar0 | grep -p "System Firmware"
```