---
{"dg-publish":true,"permalink":"/editorial-style-guide-notes/","tags":["WordPress","work"],"noteIcon":"","created":"2024-11-29T09:49:28.225-08:00","updated":"2024-12-03T15:25:20.177-08:00"}
---

[[Dashboard\|Dashboard]] | [[Garden Home\|Garden Home]]

## Editorial Style Guide

### The Challenge

One of the challenges I've encountered in migrating UCSC's [Communications & Marketing (C&M)](https://communications.ucsc.edu/) website over to the new network is how to address the [Editorial Style Guide](https://communications.ucsc.edu/editorial/editorial-style-guide/). The original site is built with a bespoke theme and relies heavily on [Advanced Custom Fields (ACF)](https://www.advancedcustomfields.com/). The new site will be on our campus WordPress network, using the official [UCSC Block Theme](https://github.com/ucsc/ucsc-2022). 

Like most of the original C&M site, the Editorial Style Guide was built using ACF. The style guide itself is a [custom post type (CPT)](https://developer.wordpress.org/plugins/post-types/registering-custom-post-types/) with *26 posts*, with each post consisting of a letter of the English alphabet, A through Z.  

The Style Guide CPT has an ACF field group associated with it that holds the guide "definition" content displayed on the front-end. The ACF field group consists of a single [repeater field](https://www.advancedcustomfields.com/resources/repeater/) that contains two additional fields, a [text field](https://www.advancedcustomfields.com/resources/text/) for the "item" and a [WYSIWYG Editor](https://www.advancedcustomfields.com/resources/wysiwyg-editor/) field for the "definition" (see image below).

The original site was built pre-Gutenberg and the ACF content is rendered in `PHP` using custom loops on custom templates. The task is to import this content and incorporate it into the current Block Theme paradigm.

## The Process
### Export and import fields and content

I was able to export the ACF Field Groups from the current C&M site as `.json` and import them into a local development site that I spun up using [wp-env](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/). In the original site, I registered the CPT via a [custom plugin](https://github.com/ucsc/ucsc-communications-custom-functionality). I can't rely on this plugin on the new site, so I used ACF to create an "empty" CPT on my dev site using the exact same name and slug as on the original site. 

Once this was done, I went into the original site's dashboard and exported *just* the Style Guide's CPT as a `.xml` file and imported it into my dev site.

The import worked perfectly. I had all my posts from the old site and in their editors, I had their ACF fields with all the content. 

## The Problem 

The problem is displaying that content on the front-end. 
### `wp_postmeta` vs `post_content`

ACF data is *metadata* -- it is stored as separate entries in the `wp_postmeta` tables of the database; whereas block data is stored as part of `post_content`. As mentioned, the original site was a complete custom build. I relied on custom templates containing custom loops to render my custom fields on the front-end of the site. I don't have the ability to create custom `PHP` templates in the new site so I need a way to get this content onto the page.

![acf-fields-imported.png](/img/user/attachments/acf-fields-imported.png)
Post content vs Post metadata
## First Attempted Solution
### ACF Block

ACF Pro provides a `PHP` based framework for [creating custom blocks](https://www.advancedcustomfields.com/resources/blocks/) for their fields. My first thought was that I needed to create a new custom block for my ACF Style Guide fields. Using a development plugin, I followed their [tutorial for creating an ACF Block](https://www.advancedcustomfields.com/resources/create-your-first-acf-block/) and was able to create a block that displayed my fields. However, the resulting block only allowed creating *new entries* with those fields; I was unable to display the entries that I imported from the original site via the block. 

![acf-style-block.png](/img/user/attachments/acf-style-block.png)
Custom ACF Block with fields but no data

After much time searching for an answer, I learned that because of the different places ACF data and Block data is stored, [there is not an elegant situation for this situation](https://support.advancedcustomfields.com/forums/topic/converting-to-gutenberg-block/).
## Second Attempted Solution
### Plugin

As part of my research, I came across the [Meta Field Block](https://wordpress.org/plugins/display-a-meta-field-as-block/) plugin in WordPress' Plugin Directory. According to its directory page, it will *display custom fields in WordPress Gutenberg effortlessly*. This did not turn out to be the case in my experiment. While the plugin claims to support "all ACF field types," it did not work for my repeater field in this use case so I abandoned it.

![data-type-not-supported.png](/img/user/attachments/data-type-not-supported.png)
Repeater field not supported in Meta Field Block plugin

## My Solution
### Shortcodes

Since ACF data is stored in `wp_postmeta` and block data is stored in `post_content`, I decided that a [shortcode](https://codex.wordpress.org/Shortcode) would be the best way to display this content. The WordPress Gutenberg editor provides a [Shortcode block](https://wordpress.com/support/wordpress-editor/blocks/shortcode-block/), so a properly developed shortcode (or two) should do the trick. 

As mentioned above, the original bespoke theme used custom loops in custom page and post templates to render field data on the front-end. For the new site, I need to convert the custom loops into shortcodes.

We have a method for adding custom code to UCSC websites. We maintain a [custom functionality plugin](https://github.com/ucsc/ucsc-custom-functionality) that we update regularly. We already provide a few [shortcodes](https://github.com/ucsc/ucsc-custom-functionality/blob/main/lib/functions/shortcodes.php) in this plugin. This means we have a place outside the theme to develop a bit more "custom functionality." The next update of the plugin would make it available for all of our sites (although it would only work on the new C&M site). 
### The Original Single Template Loop

The original site's theme was developed using the [StudioPress Genesis Framework](https://www.studiopress.com/themes/genesis/). The original theme's code below is from the CPT's "`single.php`" template. It removes the default `genesis_loop` and replaces it with the `bb_a_z_style_guide_single_loop()` function, which returns the ACF field data.

```php
remove_action( 'genesis_loop', 'genesis_do_loop' );
add_action( 'genesis_loop', 'bb_a_z_style_guide_single_loop' );

function bb_a_z_style_guide_single_loop(){
	echo '<article class="post type-post status-publish entry">';
	echo '<div class="entry-content" itemprop="text">';
		if( have_rows('style_definitions') ):while( have_rows('style_definitions') ): the_row();
		// vars
		$azItem = get_sub_field('editorial_style_item');
		$azDef = get_sub_field('editorial_style_definition');
		echo '<p><b>'.$azItem.':</b></p>'.$azDef.'<hr>';
		endwhile;
		endif;
	echo '</div>';
	echo '</article>';
}
```

### The Single Post Shortcode
Converting this to a shortcode is not to too difficult. Since I imported my field definitions from the original site, the names in field calls are exactly the same. I stripped out all the structural `html` code, as it's no longer necessary.

#### `return`, don't `echo`

An important thing to remember when writing shortcodes in `PHP` is that they are `return`ed not `echo`ed. So, all the `echo` statements in the above function need to be converted to a `return`. I set up an empty string variable at the beginning that I call `$finaldefs`. I build on it using `.=` concatenation and the `$finaldefs` variable is what is ultimately *returned*.

In the following example, the resulting shortcode is called `[style-definition]`.

```PHP
add_shortcode( 'style-definition','bb_a_z_style_guide_single_loop' );

function bb_a_z_style_guide_single_loop(){

	$finaldefs = '';

	if( have_rows('style_definitions') ):while( have_rows('style_definitions') ): the_row();
		$azItem = get_sub_field('editorial_style_item');
		$azDef = get_sub_field('editorial_style_definition');		
		$finaldefs .= '<p><b>'.$azItem.':</b></p>'.$azDef.'<hr>';
		endwhile;
	endif;

return $finaldefs;
}
```

### The Original Archive Template Loop

For the "Archive template" (I actually used this in a Page not the archive template itself -- but the concept is the same), I wrote a similar loop. The "archive loop" needs to [show *all content* from every CPT post](https://communications.ucsc.edu/editorial/editorial-style-guide/) in addition to some precursor content that appears before it. This is an "A to Z Style Guide" so there are 26 posts in this CPT. The "archive" loop needs to list all posts and their content alphabetically. Navigating the style guide is similar to navigating a dictionary or encyclopedia. The original site's code below does this.

```php
remove_action( 'genesis_loop', 'genesis_do_loop' );
add_action( 'genesis_loop','bb_a_z_styles_archive_loop' );

function bb_a_z_styles_archive_loop() {

	$pageContent = get_the_content();
	$pageContentFormatted = apply_filters('the_content', $pageContent);

	echo '<article class="entry">';
	echo '<div class="entry-content">';
	echo $pageContentFormatted;
	echo '<div class="clear"></div>';
	echo '<div class="two-thirds first">';
	echo '<hr>';

	// WP_Query $args for style guide post type
	$args = array (
	'post_type' => 'a_z_style_guide',
	'orderby' => 'title',
	'order' => 'ASC',
	'posts_per_page' => -1,
	);
	// New WP_Query
	$azDir = new \WP_Query( $args );
	if ($azDir->have_posts()) :
		while ($azDir->have_posts()) :
		$azDir->the_post();
		$azTitle = get_the_title();
		echo '<h2>'.$azTitle.'</h2>';
		// Second loop of style definitions
		if( have_rows('style_definitions') ):while( have_rows('style_definitions') ): the_row();

		// vars
		$azItem = get_sub_field('editorial_style_item');
		$azDef = get_sub_field('editorial_style_definition');
		echo '<p><b>'.$azItem.':</b></p>'.$azDef.'<hr>';
		endwhile;
		endif;
		endwhile;
	endif;

	wp_reset_postdata();
	echo '</div>';
	echo '</div>';
	echo '</article>';

}
```

### The Post Archive Shortcode

Converting the above code to a shortcode was similar to converting the single loop. I stripped out all structural `html` and converted my `echo`s to `return`s. In this code, the returned variable is `$finalloop` and the resulting shortcode is `[style-archive]`.

I didn't need to include any of the `get_the_content()` stuff from the original loop, as we will be able to use the Block Editor for that.

```PHP
add_shortcode( 'style-archive','bb_a_z_styles_archive_loop' );

function bb_a_z_styles_archive_loop() {
	$finalloop = '';

	// Call Post
	$args = array (
	'post_type' => 'a_z_style_guide',
	'orderby' => 'title',
	'order' => 'ASC',
	'posts_per_page' => -1,
	);

	$azDir = new \WP_Query( $args );

	if ($azDir->have_posts()) :
		while ($azDir->have_posts()) :
			$azDir->the_post();
			$azTitle = get_the_title();
			$finalloop .= '<h2>'.$azTitle.'</h2>';
			if( have_rows('style_definitions') ):
				while( have_rows('style_definitions') ):
					the_row();
					// vars
					$azItem = get_sub_field('editorial_style_item');
					$azDef = get_sub_field('editorial_style_definition');
					$finalloop .= '<p><b>'.$azItem.':</b></p>'.$azDef.'<hr>';
				endwhile;
			endif;
		endwhile;
	endif;

	return $finalloop;

	wp_reset_postdata();
}
```
## Incorporating into a Block Theme

Now that I've converted my loop functions to shortcodes, I need to incorporate them into the new site and theme. 
### Single posts

As mentioned above, the post titles of this CPT are simply letters of the alphabet; there are 26 posts, A through Z. As also mentioned, WordPress Core provides a Shortcode Block (shortcodes also work in the Paragraph Block). 

One approach would be to put the Shortcode Block containing the `[style-definition]` shortcode at the top of all 26 posts. This would work, but there is too much repetition (it is not very [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)).

The approach I'm taking on single posts is to use the [WordPress Full Site Editor](https://wordpress.org/documentation/article/site-editor/) to create a new "Single Posts" template for my CPT. 

The default single template has a [Content Block](https://wordpress.com/support/wordpress-editor/blocks/post-content-block/) that displays all content from a post or page's block editor. Because ACF data is not stored in `post_content`, the Content Block will not work for displaying our ACF Data. So, in my CPT single post template I replaced the Content Block with the Shortcode Block and dropped my `[style-definition]` shortcode into it.

![default-single.png](/img/user/attachments/default-single.png)
Default Single Post/Page template with Content Block

![style-single-template.png](/img/user/attachments/style-single-template.png)
CPT Single Post template with Shortcode Block and `[style-definitions]` shortcode

>[!note] NOTE
> Removing the *Content Block* from the Template *will not* remove the *Content Editor* from the single-post editor. As the first image in this post illustrates, the Content Editor will appear above the ACF Field Group. 
>  
> To minimize confusion, it might be advisable to disable the Content Editor altogether for the Style Guide CPT.
> 
> A function such as the following will disable the editor altogether in the CPT, leaving only the ACF  Field Group
> 
> ```PHP 
> add_action( 'init', 'disable_editor_style_guide', 99);
> function disable_editor_style_guide() {
> 	remove_post_type_support( 'a_z_style_guide', 'editor' );
> 	}
> ```
> ![style-guide-editor-disabled.png](/img/user/attachments/style-guide-editor-disabled.png)
> Editor completely disabled on Style Guide CPT
> 

Now, with my CPT Single Template configured, my ACF content shows on the front-end of each post and I can edit the entries in the ACF Field Group area of the editor:

![style-guide-front-end.png](/img/user/attachments/style-guide-front-end.png)
Front-end Style Guide entry with ACF content rendered by shortcode

### Archive "template"

The next task is to create an "archive" that displays all content from every post of the CPT on a single page.

When setting up a CPT in ACF, one of the options in the **Advanced Settings** allows one to create an **Archive** for the CPT that can be controlled via an archive template in the theme.

![cpt-archive-option.png](/img/user/attachments/cpt-archive-option.png)
ACF Archive option in CPT

I am not using the archive template because our "archive" page also needs to have additional precursor content added to it via the Block Editor that appears above the Style Guide content. 

While I'm not using the archive template, I have this option turned on for navigational purposes. As seen in the image above, you have the option of defining an *archive slug* when creating a CPT in ACF (otherwise it inherits the slug of the CPT itself and you can also change its label). This slug will show up in the permalink. 

![[single-permalink.png \| Single CPT post permalink]]
CPT Single Permalink

With this option selected, its slug or its label (if defined) will also show up in the breadcrumbs for the single CPT post.

![single-breadcrumb-with-label.png](/img/user/attachments/single-breadcrumb-with-label.png)
Breadcrumbs showing `/a-z-style-guide/` CPT archive with `Editorial Style Guide` label

If the Archive option is not selected, its slug will not appear in its breadcrumb (it'll still appear in the permalink).

![single-breadcrumbs-no-archive.png](/img/user/attachments/single-breadcrumbs-no-archive.png)
Breadcrumbs of same without CPT Archive option

So, rather than creating a custom archive template for our Style Guide "archive" content, I created a new Page and named it accordingly. I used a redirection plugin to redirect the slug archive `/a-z-style-guide/` to the page slug `/editorial-style-guide/`. This way, when a user clicks "Editorial Style Guide" (the CPT slug's label) in the breadcrumbs, it redirects to the page called "Editorial Style Guide," which is our "archive" page.

I then added the precursor content to our page via the Content Editor and *then* placed the Shortcode Block below it with my `[style-archive]` shortcode in it.

![style-guide-editor-precursor-content.png](/img/user/attachments/style-guide-editor-precursor-content.png)
Style Guide Content Editor with precursor content and Shortcode block

![style-guide-page-precursor-front.png](/img/user/attachments/style-guide-page-precursor-front.png)
Style Guide Page front-end with precursor content above and Style Guide content below

Using this approach, I was able to get the exact same functionality as I had on the original site I am migrating from.
## Conclusion

As I mention at the top of this post, the development community has not come up with a way to bring legacy metadata content into the Block Editor paradigm. If I were starting "from scratch" on this site, I would have developed a custom ACF Block or perhaps explored the Meta Field Block Plugin more extensively. For now, though, developing a shortcode as described in this post seems to be the best way to achieve our desired functionality in the immediate term.


