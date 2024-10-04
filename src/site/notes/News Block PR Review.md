---
{"dg-publish":true,"permalink":"/news-block-pr-review/","hide":true,"noteIcon":"","created":"2024-10-04T07:34:22.972-07:00","updated":"2024-10-04T14:26:47.015-07:00"}
---

[[Dashboard\|Dashboard]] | [[Garden Home\|Garden Home]]

# Overview
Modern Tribe created a PR that creates the News Block in the Custom Functionality Plugin
- [Register News Block #30](https://github.com/ucsc/ucsc-custom-functionality/pull/30)

This PR adds a **News Block** icon in the block chooser of a Page or Post editor with the *ucsc* name space.
# Conclusion
>[!note] TLDR;
>This is a functional prototype. It pulls posts from [Pantheon Dev News](https://dev-news-ucsc.pantheonsite.io/) into a block, returning structured html. Here are a number of fixes and suggestions:
>
>**Fixes:**
>- [[News Block PR Review#Header Alignment\|#Header Alignment]]
>- [[News Block PR Review#More News Link\|#More News Link]]
>- [[News Block PR Review#Whole Card Wrapped in Link\|#Whole Card Wrapped in Link]]
>
>**Questions:**
>- Should the presence of the block be indicated in the [[News Block PR Review#Editor Experience\|#Editor Experience]]?
>- Should [[News Block PR Review#Title and Description\|#Title and Description]] be stylable?
>- Are we omitting [[News Block PR Review#Taxonomies\|#Taxonomies]] on purpose?
>- Should we be able to control the number of [[News Block PR Review#Posts per page\|#Posts per page]]?


# Analysis
This analysis is based on a review of the [Register News Block](https://github.com/ucsc/ucsc-custom-functionality/pull/30) pull request by Modern Tribe.
## Editor Experience
>[!Question]
>Should there be some indicator that there is a "news block" on the page in the editor?

The new block is available in block chooser.
![news-block-block-icon.png](/img/user/attachments/news-block-block-icon.png)

Once the block is dropped onto a Page or Post, however, there is initially no visual indication that the block is *on the page*
![news-block-no-indication.png](/img/user/attachments/news-block-no-indication.png)

But **Block options** appear in the right-hand settings column.
![news-block-options.png](/img/user/attachments/news-block-options.png)

## Block Options
Discussion of the *block options* in the right-hand sidebar.
### Title and Description
>[!Question]
>Should these elements be styleable?

*Title* and *Description* are form fields that return a `<h2>` and `<div>`, respectively; however, they are not "styleable." One cannot adjust their font size, color, background, padding or margin on these elements. 
![news-block-options.png](/img/user/attachments/news-block-options.png)

### Header Alignment
>[!Fix]
>Option to align Header Center or Left does not work. 

The Header's `<h2 class="ucsc-news-block__header-title">` and the description's `<div class="ucsc-news-block__header-description">` elements are wrapped in `<div class="ucsc-news-block__header">`. The **Header alignment** settings target this outer div. 
#### Left Alignment
Selecting *Left alignment* adds a class, `align-header-left`, to the outer `<div>` and has no effect, as the default is already left-aligned. Additionally, there is no style definition for `.align-header-left` in the *Styles* panel of the *Element Inspector*.

![header-alignment-left.png](/img/user/attachments/header-alignment-left.png)

```
<div class="ucsc-news-block__header align-header-left">
<h2 class="ucsc-news-block__header-title">News block</h2>
<div class="ucsc-news-block__header-description">Testing the news block</div>
</div>
```

![news-block-header-align-left.png](/img/user/attachments/news-block-header-align-left.png)

#### Center Alignment
Selecting *Center Alignment* has no effect. No additional classes are added to `<div class="ucsc-news-block__header">`
![header-alignment-center.png](/img/user/attachments/header-alignment-center.png)

```
<div class="ucsc-news-block__header">
<h2 class="ucsc-news-block__header-title">News block</h2>
<div class="ucsc-news-block__header-description">Testing the news block</div>
</div>
```

![news-block-header-align-center.png](/img/user/attachments/news-block-header-align-center.png)

### More News Link
>[!FIX]
>More News link needs better styling

Adding a *More News* link in the options adds a button to the end of the query. The button is not styled well. It is not spaced well and its *editor style* does not match its *front end style*.

![news-block-more-news-option.png](/img/user/attachments/news-block-more-news-option.png)

**Editor button**
![news-block-more-news-button-editor.png](/img/user/attachments/news-block-more-news-button-editor.png)

**Frontend button**
![news-block-more-news-button-frontend.png.png](/img/user/attachments/news-block-more-news-button-frontend.png.png)

### Taxonomies

> [!Question] 
> - Are we purposefully omitting `People`, `Sections` and `Story Types`?
> - How are the taxonomies pulled in?

As the instructions indicate, *both* a **taxonomy type** (Categories, etc. ) *and* a **taxonomy term** need to be selected before content appears in the editor. Taxonomy terms change relative to the type that is chosen. **Multiple terms** can be selected. Currently only *four* taxonomies are available via the block options:
 ![news-block-taxonomies.png](/img/user/attachments/news-block-taxonomies.png)
but there are more on the news site:
- Categories
- Academic Divisions
- Administrative Divisions
- Colleges
- People
- Sections
- Story Types

### Toggle options
>[!NOTE]
>All toggle options work as expected

![news-block-toggle-options.png](/img/user/attachments/news-block-toggle-options.png)

## Additional Issues
### Whole Card Wrapped in Link
>[!FIX]
>The entire *card* is wrapped in an `<a>` tag. 

This is a significant issue. The card's *entire content* is wrapped in an `<a>` tag. Which means *everything is a link.*
Code:
![news-card-links.png](/img/user/attachments/news-card-links.png)

On the front end (hover):
![news-center-post-link.png](/img/user/attachments/news-center-post-link.png)


### Posts per page

>[!Question]
>Should controlling the number news items returned be an option?

The WordPress Query Block provides options to control the number of posts per page that are returned. Should/could we add this as an option to this block? 