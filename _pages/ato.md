---
title: Lifecycle of a Launch
---

Every federal information system must go through NIST's [Risk Management Framework](background/) before it can be used to process federal information. This process culminates in a signed Authority to Operate (ATO) being issued. Because the ATO process is a complex, multi-step process which will constrain the design and implementation of your system, you should start thinking about how it applies to your system _before_ you begin designing and implementing it.

### Key roles

- **Authorizing Official (AO)** The AO is ultimately responsible for determining if the risk of operating the system is acceptable, and if so, issuing an Authority to Operate (ATO) for that system. At 18F the Director of Infrastructure is the AO.
- **System Owner** The system owner is usually the product lead or tech lead of the project team. They will be named in the ATO documents and are the main contact during the evaluation process that leads up to an ATO.

### Definitions

- **Information system** means a discrete set of information resources organized for the collection, processing, maintenance, use, sharing, dissemination, or disposition of information ([44 U.S.C. § 3502](https://www.law.cornell.edu/uscode/text/44/3502#8)).

### Timeline

There are a few factors that will determine how long it takes a project to get an ATO. These map to [the checklist](checklist/), so might be helpful to open that up in another window and follow along.

* Everything in [Phase 1](checklist/#phase-1-ato-sprint-prerequisites) needs to be done before the project can enter the ATO Sprint. That responsibility is on the project team and the respective Infrastructure Lead. Completing Phase 1 could take 40 hours of work.
* Your Infrastructure Lead conducts the documentation review of [Phase 2](checklist/#phase-2-documentation-review). Projects are scheduled for [Phase 3](checklist/#phase-3-ato-sprint) by the Infrastructure Leads as a group.
* [Phase 3](checklist/#phase-3-ato-sprint) should take two weeks, assuming the previous Phases were done thoroughly.

**The ATO Sprinting Team makes no _guarantees_ regarding the timeline of ATOs.**

### Team

ATO Sprints are staffed by cross-divisionally by the GSA Office of the Chief Information Security Officer (OCISO) and 18F. Beyond the project team roles (ex: system owner, product owner, etc) ATO Sprint roles typically include:

* **Authorizing Official (AO)**: Responsible for overall impact categorization and risk acceptance.
* **Infrastructure Lead**: Manages infrastructure decisions and ATO queue within a business unit. In consult with the other leads, reaches consensus on what projects go into each sprint.
* **Information System Security Officer (ISSO)**: Supports the information security system, consults on control selection, organizes scanning process. Reports to the Information System Security Manager (ISSM).
* **Penetration tester(s)**: Conducts the penetration test after terms are agreed to as documented in the Rules of Engagement (RoE) signed by 18F.
* **Assessor**: Validates and verifies that the documented controls (see [Step 3](#step-3--document-the-controls)) actually work, using the assessment cases (see [Step 4](#step-4--assess-the-controls)).

### Steps

#### Step 0 — Create ATO checklist

As soon as you begin developing an [alpha](https://18f.gsa.gov/dashboard/stages/#alpha), [create your ATO checklist](checklist/) to set up a tracking mechanism for your ATO. You can ask questions in your checklist thread to understand the specific considerations for your system. At this time it also good to ensure your system is eligible for [pre-assessment](types/#conditions-for-pre-assessment) authorization for user testing purposes.

#### Step 1 — Determine impact level

Work with your Infrastructure Lead to categorize your system's impact levels, using the [ATO Levels](levels/) guide. If your system will be providing novel or risky functions, or handling extremely sensitive data, do this as early as possible.

#### Step 2 — Select controls

"Controls" are individual security requirements laid out by the National Institute of Standards and Technology (NIST).
Your system's impact level baseline will determine the controls you need to implement.

#### Step 3 — Document the controls

This step is essentially "state how your system meets each of the regulations". Using established web frameworks (Rails, Django, etc.) and hosting in cloud.gov takes care of a lot of the lower-level controls and security best practices for you, so you only need to be concerned with your application's custom code and configuration. This custom code and configuration is known as the "attack surface".

Work together with your Infrastructure Lead on this step. The documentation generated by similar 18F projects can be helpful at this stage. The final version of this document is called the [System Security Plan](https://before-you-ship.18f.gov/ato/ssp/). Consult the checklist for examples.

##### Compliance Masonry

The [Compliance Masonry](https://github.com/opencontrol/compliance-masonry) YAML file uses structured data to state how each control is one of the following:

* Not relevant to the system
* Relevant to the system, and is one of the following:
    * Taken care of by one of:
        * The platform (cloud.gov)
        * The framework (Django, etc)
        * The authorization layer
        * Etc.
    * Implemented by you
    * Not yet taken care of

An "inherited" control would be something like FedRAMP requiring that fire extinguishers be present near the servers, which is something that AWS needs to worry about for _their_ compliance, and we don't need to re-explain when launching an application hosted there. Your Masonry file essentially contains the "overrides".

The controls that are _not_ inherited from an underlying system must be listed in your Masonry file with a short explanation ("narrative"), and "implemented" before the system can receive an ATO. "Implementation" in the compliance sense is the same as in the code sense: ensure that the system meets that requirement, based on current industry best practices.

#### Step 4 — Assess the controls

In other words, "verify that your system is secure". The first step in doing so is to run the [security scans](../security/scanning/). This is a preliminary assessment, final assessment will be done in collaboration with GSA OCISO. You are encouraged to run scans yourself, so that there aren't big surprises during the ATO Sprint.

In parallel, you will collaborate with a GSA OCISO assessor to verify all the controls in the SSP. The exact tests are given by this [assessment case template](https://docs.google.com/spreadsheets/d/1xYv9PHk6P1ePHSQcJWXSAYbAvmxzSsDOZrdS285DKYY/edit?ts=58eba80c#gid=1541480683).

#### Step 5 — Complete documentation package

Fill out the documentation in the checklist. We hope to auto-create the [SSP](ssp/) from the Compliance Masonry YAML file at some point, but for now you should copy and paste.

The full list of data and functions in and of the system (in government parlance "mission based information types" and "delivery mechanisms") must be itemized in structured data. While the data types are obviously arbitrary and custom to each system we produce, the government has a formalized data set of mission functions that should be mapped to the system via [NIST 800-60](http://csrc.nist.gov/groups/SMA/fisma/categorization.html). For a Rails app, for example, this can simply be a link to the `db/schema.rb` file on GitHub.

#### Step 6 — Authorize the system

Your Infrastructure Lead will work with you to schedule and prioritize your system assessment. Once assessment starts, the first step is that the AO will review all the items in your ATO checklist including all the documents you generated.

Then, for most systems, a team with members from the project team, your AO, your Infrastructure Lead, and the GSA OCISO will convene for at least a week as the [ATO Sprinting Team](https://docs.google.com/document/d/1bGOV_pp_BlAzZsoa2D5pnsinx3R2gVnaBdFvHkwv0ig/edit), and begin to follow the [Lightweight Security Authorization Process](https://insite.gsa.gov/portal/content/627230).

Folks from OCISO will conduct a penetration test on the system. Any penetration test findings deemed serious enough to prevent an ATO will need to be fixed right away to unblock the ATO process. They will also review the SSP document and test the control narratives. This testing and review process will take 1-2 weeks and should be the top priority for the project team at the time.

Once all of the materials are prepared and testing is done and the system is considered "ready" by the Authorizing Official, they will sign the ATO memo.

#### Step 7 — Continuously monitor the controls

There are several ways to ensure that your system remains compliant:

* Act on any security notifications from your [static analysis](../security/static-analysis/).
* Perform and act on findings from [dynamic scanning](../security/dynamic-scanning/).
* Keep your System Security Plan (and any other architecture/security-related documentation) up-to-date.

### Re-authorization

Your system may need to be reassessed and re-authorized if your application team is planning to make substantive changes, such as changes to:

* Encryption methodologies
* Administrative functionality within the application
* The kinds of information you store (for example, [personally identifiable information (PII)](../privacy/))
* The external services used or how/what data flows to/from them
* Anything that will requires an update to the System Security Plan, system diagram, etc.

Example changes that do _not_ require re-authorization, as long as they don't include the above:

* Features
* Bug fixes
* Interface changes
* Documentation updates

The Authorizing Official determines whether a system needs re-authorization. If you're planning a change that you think may require re-authorization, please [open an issue in the Infrastructure repository](https://github.com/18F/Infrastructure/issues/new?title=ATO+re-authorization+for+%3Cproject%3E?) to explain your planned change, so they can evaluate it.

If it needs re-authorization, follow the usual steps for getting an ATO, starting with [the checklist](checklist/). You should be able to reuse most of your existing ATO materials, assuming they have been kept up-to-date.

### ATO renewal

If your current ATO is going to expire, you'll need the ATO to be renewed. Follow the usual steps for getting an ATO, starting with [the checklist](checklist/). You should be able to reuse most of your existing ATO materials, assuming they have been kept up-to-date.