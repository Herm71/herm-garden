---
{"dg-publish":true,"permalink":"/maps-of-content/","hide":true,"tags":["obsidian","project-management"]}
---

[[Dashboard\|Dashboard]] | [[Garden Home\|Garden Home]] 

>[!note] Obsidian plugins discussed
>- [Dataview](https://github.com/blacksmithgu/obsidian-dataview)
>- [Homepage](https://github.com/mirnovov/obsidian-homepage)

I'm intrigued by the concept of [digital gardening]( https://maggieappleton.com/garden-history), looking at your content as a *collection of loosely organized notes that you allow to grow over time*. One methodology for tending to one's "digital garden" in Obsidian is using [Maps of Content](https://obsidian.rocks/maps-of-content-effortless-organization-for-notes/), which allow a "gardener" to structure their notes organically over time.

In this methodology, one's note structure is flat, which is to say, they are not organized in folders or through a heavy use of tags. 

### No Folders?
Folders are **binary**, a note is either "inside" a folder or it's not. A note could be relevant to any number of "folders" but a note cannot be inside two folders at the same time.

### No Tags?
Using tags in a Vault is appropriate, just not as an organizing principle. Tags require "institutional knowledge," ie., the foreknowledge of what tags are available, and what they are used for. Tags must be kept up with and many Obsidian users have an entire note that explains tagging structure.

### MOCs To The Rescue
The Maps of Content methodology solves for these drawbacks. Let's say you have a "project" of some sort. Some top-level topic that you want to organize some thoughts under. Call it `project-a`. In this parlance, you want to "map" all the content related to `project-a` to one place for easy retrieval. (Again, no folders, no tags.)

#### The Parent
The first step is to create a parent note, a *Map of Content*, for that topic:
```Markdown
project-a.md
```

#### The Children
Now let's say you want to take a note or jot down a thought related to that project. Obsidian supports "Wikilinks" internal linking, meaning you can link back to a note using double-square-brackets.

So, create a new child note related to your project (`deployment-issues.md` - name it whatever you like) and link back to it using the Wikilinks notation:

```Markdown
[[project-a]]

# Note Title
Some thoughts on deployment issues
```

You can create as many notes as you like related to `project-a` and keep them together by backlinking to their parent MOC. In fact, a single note can have many "parents," linking to more than one MOC. This solves the binary folder issue; a note can't be in two folders at the same time but it *can* be in two (or more) MOCs at the same time.
#### Viewing the Map
This is where the **Dataview** plugin comes into play. 

>[!note]
>**Dataview** is an incredibly powerful plugin that turns your static Markdown notes into a database that you can make SQL-like queries against.

In our parent MOC (`project-a.md`), add a code block and make a *Dataview* query that lists *all notes* associated with the project. 

````
- [[Thoughts on Obsidian.md|Thoughts on Obsidian]]

{ .block-language-dataview}
````

 Might look something like this: 

---

![project-a-moc.png|Project A MOC](/img/user/attachments/project-a-moc.png)

Any topic can be organized in this manner. Create MOCs for whatever you want and link child notes accordingly. 
### MOCs of MOCs
Obsidian offers a **Homepage** plugin that allows a user to use any note as a home page or dashboard. It can be set to open automatically once you launch Obsidian, giving you an overview of your notes. 

Linking from your Homepage to your various MOCs gives you easy access to their content. It also allows you drill down and focus on just the MOC (ie., Project) you want to address.

Something like:

![moc-of-mocs.png|MOC of MOCS](/img/user/attachments/moc-of-mocs.png)
### Final Thoughts
As mentioned above, I'm still exploring what Obsidian has to offer. I've been experimenting with this methodology for the past few weeks; but I've also been adapting it for my own uses. I am seeing the benefit of a flat file structure and using a link-based navigation. I might not use *all* of the methods recommended for this structure but I can certainly see its utility.