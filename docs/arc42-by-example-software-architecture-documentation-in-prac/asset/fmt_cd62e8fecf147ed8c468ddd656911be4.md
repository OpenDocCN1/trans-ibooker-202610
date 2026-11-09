
In their ongoing effort to optimize their business, **MoPho**'s management decide to streamline their tariff landscape. Within a large campaign, they contact every customer with outdated tariffs and offer upgrades to new and mostly more beneficial tariff options. Some customers get to pay less but have to accept a longer contract duration. Others will have to pay a little more, but receive additional services such as increased capacity, bandwidth, or other benefits. When customers accept the new tariff offer, the formal contract between **MoPho** and the customer has to be updated, which requires a valid signature to be legally binding.

### Note
In some countries, or for some contract types, there might be simpler solutions than a written signature. Please ignore this for the moment.

**MoPho** intends to inform certain customers via printed letters of the new tariff offerings. Customers can respond via letter, fax, phone, or in person at one of **MoPho**'s many sales outlets. As **MoPho**'s core competency involves phone services, they don't want to bother with printing letters, scanning and processing replies, answering phone inquiries, and dealing with other out of scope activities. Therefore, they employ MaMa-CRM as an all-around carefree solution. Look at the following MaMa approach to this scenario:

![Figure 3.2: Telecommunication example scenario ](image/B15546_03_02.jpg)

#### Figure 3.2: Telecommunication example scenario

The steps depicted in the preceding scenario need some explanation:
1.  **MoPho** selects appropriate customers from its internal IT systems. We're talking about a 30+ million customer base, and approximately 10 million of those customers will be part of this campaign. **MoPho** exports their address, contract, and tariff data and transmits these to MaMa-CRM. **MaMa** imports this customer data.
2.  **MaMa** forwards only the relevant parts of the customer data to the print service provider (a "**Partner**"). This company creates personalized letters from address and tariff data, prints those letters, and finally delivers them to the postal service (who ensures the letters finally end up in the customers' mailboxes).
3.  MaMa now informs the call center (also a "**Partner**") participating in this campaign so that they can prepare for the customer's reaction.
4.  In the meantime, the print service has delivered the letters to the postal service, which delivers letters to the respective customers. This is more difficult than it sounds: 1-5% of addresses tend to change within 12 months, depending on the customer base. Some customers refuse to accept marketing letters. The postal service informs MaMa about all these problem cases (address unknown, forwarding request, and so on) so that MaMa can decide on further action.
5.  The customers decide what to do with the letter: There's a multitude of options here:
    The customer fills out the enclosed reply form and signs it.
    The customer fills out the reply form but forgets to sign it.
    The customer does not understand this offer and sends an inquiry by letter.
    The customer inquires by phone call.
    The customer ignores the letter and does not respond at all.
    There are additional special cases (the customer is not contractually capable, has a custodian or legal guardian, is underage, deceased, or temporarily unavailable).
    Let's assume that the customer accepts and sends the letter back via the postal service. These letters will be forwarded to the scan service provider (again, a "**Partner**"), which scans them and performs optical character recognition. **MaMa** imports the scan results from the scan service provider. Again, several cases are possible here. **MaMa** distinguishes between all possible cases and takes the appropriate actions:
    The form is completely filled and signed.
    The form is completely filled, but the customer forgot to sign it.
    The customer signed it but forgot to fill in other important fields of the form.
    If the form is not returned within an appropriate period of time, **MaMa** might decide to resend the letter, maybe with a different layout, wording, or even a different contractual offer.
    Surely you can imagine several other possible options.
6.  Finally, for every customer who accepted the changed tariff agreement, **MaMa** sends back the appropriate information so that the mandator of **MoPho** can internally change the corresponding master data, namely contracts and tariffs, and perform all the other required actions (activities in **MaMa**).
    For **MoPho**, the primary advantage of this is the centralized interface for all customer interaction: all the processes that are required to conduct this crucial campaign are handled by MaMa-CRM, without intervention by **MoPho**. Something else that's practical for **MoPho** is the nearly unlimited flexibility of MaMa-CRM to import and export a variety of different data formats. This flexibility allows MaMa-CRM to easily incorporate new campaign partners (such as a second scan service provider, an additional email service provider, or a web hosting partner).

## 1.1.2 Campaign Configuration
**MaMa** provides a software foundation for campaign instances that have to be extensively configured to operate for a specific mandator with a number of specific partners.

The following section provides an overview of such a configuration for the **MoPho** campaign we described in the previous section:
1.  Configure the client master data:
    Define and configure the required data attributes and additional classes. Usually, this information is completely provided by the mandator.
    This data (for example, contract-, tariff-, offer-, and other business-specific entities or attributes) are modeled as extensions of the **MaMa** base model in UML.
    In the **MoPho** telecommunications example, this data includes the existing tariff, a list of potential new tariffs, the contract number, the validity period of the existing tariff, and the validity period of the new tariff.
2.  Configure the required communication channels:
    Define and exchange security credentials (passwords, certificates, and user IDs).
    Determine the contact address, especially for handling technical and process issues.
3.  Define the campaign metadata:
    This includes start and end dates, target dates for specific activities, the reply address for letters, the reply email address, and the phone number for the call center.
4.  Define campaign activities:
    Campaign activities can be either input, output, or **MaMa** internal activities.
    Output activities *export* data to partners or the mandator, for example, **PrintLetter**, **DataToCallCenter**, **0044ataToScanServiceProvider**, **ResultsToMandator**, and **ProductionOrderToCardProvider**.
    Input activities *import* data provided by the mandator or partners, for example, **MasterDataFromMandatorImport**, **ScanDataImport**, **CallCenterImport**, and **SalesOutletImport**.
    Internal activities define data maintenance operations, for example, to delete customer data 60 days after completion and delete all campaign data 120 days after campaign termination.
5.  Model campaign flow:
    The required flow of activities, that is, what needs to happen under which conditions (including plausibility checks).

## 1.1.3 Activities Subject to Charge
Some activities result in charges, such as the following:
*   Sending letters via a postal service results in postal charges.
*   Producing e-Health cards results in card production charges.

Since we are talking about potentially millions of client activities, these charges will amount to a significant flow of money:
*MaMa-CRM does NOT handle billing and payment processes.*

Instead, these are handled in parallel with MaMa campaigns by InDAC's finance department.

### Note
