# Footprinting and Reconnaissance Techniques

Hands-on cybersecurity lab documenting reconnaissance techniques using search engines, web services, social networks, website analysis, and DNS tools.

**Course:** Ethical Hacking & Network Defense  
**Status:** Complete

## Overview

In this lab, I explored how publicly available information can be used to learn about an organization and its online presence. My goal was to understand what these techniques reveal and how that information can help defenders recognize potential security risks.

This repository documents my work with screenshots, short explanations, and lessons learned. Activities are limited to the targets and tasks authorized for this course lab.

## Learning Goals

- Use Google advanced search operators to find relevant information.
- Gather information through web services, including OpenCorporates.
- Explore publicly available information on social networking sites.
- Review website source code and archived webpages using archive.org.
- Gather and interpret DNS information using `nslookup` and `Dnsenum`.

## Lab Exercises

| Exercise | Topic | Status |
| --- | --- | --- |
| 1 | [Footprinting Using Search Engines](#exercise-1--footprinting-using-search-engines) | Complete |
| 2 | [Footprinting Using Web Services](#exercise-2--footprinting-using-web-services) | Complete |
| 3 | [Footprinting through Social Networking Sites](#exercise-3--footprinting-through-social-networking-sites) | Complete |
| 4 | [Website Footprinting](#exercise-4--website-footprinting) | Complete |
| 5 | [DNS Footprinting](#exercise-5--dns-footprinting) | Complete |

Each exercise includes screenshots and a brief explanation of the tasks, observations, and lessons learned. Images linked from the course guide are labeled as lab references.

## Exercise 1 – Footprinting Using Search Engines

**Focus:** Using Google advanced search operators to narrow searches and locate publicly available information.

Search engines can reveal office locations, contact details, email addresses, and employee names. This exercise explores how that public information can support reconnaissance and why it can create social engineering risks.

### Lab Environment

| Device | Operating System | Role |
| --- | --- | --- |
| ACIDC01 | Windows Server 2022 | Domain controller |
| ACIWIN11 | Windows 11 Pro | Domain member workstation |

### Task 1 – Footprint Using Google Advanced Search Operators

#### Lab Setup

I connected to the ACIWIN11 virtual workstation to begin the exercise. The screenshot shows the Windows 11 desktop, ready for the browser-based search tasks.

![ACIWIN11 virtual workstation desktop before beginning search engine reconnaissance](screenshots/exercise-01/01-windows11-lab-setup.png)

#### Open the Browser

I opened Microsoft Edge on the lab workstation to begin the Google searches.

<details>
<summary>Screenshot: Microsoft Edge ready for the search tasks</summary>

![Microsoft Edge new tab on the lab workstation](screenshots/exercise-01/02-edge-browser.png)

</details>

#### Restrict Results to a Domain with `site:`

```text
"google search operators" site:google.com
```

I searched for the phrase "google search operators" and used `site:google.com` to limit results to Google's domain. The visible results included developers.google.com and support.google.com, showing that the filter also includes subdomains. I learned how to focus a search on a specific organization’s public web presence.

![Google results restricted to google.com and its subdomains](screenshots/exercise-01/03-site-search-results.png)

#### Search for Words in URLs with `allinurl:`

```text
allinurl:google search operators
```

I used `allinurl:` to search for the words google, search, and operators in webpage URLs. The results shown included Ahrefs, Google for Developers, and Search Engine Land. This introduced me to filtering by URL terms; the operator does not require an exact phrase or a specific word order.

<details>
<summary>Screenshot: entering the allinurl query before submitting it</summary>

![The allinurl query entered while the previous site search results remain visible](screenshots/exercise-01/04-allinurl-query-entry.png)

</details>

![Results after submitting the allinurl search](screenshots/exercise-01/05-allinurl-search-results.png)

**Observation:** The results page shortens several URLs, so this screenshot alone does not confirm every term in each full URL.

#### Search for a URL Term with `inurl:`

```text
inurl:google search operators
```

I used `inurl:google` to search for URLs containing google, with search and operators as additional search terms. Unlike `allinurl:`, only the word immediately after `inurl:` is restricted to the URL. I scrolled through the results and found pages about Google search operators from several websites.

<details>
<summary>Screenshot: entering the inurl query before submitting it</summary>

![The inurl query entered while the previous allinurl results remain visible](screenshots/exercise-01/06-inurl-query-entry.png)

</details>

![Scrolled results for the inurl search](screenshots/exercise-01/07-inurl-search-results.png)

#### Find PDF Documents with `filetype:`

```text
Cybersecurity filetype:pdf
```

I filtered my cybersecurity search to PDF documents. The visible results included the NIST Cybersecurity Framework 2.0 and guidance from other government agencies. I learned how to locate public reports by file format; this query does not require the word cybersecurity to appear in the title.

![Cybersecurity search results with PDF labels, including NIST and other government sources](screenshots/exercise-01/08-pdf-search-results.png)

#### Look Up a Term with `define:`

```text
define:cybersecurity
```

I searched for a definition of cybersecurity using `define:`. The results included CISA's "What is Cybersecurity?" page and a computer security information panel. This helped me connect the term to protecting networks, devices, and data.

<details>
<summary>Screenshot: entering the definition query before submitting it</summary>

![The define query entered while the address bar still shows the previous PDF search](screenshots/exercise-01/09-define-query-entry.png)

</details>

![Definition search results showing CISA and a computer security information panel](screenshots/exercise-01/10-define-search-results.png)

**Skills practiced:** Targeted searching, domain and URL filtering, finding PDF documents, researching terminology, and interpreting search results.

**Defense connection:** These techniques help identify publicly searchable information about an organization that could be used to make social engineering attempts more convincing.

**References:** [Google's site operator documentation](https://developers.google.com/search/docs/monitor-debug/search-operators/all-search-site) and [Google's legacy Search Appliance operator reference](https://www.google.com/support/enterprise/static/gsa/docs/admin/current/gsa_doc_set/xml_reference/request_format.html) (inurl and allinurl syntax), plus [Google's file type documentation](https://developers.google.com/search/docs/crawling-indexing/indexable-file-types).

**Exercise 1 complete.** I practiced narrowing searches by domain, URL terms, and file type, and used a definition query to research terminology.

## Exercise 2 – Footprinting Using Web Services

**Status:** Complete  
**Focus:** Using Google as a web service to identify publicly indexed subdomains.

### Task 1 – Use Web Services for Footprinting

I continued in Microsoft Edge on ACIWIN11, using the same lab environment as Exercise 1: the Windows 11 Pro workstation and ACIDC01 Windows Server 2022 domain controller.

```text
site:google.com -inurl:www
```

I combined `site:google.com` with `-inurl:www` to search Google's domain while excluding URLs containing the term www. The visible results included assistant.google.com, cloud.google.com, and play.google.com. I learned how combining search operators can reveal different parts of an organization's public web presence.

<details>
<summary>Setup screenshots: browser starting point and query entry</summary>

The browser initially showed the definition search from Exercise 1.

![Previous cybersecurity definition results before starting Exercise 2](screenshots/exercise-02/01-browser-starting-point.png)

I entered the new query before submitting it; the previous definition results were still visible underneath.

![Subdomain search query entered before submitting it](screenshots/exercise-02/02-subdomain-query-entry.png)

</details>

![Submitted search showing assistant.google.com, cloud.google.com, and play.google.com](screenshots/exercise-02/03-subdomain-search-results.png)

**What I observed:**

| Subdomain visible in the results | Service shown |
| --- | --- |
| `assistant.google.com` | Google Assistant |
| `cloud.google.com` | Google Cloud |
| `play.google.com` | Google Play |

**Limitation:** This search identifies some publicly indexed subdomains, not every subdomain. The exclusion applies to the term www in URLs, rather than only to a www hostname.

**Skills practiced:** Combining search operators, excluding URL terms, identifying subdomains, and documenting public search results.

**Defense connection:** Reviewing publicly visible subdomains can help defenders identify online services to include in an organization's asset inventory.

**Reference:** [Google's site operator documentation](https://developers.google.com/search/docs/monitor-debug/search-operators/all-search-site) explains that site searches are not exhaustive.

## Exercise 3 – Footprinting through Social Networking Sites

**Status:** Complete  
**Focus:** Researching public company records using OpenCorporates. The heading follows the course lab; this task uses a corporate information database.

### Lab Environment

| Device | Operating System | Role |
| --- | --- | --- |
| ACIDC01 | Windows Server 2022 | Domain controller |
| ACIKALI | Kali Purple 2023.1 | Stand-alone Linux workstation used for this task |

### Task 1 – Footprint Using OpenCorporates

I used Firefox on ACIKALI to search OpenCorporates for microsoft. I narrowed the results to Washington (US) and selected MICROSOFT CORPORATION. This helped me practice finding a specific company record and interpreting publicly available business information.

#### Lab Reference Screenshots

These six screenshots come from the Infosec Learning lab guide and are embedded from its original image links.

**1. Open Firefox on ACIKALI.**

![Lab reference: opening Firefox from the Kali Purple desktop](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s1_1.png)

**2. Enter `opencorporates.com` in the address bar.**

![Lab reference: entering the OpenCorporates website address](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s2_2.png)

**3. Search for `microsoft` using the Companies option.**

![Lab reference: Microsoft company search on OpenCorporates](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s3_2.png)

**4. Filter the results by Washington (US).** This narrows the list by registration jurisdiction.

![Lab reference: Washington jurisdiction filter in the search results](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s4_1.png)

**5. Select MICROSOFT CORPORATION.** Checking the jurisdiction and company name helps distinguish it from similarly named records.

![Lab reference: Microsoft Corporation in the Washington search results](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s5_1.png)

**6. Review the company record.**

![Lab reference: Microsoft Corporation record with registration details and address](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s6_1.png)

**Information visible in the lab reference:**

| Field | Value shown |
| --- | --- |
| Company name | MICROSOFT CORPORATION |
| Company number | 600413485 |
| Status | Active |
| Company type | WA PROFIT CORPORATION |
| Jurisdiction | Washington (US) |
| Registered address | 1 Microsoft Way, Redmond, WA 98052-8300, United States |

These values describe the supplied screenshot, not a current verification of the company record. The incorporation date and officer details require login in the reference image and were not visible.

**What I learned:** Filtering by jurisdiction makes a broad company search more precise. Public records can reveal registration details and addresses, but a similar company name alone does not establish a relationship.

**Skills practiced:** Public-source research (OSINT), company searches, jurisdiction filtering, and interpreting corporate records.

**Defense connection:** Public business information can help verify an organization's identity, but it can also make impersonation attempts sound convincing. Knowing a company's address or registration details is not proof that a message is legitimate.


## Exercise 4 – Website Footprinting

**Status:** Complete  
**Focus:** Reviewing webpage source and archived webpages to understand a website's public information and history.

### Lab Environment

| Device | Operating System | Role |
| --- | --- | --- |
| ACIDC01 | Windows Server 2022 | Domain controller |
| ACIWIN11 | Windows 11 Pro | Domain member workstation |
| ACIKALI | Kali Purple 2023.1 | Stand-alone Linux workstation used for this task |

### Task 1 – Footprint Using Source Code

**Status:** Complete

#### Open the Lab Website

I used Firefox on ACIKALI to open the lab website. It displayed the Infosec Learning login page.

```text
https://lab.infoseclearning.com
```

<details>
<summary>Screenshot: entering the lab website address</summary>

![Lab website address entered in Firefox before navigating away from the previous page](screenshots/exercise-04/01-enter-lab-url.png)

</details>

#### View Page Source

I right-clicked the login page and selected **View Page Source**. This let me inspect the HTML delivered to the browser and identify references to the site's resources.

![Firefox context menu with View Page Source selected](screenshots/exercise-04/02-view-page-source.png)

![HTML source showing generator metadata, stylesheets, and theme paths](screenshots/exercise-04/03-html-source.png)

#### Identify JavaScript References

I scrolled to line 95 in the captured source and found a `<script src="...">` reference to a JavaScript file. The source also showed resource paths and generator metadata. I learned how visible HTML can provide clues about a website's technologies and file organization.

![Highlighted script source reference near line 95](screenshots/exercise-04/04-javascript-references.png)

**What I observed:**

| Visible source detail | What it indicates |
| --- | --- |
| `<script src="...">` references to `.js` files | The page loads JavaScript resources. |
| `/sites/default/files/js/` and `/sites/default/files/css/` | Public URL paths used for JavaScript and stylesheets. |
| `/themes/isl_theme/` | A theme resource path referenced by the page. |
| Generator metadata naming `Drupal 10` and `Commerce 2` | The page advertises these technologies; this is a source-level clue, not independent version verification. |

**Skills practiced:** Inspecting HTML source, recognizing script and stylesheet references, identifying technology clues, and documenting visible resource paths.

**Defense connection:** Reviewing public source helps defenders understand what technical information a website exposes. These observations do not establish a vulnerability, reveal server-side source code, or provide a complete server directory listing.

### Task 2 – Footprint Using Archive.org

**Status:** Complete

I opened the Wayback Machine in Microsoft Edge on ACIWIN11 and searched for `practice-labs.com`. I followed the lab's steps to review an archived version and compare it with the live website. This helped me understand how older website content can remain publicly available after a site changes.

![Wayback Machine with practice-labs.com entered in the search field](screenshots/exercise-04/05-wayback-domain-search.png)

**Lab comparison steps:** Select 2019 in the capture timeline, choose February 27, and open the 19:16:12 snapshot specified in the lab. Then visit `https://practice-labs.com` to compare the archived page with the live site. The lab guide describes a redirect to `www.acilearning.com/itpro`; this is the guide's example, not a verified present-day redirect.

#### Lab Reference Screenshots

The five images below are provided by the Infosec Learning lab guide and illustrate the remaining steps. They are embedded from the original image links.

**1. Review the capture timeline.** The reference image shows 2024 selected and the available capture history.

![Lab reference: Wayback Machine capture timeline for practice-labs.com](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t2_s4.png)

**2. Select February 27, 2019.** The date popup shows two snapshots: 17:46:10 and 19:16:12.

![Lab reference: two snapshots listed for February 27, 2019](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t2_s5.png)

**3. Choose the 19:16:12 timestamp.** This selects the capture used in the lab.

![Lab reference: selecting the 19:16:12 capture](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t2_s6.png)

**4. Inspect the archived page.** The February 27, 2019 capture displays the Practice Labs branding and welcome page.

![Lab reference: archived Practice Labs welcome page from February 27, 2019](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t2_s7.png)

**5. Enter the live website address.** The reference image shows `https://practice-labs.com` being entered while the archived page remains underneath. It does not show the loaded live page or the redirect destination.

![Lab reference: entering the live Practice Labs URL for comparison](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t2_s8.png)

**What I learned:** Calendar circles indicate archived captures, not proof that the website changed on each date. The archive does not preserve every update or guarantee a complete copy of every page. [Internet Archive's Wayback Machine guide](https://help.archive.org/help/using-the-wayback-machine/)

**Skills practiced:** Searching web archives, selecting dated captures, and comparing historical and live web content.

**Defense connection:** Older pages may preserve branding, contact details, or other information that could support impersonation attempts. Reviewing a site's history can help defenders understand what remains publicly accessible.

**Exercise 4 complete:** I practiced inspecting webpage source and using web archives for historical research.

## Exercise 5 – DNS Footprinting

**Status:** Complete  
**Focus:** Querying DNS records with `nslookup` and reviewing automated enumeration with `dnsenum`.

### Lab Environment

| Device | Operating System | Role |
| --- | --- | --- |
| ACIDC01 | Windows Server 2022 | Domain controller |
| ACIKALI | Kali Purple 2023.1 | Stand-alone Linux workstation used for both tasks |

The screenshots below are reference images from the Infosec Learning lab guide. Record values describe those captures and may differ from later DNS responses.

### Task 1 – DNS Footprint Using Nslookup

#### Prepare the Terminal and DNS Tools

I opened Terminal Emulator on ACIKALI, refreshed the package list, and installed `dnsutils` for the DNS queries.

```bash
sudo apt-get update
sudo apt install dnsutils -y
```

![Lab reference: opening Terminal Emulator on Kali Purple](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s1_3.png)

![Lab reference: refreshing the package list](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s2a_1.png)

![Lab reference: installing dnsutils and its dependencies](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s2b.png)

#### Resolve the Domain

```bash
nslookup practice-labs.com
```

I used a basic lookup to resolve the domain. The reference output identifies `1.1.1.1` as the queried resolver on port 53, followed by a non-authoritative answer containing `199.60.103.192` and `199.60.103.92`.

![Lab reference: basic lookup showing the resolver and returned IP addresses](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s3_4.png)

#### Request A Records

```bash
nslookup -type=A practice-labs.com
```

I requested A records specifically to find IPv4 addresses. The reference shows the same two addresses as the basic lookup.

![Lab reference: A-record lookup for practice-labs.com](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s4_3.png)

I used `clear` between queries to keep the terminal readable. The following reference also shows an earlier typo, `practice-lasbs.com`, returning `NXDOMAIN`, followed by a successful query with the correct spelling.

![Lab reference: corrected domain spelling and clear command](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s5_3.png)

#### Inspect the SOA Record

```bash
nslookup -type=soa practice-labs.com
```

I queried the Start of Authority record to review zone information. The reference lists `ns-444.awsdns-55.com` as the primary server, along with a serial number and refresh, retry, and expiry values.

![Lab reference: SOA record showing zone administration fields](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s6_2.png)

#### Inspect TTL with Debug Output

```bash
nslookup -type=A -debug practice-labs.com
```

I enabled debug output to see more detail about the DNS response. Both A records show a TTL of 123 seconds in this capture, indicating how long those returned records may be cached before they need refreshing.

![Lab reference: debug output showing A records with a TTL of 123 seconds](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s7_1.png)

#### Find Mail Exchange Records

```bash
nslookup -query=MX practice-labs.com
```

I requested MX records to identify the domain's inbound mail routing. The reference shows `practicelabs-com02b.mail.protection.outlook.com` with preference 0; lower preference values take priority when multiple MX records exist.

![Lab reference: MX query showing the mail exchange hostname and preference](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s8_1.png)

#### Identify Authoritative Name Servers

```bash
nslookup -type=ns practice-labs.com
```

I requested NS records to identify the authoritative name servers. The reference lists four servers: `ns-1153.awsdns-16.org`, `ns-1938.awsdns-50.co.uk`, `ns-444.awsdns-55.com`, and `ns-894.awsdns-47.net`.

![Lab reference: NS query listing four authoritative name servers](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s9_1.png)

#### Try an ANY Query

```bash
nslookup -query=any practice-labs.com
```

The reference returns `NOTIMP` (not implemented) instead of a list of records. I learned that ANY is not a reliable way to retrieve every DNS record; querying individual record types gives more useful results. DNS servers may limit ANY responses. [RFC 8482](https://www.rfc-editor.org/rfc/rfc8482.html)

![Lab reference: ANY query returning NOTIMP](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t1_s10_1.png)

### Task 2 – DNS Footprint Using Dnsenum

#### Install the Tool

```bash
sudo apt install dnsenum -y
```

I installed `dnsenum` to practice gathering DNS information with an automated tool.

![Lab reference: installing dnsenum and its dependencies](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t2_s1_1.png)

#### Run the Lab Command

```bash
dnsenum --noreverse practice-labs.com
```

The lab command uses `--noreverse` to skip reverse lookups. Dnsenum can gather host addresses, name servers, and MX records, and perform additional enumeration such as subdomain queries and zone-transfer attempts. It is an active enumeration tool, so its use stays within the authorized lab scope. [Kali's Dnsenum documentation](https://www.kali.org/tools/dnsenum/)

![Lab reference: dnsenum command entered with the noreverse flag](https://infosec-d8-prod.s3.amazonaws.com/2025-02/dnsenum.png)

#### Review the Output

The supplied output reference shows A and CNAME records for names including `portal.practice-labs.com`, `shop.practice-labs.com`, `web.practice-labs.com`, and `www.practice-labs.com`. I learned to distinguish address records from aliases and to read the tool's output carefully.

![Lab reference: dnsenum output showing host records, network ranges, and a reverse-lookup section](https://infosec-d8-prod.s3.amazonaws.com/2024-07/t2_s3_1.png)

**Reference discrepancy:** This output image includes “Performing reverse lookup on 1280 ip addresses,” which does not match the separate `--noreverse` command image. It illustrates enumeration output but does not verify the result of that exact command. Listed network ranges also do not establish that the organization owns every address in those ranges.

**Skills practiced:** Linux package installation, DNS queries, A/SOA/MX/NS record interpretation, TTL inspection, DNS error interpretation, and enumeration-output review.

**Defense connection:** DNS records help defenders understand publicly visible hosts, mail routing, and name-server infrastructure. They provide useful leads for asset review, but do not by themselves prove a vulnerability or reveal all internal systems.

## Key Takeaways

This lab helped me understand how information from search engines, company records, webpage source, web archives, and DNS can build a picture of an organization's online presence. I practiced narrowing searches, identifying technology clues, and interpreting different DNS record types.

I also learned to separate observations from assumptions. Search results and archives can be incomplete, an ANY query may fail, and a screenshot may not match the command described. Checking these details makes my findings more accurate.

From a defense perspective, I can use these skills to review public information, recognize details that could support impersonation, and document technical findings clearly.

## Skills Demonstrated

- Search operators and public-source research (OSINT).
- Company-record searches and jurisdiction filtering.
- HTML source inspection and technology identification.
- Historical website research using the Wayback Machine.
- DNS record queries and enumeration-output interpretation.
- Screenshot documentation and evidence-based reporting.

**Lab complete:** All five exercises are documented.
