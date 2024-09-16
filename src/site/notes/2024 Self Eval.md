---
{"dg-publish":true,"permalink":"/2024-self-eval/"}
---

MOC: [[Web Team MOC\|Web Team MOC]]

# Accomplishments

 - **Launch new UCSC Web Presence**, Sept. 2023
	- I was part of the team that developed and migrated the old WCMS UCSC home presence to WordPress, which launched in Late September
- **UCSC WordPress Theme**
	- [15 PRs successfully merged into the UCSC WordPress theme](https://github.com/ucsc/ucsc-2022/pulls?q=is%3Apr+is%3Aclosed+assignee%3AHerm71), one that is approved but not merged. Notable contributions include:
		- [Corrected issues with images breaking out of their content area](https://github.com/ucsc/ucsc-2022/pull/274). (This is a noted WordPress bug 🐛.)
		- Contributed to the initial design of the [single post template](https://github.com/ucsc/ucsc-2022/pull/290).
		- Returned the ["Last modified" date](https://github.com/ucsc/ucsc-2022/pull/298) to the footer area.
		- [Styled the inline `<code>` element](https://github.com/ucsc/ucsc-2022/pull/301).
		- Added [custom style options to the Details Block](https://github.com/ucsc/ucsc-2022/pull/306) recently added to WordPress Core. 
			- Added deprecation labels to the "Accordion Block" in the Gutenberg Blocks plugin
		- [Added "Zoom" hover effect to all images that contain a link](https://github.com/ucsc/ucsc-2022/pull/351) (`<a>` tag).
- **UCSC Custom Functionality Plugin**
	- [Three PRs successfully merged](https://github.com/ucsc/ucsc-custom-functionality/pulls?q=is%3Apr+is%3Aclosed+assignee%3AHerm71)into plugin, including:
		- [Re-adding the "Edit site" link to logged-in admin bar](https://github.com/ucsc/ucsc-custom-functionality/pull/23).
		- [Audit code to make sure it's "pluggable"](https://github.com/ucsc/ucsc-custom-functionality/pull/24) in preparation for installing on the Faculty WordPress service (note, due to recent budget cuts, this project is on hold).
		- [Added admin settings page under "Settings" menu in the dashboard](https://github.com/ucsc/ucsc-custom-functionality/pull/25).
- **UCSC News Functionality Plugin**
	- [Developed the News Functionality plugin](https://github.com/ucsc/ucsc-news-functionality) for use on the new UCSC News site to address links to external news sites. 
		- Creates a custom post type called "Media Coverage."
		- Creates an ACF Field Group to capture external article URL and to display the article's media source.
- **UCSC News Site**
	- Assisted in the development of the soon-to-be-launched UCSC News site in WordPress
	- Assisted in the development of taxonomies for the new site.
	- Developed a working model using Query Loops in the [Pantheon Multidev Sandbox](https://sandbox-news-ucsc.pantheonsite.io/) site. 
- **UCSC Events Calendar**
	- Facilitated the installation and configuration of the new [Emphasis Theme](https://calendar.ucsc.edu/).
	- Updated the [Events Calendar Docs](https://ucsc.github.io/events-calendar/) site to reflect changes from **Emphasis** theme.
	- Updated the [Events Calendar Users](https://groups.google.com/u/3/a/ucsc.edu/g/events-calendar-group/members) Google group from a Calendar export.
	- Drafted [update communication](https://ucsc.github.io/events-calendar/theme-update-communication) that will be sent to the updated Events Calendar Users group.
	- Continuous ad-hoc support for Calendar users.
-  **Open Labs**
	- Regular contributor and educator in our weekly/bi-weekly Open Labs sessions as a WordPress expert. 
- **Graduate Student WordPress Branding Presentations**
	- Continued my now-annual sessions to graduate students
	- Two sessions in the Fall on general WordPress and Personal Branding
# Failures

- **UCSC Gutenberg Blocks Plugin**
	- The Gutenberg Blocks plugin includes blocks for the _Campus Directory, Class Schedule, Course Catalog_, and the deprecated _Accordion and Accordion Wrapper_ blocks.
	-  In an attempt to clean up and organize the code, I missed the build step (more accurately, I did not realize Kumar had previously shipped the code "built," i.e., he included the `build` directory).
	- I also did not have a proper testing regimen in place so what I thought tested "good" was, in fact, not good at all.
	- This resulted in the above mentioned blocks being unavailable for Campus users for approximately **one week**.
- **Lessons learned**
	- **TESTING** As a result of this failure, I re-vamped my local development environment and established a better testing process.
	- **DON'T BITE OFF MORE THAN I CAN CHEW** The work I did in the GB plugin was done over several months and had *way  too large a scope*. I learned to take issues in smaller bites (bytes? 😃) and to properly scope the intended changes prior to doing the work.
