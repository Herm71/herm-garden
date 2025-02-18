---
{"dg-publish":true,"permalink":"/news-site-build-notes/","hide":true,"tags":["WordPress","work"],"noteIcon":"","created":"2025-02-16T08:21:26.857-08:00","updated":"2025-02-17T22:47:26.719-08:00"}
---

[[Dashboard\|Dashboard]] | [[Garden Home\|Garden Home]] > [[News Center Migration\|News Center Migration]]

Notes on building out the News Site based on Modern Tribe's completed work and general notes as this is the first build of the entire site.

## Featured News Block
- The **FNB** cannot be used in an *Archive Template*. It will not pull in the posts from that archive (e.g., the *Sections Template*). Selecting the "Most Recent Posts" option pulls in the most recent *Posts* based on the primary post-query, not the most recent posts of a taxonomy, such as *Sections*.
- Featured images in the **FNB** do not "zoom on hover".
## Section Archive
As discussed, the **FNB** will not work on archive templates. We need a different approach to this information architecture. I currently have these "archives" developed as Pages on the site for demonstration and discussion.
## POTW Archive
- The *Photo of the Week* archive has a different pagination design than the overall theme pagination.
- *POTW* archive's pagination displays regardless of whether there are enough posts to fill the page, unlike the normal WordPress pagination
## Archive Templates
Our archive templates are not consistent. 
- **Category Archive** is the template created by Modern Tribe, which is similar in design to the **Blog Home** template (another archive). It utilizes the **post-query** block that was also designed by MT. 
- **All Archives** is the "generic" template that would display all other taxonomy archives, such as **Tags** or **Sections**, unless a custom template was to be created for them.
- **Media Coverage Archive** is the template that shows all our Media Coverage entries. Neither the **Category Archive** nor the **All Archives** templates are appropriate for these items. I have created a new archive with a new loop to display these based loosely on the **POTW Archive**
- **Section Archives** display posts from our various sections. Our approach to these might need to be adjusted.
- **Photo of the Week Archive** developed by Modern Tribe. Looks good but is inconsistent with some of the overall design elements.
## "Related Topics" and taxonomies blocks
The **Single Post Template** developed by MT adds additional text and styling to the default taxonomies blocks (Categories, Tags, Sections, etc.), namely a header that says "Related Topics." This treatment appears *no matter where* one uses the taxonomies blocks. I'm not sure if this is the desired affect.
# Tasks
- [ ] Todos go here

