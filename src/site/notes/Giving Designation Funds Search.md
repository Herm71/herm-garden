---
{"dg-publish":true,"permalink":"/giving-designation-funds-search/","tags":["WordPress","work"]}
---

[[Dashboard\|Dashboard]] | [[Garden Home\|Garden Home]]

## Understanding of the Project
University Advancement, Alumni Giving has an existing [Alumni Giving Designation Search](https://designationsearch.ucsc.edu/search-designations/index.php) tool, which is a custom PHP script. The [[Giving Site Migration\|Giving Site Migration]] to WordPress is underway and this functionality will be incorporated into the new site. This data exists in a [spreadsheet](https://docs.google.com/spreadsheets/d/1v4mE2ffp2JCcmtBz4rYCke_ZX-y15Rok/edit?usp=sharing&ouid=103689770236599533068&rtpof=true&sd=true) and will need to be imported.
## Approach
The approach to this Project is to utilize **Advanced Custom Fields** to create a *Custom Post Type* with *Custom Fields* that will contain the designation search tool data. Users will be able to search the Funds database based on the data in these custom fields.

A custom WordPress template or page will be used to display the listings. Custom search functionality will be implemented that users can filter and search by the custom fields.
## Constraints
The new Giving site will be on our CampusPress network, which restricts our ability to create a custom plugin; therefore, as much functionality as possible needs to be done within the interface of the WordPress Dashboard.

## Mind Map
Here's a mind map: [[Giving Designation Tool.canvas|Giving Designation Tool]]

## Notes and Tasks

### 2025-03-13
**Rob/Jason Naming Review**
- [x] Test plugin, release ✅ 2025-03-14
- [x] issue with side placement ✅ 2025-03-14
- [x] issue with double-save ✅ 2025-03-14

### 2025-03-10
**Fund Designation Demo**

---
- [x] Ask Jenna what Form, Code AQ_Code 📅 2025-02-28 ✅ 2025-03-06
- [x] Move code to UCSC Giving Functionality Plugin 📅 2025-02-28 ✅ 2025-03-06
- [x] Code Priority/Link post switch ✅ 2025-03-06
- [x] Ask Jenna about extra "Area" codes in spreadsheet 📅 2025-03-07 ✅ 2025-03-13


Fund link = link plus appended AQ Code for button URL.
example: https://give.ucsc.edu/campaigns/38026/donations/new?designation=a1K8c00000i25zqEAA

Generic fund link with tracking code: https://give.ucsc.edu/default-giving-form/?a=10686891



- [x] Demo for Jenna 📅 2025-03-10 ✅ 2025-04-30
- [x] Reenable block editor ✅ 2025-03-06
figure distinction between Priority Funds and "normal" funds
## Index 

- [[Giving Functionality Plugin Readme\|Giving Functionality Plugin Readme]]

{ .block-language-dataview}

## Tasks


{ .block-language-dataview}

Created: <% tp.date.now("YYYY-MM-DD") %>