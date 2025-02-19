---
{"dg-publish":true,"permalink":"/giving-designation-funds-search/","tags":["WordPress","work"],"noteIcon":"","created":"2025-02-19T08:57:45.856-08:00","updated":"2025-02-19T14:45:54.463-08:00"}
---

[[Dashboard\|Dashboard]] | [[Garden Home\|Garden Home]]

## Understanding of the Project
University Advancement, Alumni Giving has an existing [Alumni Giving Designation Search](https://designationsearch.ucsc.edu/search-designations/index.php) tool, which is a custom PHP script. The [[Giving Site Migration\|Giving Site Migration]] to WordPress is underway and this functionality will be incorporated into WordPress. This data exists in a [spreadsheet](https://docs.google.com/spreadsheets/d/1v4mE2ffp2JCcmtBz4rYCke_ZX-y15Rok/edit?usp=sharing&ouid=103689770236599533068&rtpof=true&sd=true) and will need to be imported.
## Approach
The approach to this Project is to utilize **Advanced Custom Fields** to create a *Custom Post Type* with *Custom Fields* that will contain the designation search tool data. Users will be able to search the Funds database based on the data in these custom fields.

A custom WordPress template or page will be used to display the listings. Custom search functionality will be implemented that users can filter and search by the custom fields.

## Constraints
The new Giving site will be on our CampusPress network, which restricts our ability to create a custom plugin; therefore, as much functionality as possible needs to be done within the interface of the WordPress Dashboard.

Here's a mind map: [[Giving Designation Tool.canvas|Giving Designation Tool]]

## Index 


{ .block-language-dataview}

## Tasks


{ .block-language-dataview}

Created: <% tp.date.now("YYYY-MM-DD") %>