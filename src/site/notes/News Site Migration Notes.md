---
{"dg-publish":true,"permalink":"/news-site-migration-notes/","hide":true,"tags":["WordPress","work"]}
---

[[Dashboard\|Dashboard]] | [[Garden Home\|Garden Home]] > [[News Site Migration\|News Site Migration]]

- In the news
- Related Stories: Weird pulling in
- News Block: Not auto-suggesting

## 2025-04-30

### News Block Script
The launch of the UCSC News site on WordPress also brought with it a new News WordPress block for use on other Campus WordPress sites.

This block will enable displaying stories from the News site based on various taxonomies and keywords.

(I've got a Landing Page mock-up in our Sandbox site that has a section called **Research news**. This section was built manually using core WP blocks. In this tutorial, I'm going to replace this section with content from our News site using the News block. )

#### Editor
You can add the News block to a page the way you would add any other block. The easiest way is to type `/news` in an empty paragraph block. You will see the News block in the auto-suggest list. you can also click the Big Blue Plus icon in the top left of your screen to reveal the block chooser panel and either scroll to it or type `news` in the search field.

Once the block is in the content editor, you'll see the big instructions to Select your taxonomies and terms. This is done in the right-hand panel, which has all the block configuration settings.

- Title and Description
- Header Alignment
- More News link -- Explain in more detail
- Taxonomies and terms
- Number of posts to show: 3, 6, or 9
- Options to hide or show various elements
	- There is a known bug with the Author  display and we intend to fix it in a future update, so for now we recommend hiding the author.

#### Build the block
Start building block, NOTE LAG, go through the using an ACADEMIC DIVISION process and view page. (Anthropology)

*View front end.* 
##### Grouping 
Notice there isn't any way to position this block. No margin or padding settings. This can be "fixed" by simply adding the block to a group. That way, you adjust the padding and margins on the group relative to the blocks above or below.

### More News Link 
Now let's discuss the More News Link

This is, of course, where you would add a link to an archive of more stories related to your topics. You can find archives for Topics/Categories and Departments/Divisions from the News home top nav.

#### Single Term
For instance, let's say I'm pulling stories from the **Anthropology Department**. I've got my 3, 6, or 9  Anthro posts. If I want to link to more Anthro posts, I can head over to the News site and find the Anthropology archive by heading to ***News by Division>Anthropology*** and copying its link. I'll paste this link into the "read more" field, add some button text, and we're good-to-go.

#### Multiple Terms
Let's say you're pulling in more than one term per taxonomy. This time, I want to pull stories from ***both*** **Arts & Culture** ***and*** **Health**.

You can combine terms in your "Read More" link by separating the terms with a comma.

First, I'll find the Archive one of my terms, let's go with Arts & Culture. Find the A&C archive by going to ***Explore Topics>Art's & Culture***.

Then, I'll add the second term to the URL separated by a comma, Health. You may add as many terms as you want.

You'll notice that the Archive header does not change -- you will see the header of the first term in the URL -- but we now have a listing of both A&C and Health posts.

We can copy and paste this link into the Read More field in our news block, edit the button text accordingly, and Bob's your uncle.
## 2025-03-30
Notes on latest import:
- **Featured images** are not sized properly. Neither **Media Coverage** nor **Post Query**.
- **Post cleanup:** Converting to Blocks doesn't fix everything. Sometimes it creates a *Custom HTML* block to address non-necessary `HTML` that get's imported.
- **Media Coverage** archive loop ("All Media Coverage" link) does not link out to source, it links to the Single template.

## 2025-02-27
UCSC News Single Post overrides the default Single Post template 

oembed templates?

Third Party == External Giving Link

## 2024-12-03
News Block notes

## 2024-11-05
Final crunch. Setting sprints and TODOs. Starting today, first sprint runs until next Tues. JC will need to review the news block.

## 2024-10-01
Modern Tribe design review discussion.
- [Review Document](https://docs.google.com/document/d/1gwy3w59uFwPGXfMknBG-Pqb-AoEX5wqvo3M55taK94I/edit#heading=h.8gkvacbz93ia)
- [Loom Design Walkthrough](https://www.loom.com/share/28fe32e92f7b49b8b9598484cf86dbe6?sid=cf81278d-efcb-4141-8b09-3ff966d5d4d0)
## 2024-09-17

## Spinning plates (Rob's word)
- News Block for WordPress sites
	- Waiting or testing
- Data integrity of the import(s)
- Category re-implementation
- Retroactive category filling (Sections)
- Final data import
- Design of three templates:
	- In progress. Need timeline
	- Home page
	- Blog page
	- Single story page
- Implement three templates
- Training for writers and C&M staff
- Redirects for numeric URLs
- Solution for WCMS sites when news switches over
## 2024-08-20

- [x] Discuss Email Newsletter Archives, whether and how we should do it. 📅 2024-08-20 ✅ 2024-08-20
Abby agrees we should not archive TN and others.

Lisa asks if there's ever a need by writers to go back. Abby says she's got them in her inbox.

SalesForce doesn't even archive.

In the end, Lisa says "check with Scott."

- [x] Recreate `Local Dev` in Pantheon `Multidev` ➕ 2024-08-20 📅 2024-08-20 ✅ 2024-08-21

# Project Milestone
- [x] Demo News Site 📅 2024-12-09 ✅ 2025-04-30