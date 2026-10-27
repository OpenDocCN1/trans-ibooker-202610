deployed on any Anthos clusters, including those running on-prem. These solutions are fully managed by Anthos, and benefit from all Anthos capabilities, including auto-scaling which guarantees that on-prem compute resources are only allocated while in use, and only what is needed. Anthos meters these AI solutions and bills customers based only on usage. Anthos Hybrid AI allows AI applications to be deployed, run and be un-deployed dynamically, on-demand.

# 15.5.2 Hybrid AI Solutions

The first release of Anthos Hybrid AI makes available the following Google Cloud AI services on-prem:

+   - *Optical Character Recognition (OCR)*: which converts documents into a text in digital format; and
+   - *Speech-to-Text*: which converts voice into text with support including English, French, Spanish, Cantonese and Japanese. The [Google Cloud version](https://) already supports more than one hundred different languages and variations allowing for easy inclusion of additional language to the on-prem version, driven by demand.

The following sections discuss each of these use cases in greater detail.

# Optical Character Recognition

Enterprises have a tremendous amount of data being held hostage in non-digitized media, such as paper documents, or hidden inside digital formats that do not allow them to be easily analyzed digitally. For example, a scanned digital image of an invoice or of a government issued title may function as a record of a transaction and a proof of ownership, respectively, but may still remain very challenging to extract the individual pieces of information that can be analyzed separately. The names of the entities involved in the deal, date, location, value, and specifics about the items exchanged may remain buried into the digital image. Previous efforts to digitize this information manually with humans entering the data have proven themselves to be error-prone, time consuming, cost prohibitive and hard to scale.