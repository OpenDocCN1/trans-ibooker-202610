
![](img/879ce8537ecb7481ad77093e6fcbe574_340_0.png)

> _Figure 13-1. High-Level Structure of Data Objects Most Commonly Relevant for Hiring Integrations and Automations_

## Candidate Object

The Candidate object is created any time a candidate creates an account on the external career site, an internal SuccessFactors user visits the internal career site, or a recruiter manually creates a candidate (or an interface you built creates one!). It contains basic information about the person such as first and last name, address, and also qualification data such as education, prior work experience, or any other background elements that have been configured. Most often, you will want to pull any personal information about a new hire from this object. This is because it is the easiest object for the new hire to update in the system. Application data includes candidate information; however, it is a snapshot of when the candidate applied. Many things can happen between the time a candidate applies and when they are ready to be hired! Last names can change, degrees can be earned, residences can change, etc., so it is best to get this information from where the candidate can easily update and make sure the recruiters know to inform the candidate to keep this information up to date so that downstream systems get accurate data.

## JobRequisition Object

The JobRequisition object is created when a user clicks the “Create Requisition” button within recruiting, a requisition is created from Employee Central position management, or an integration creates it from a third-party system. It contains information about a position such as the title, company code, and department. It also contains other information like the number of openings and the final time to fill once the requisition is closed. Additionally, information about the posting is associated with this object as well as associations with the recruiting team such as the recruiter and hiring manager. Typically, we see information about the position drawn from this object into HRIS systems such as the position ID that corresponds to the position ID in the HRIS system. Cost center information can also be pulled into payroll systems to understand where to charge payments to the employee. In cases where the pay is fixed based on the position, pay information can also be drawn from this object.

## JobApplication Object

Anytime a candidate applies to a job posting or a recruiter adds a candidate to a requisition, a JobApplication Object is created. We tend to think of a JobApplication as the union of a candidate and a requisition (this can also help you understand why the system always requires users to create a candidate profile prior to applying to a posting). As mentioned in the Candidate object section, the JobApplication object takes a snapshot of the candidate profile at the time of application and stores it on the JobApplication. So it is important to take this in consideration when transferring data upon hire (e.g., Candidate data on the JobApplication may be out of date by the time you are ready to hire). JobApplications also make associations with other objects like assessments, background checks, interviews, etc. Additionally, in our experience, we tend to add custom fields to this object (defined in the template XML) that include important information about the candidate that is relevant (e.g., if the candidate is a rehire, whether they passed a rehire check, legacy system IDs, etc.). Items that we don't want to capture on a candidate profile until someone has actually applied tend to reside within the JobApplication object as well. For example, racial information, tax ID, and birthdate are sensitive data we tend to avoid collecting until needed and therefore often get collected on the Job Application toward the end of the recruiting process.

### JobOffer Object

The JobOffer object is the object that stores data behind the offer approval screen. When you create an offer approval on an application, this object gets created. It is typically used to capture financial information for HRIS and payroll systems in the event that this information is not stored on the application or requisition. Since the JobOffer object is a child of the JobApplication object and the JobApplication is a child of the JobRequisition object, it provides the most specific information available (e.g., it is specific to a particular candidate rather than the entire requisition and is specific to the latest offer on the table). However, it might not be used in all scenarios (e.g., in the event of an hourly job that starts at a fixed hourly rate for all hired employees). In the most common scenario, a base salary and possibly any bonus information are sent to an HRIS/payroll system from this object. Sometimes a job start date, orientation date, etc., may also be included here.

> **Note** *In addition to not being required for all jobs, it is also important to keep in mind that multiple JobOffer objects may be associated with one JobApplication object (e.g., when a candidate rejects the first offer and a new offer is made). Thus, we recommend logic to be included in any interface that checks if the JobOffer object exists and, if one exists, then to pull information only from the latest one.*

### Case Study: Sending Hire Data to a Payroll System Using a Custom CSV Format

Now that we've walked through the different types of data that are collected throughout the recruiting process and how they are stored, we are ready to start transferring relevant data to other systems now that you are ready to hire the candidate. We started the book looking at how requisitions could be created from a payroll system. To continue this scenario and since hiring into a payroll system involves significant data and is the most practical scenario, it makes sense to explore it further. Typically, payroll systems will want to import data in a predefined CSV format that cannot be changed. Being able to create a CSV file with the needed data without coding can help avoid middleware and developer costs. Let’s see how it is done!

> Note If you have already read through prior chapters, you are probably already familiar with using the Integration Center to gather the needed objects and create a CSV file. Here, we got into a little more detail about how to gather data from multiple objects and also change the format to the CSV file output to fit more rigid formatting needs.

To begin our journey, let’s pretend the HRIS system administrator has given us a sample CSV file as shown in Figure 13-2. We must output data in the same format as shown in Figure 13-2 in order for it to load into payroll. The first thing to notice is that this is a character delimited file instead of comma – and the character used to delimit data is a pipe character "|". When we create the file, we will need to make sure to configure it using this character instead of a comma, in addition to making sure the header titles and order of all of the fields we gather from throughout the system are in the same format.

![](img/879ce8537ecb7481ad77093e6fcbe574_343_0.png)

## Figure 13-2. Sample Payroll Input File

> Note You would likely also have more fields on a payroll file such as type of pay (hourly/salary), work categorization, etc. We have trimmed this file down to show you how to get to different types of fields from different objects without being completely comprehensive.

6. Now, we will make sure to use a pipe delimited format. On the Options screen, give the integration a name. Be sure “Output File Type” is set to “Simple Delimited,” “File Delimiter” is set to “Pipe (|),” and “Header Type” is set to “Simple Header.” An example is shown in Figure 13-3.

![](img/879ce8537ecb7481ad77093e6fcbe574_345_0.png)

Figure 13-3. Example Options Screen When Creating Sample Payroll Input File

7. Next, we will want to add our main filter. Click “Filter and Sort” at the top. Click “Advanced Filters,” and then in the “Field” field, click “Browse.” In the new pop-up, click “Entity Tree View.” This will allow us to navigate to related entities. Scroll down to “jobAppStatus” and click this object to expand it. Click “Application Status Id.” An example is shown in Figure 13-4. Click “Change Association to Application Status Id” when you are ready to set the filter.
