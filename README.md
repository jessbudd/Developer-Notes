# Dev Tricks, Notes & Hacks

This repo is a list of notes for my future reference.

## Internet Explorer
### Word Breaks
`<wbr>` is not supported by any version of IE. If word-wrap:word-break and word-break: word-all do not work, you can insert the following code after the element:
`<img src="transp.gif" width="0" height="0" alt="">`

### HTML
Some HTML5 tags do not work in any version of IE (eg `<main>`) and you need to explicitly add the display property to the css. 

## Shortcuts
To get to the cpanel login of most pages type `":2082"` to the end of the url address.

To get to the webmail login of most pages type `":2096"` to the end of the url address.

To get the IP of a site, type `"nslookup [site address]"` into CLI.




## WORDPRESS

### Read More Links
If your theme contains the_excerpt() function (https://developer.wordpress.org/reference/functions/the_excerpt/), the insert "read more"  tag from the post editor will cause the "read more" link to be overridden and not appear on those posts. This is a problem when some posts contain the read more tag and others do not, as it will cause inconsistency on the main page. 

There are two steps to get around this. 

First you need to remove any functions that create the "read more" link to excerpts in the theme's functions.php file. It should look something like this, and can be commented out.
<pre>/**
 * Returns a "Read more" link for excerpts
 
function irony_excerpt_more( $more ) {
	return '<a class="more-link" href="'. get_permalink( get_the_ID() ) . '">' . __( 'Read more', 'irony' ) . '</a>';
}
add_filter( 'excerpt_more', 'irony_excerpt_more' );
*/</pre>

Secondly, you want to add a read more link to alln excerpts, regardless of their length or whether they contain a read more tag or not. Paste this code into your functions.php file.

<pre>
//Read More Button For Excerpt
function themeprefix_excerpt_read_more_link( $output ) {
	global $post;
	return $output . ' <a href="' . get_permalink( $post->ID ) . '" class="more-link" title="Read More">Read More</a>';
}
add_filter( 'the_excerpt', 'themeprefix_excerpt_read_more_link' );
</pre>

### Sub-Menus Not Appearing
If sub-menus show in HTML and it's not a CSS issue (is the element set to display:none?), then it's likely a PHP or wordpress config issue.
To check php arrays, wrap in a var_dump eg
<?php var_dump(wp_nav_menu( array( 'theme_location' => 'primary', 'menu_class' => 'nav-menu', 'walker' => $walker ))); ?>


### Genesis: Add Widget Area Easily
Put this in functions.php. This example is for a slider that appears on all pages, but could be modified easily. The key thing to note is the add_action line, it controls where the widget will go.
<pre>/** Slider widget area for every page */
genesis_register_sidebar( array(
	'id'	=> 'slider-everywhere',
	'name'	=> __( 'Everywhere Slider', 'ally' ),
	'description'	=> __( 'Similar to the normal "slider" widget, but appears everywhere, not just the homepage.', 'ally' ),
) ); 
add_action( 'genesis_after_header', 'custom_do_slider_everywhere', 10 );
function custom_do_slider_everywhere() {
	genesis_widget_area( 'slider-everywhere', array(
		'before' => '<div id="slider-wrap"><div class="slider-inner">',
		'after' => '</div></div>',
	) );
} </pre>

### Woocommerce - Add Icon to Cart Button 
Add font-awesome to head link and this code to custom css:

<pre>	.woocommerce a.button::after, .woocommerce button.button::after, .woocommerce input.button::after {
		font-family: 'FontAwesome';
		content: ' \f08a'; <--font awesome code-->
		}
	</pre>

### Make First Post Image Appear in Excerpt
This will take the first image of a post when there is no feature image set.
<pre>function improved_trim_excerpt($text) {
        global $post;
        if ( '' == $text ) {
                $text = get_the_content('');
                $text = apply_filters('the_content', $text);
                $text = str_replace('\]\]\>', ']]&gt;', $text);
                $text = preg_replace('@<script[^>]*?>.*?</script>@si', '', $text);
                $text = strip_tags($text, '<img>');
                $excerpt_length = 80;
                $words = explode(' ', $text, $excerpt_length + 1);
                if (count($words)> $excerpt_length) {
                        array_pop($words);
                        array_push($words, '[...]');
                        $text = implode(' ', $words);
                }
        }
        return $text;
}
remove_filter('get_the_excerpt', 'wp_trim_excerpt');
add_filter('get_the_excerpt', 'improved_trim_excerpt');
</pre>

### Local Dev Site Redirects to Live Site
It's likely you've forgotten to change the site url. There are four methods to do this (https://codex.wordpress.org/Changing_The_Site_URL), the most simple being adding this code to wp-config file:

<pre>define('WP_HOME','http://example.com');
define('WP_SITEURL','http://example.com')</pre>


### Favourite Plugins  

* Booking plugin (premium) with calendar function, forms, payments and supports half days [Bookly](https://codecanyon.net/item/bookly-booking-plugin-responsive-appointment-booking-and-scheduling/7226091)
* Backup plugin with automatic backups to dropbox/drive [Updraft](https://updraftplus.com/)
* Portfolio plugin [Huge-It](https://huge-it.com/wordpress-portfolio-gallery-user-manual/)
* Grid layout plugin for your posts and pages (pro version supports woocommerce products etc) [Content Views](https://contentviewspro.com/)
* Basic free slider [Meta Slider](https://wordpress.org/plugins/ml-slider/)

## Accessibility

Screen reader text CSS
<pre>{position:absolute;
left:-10000px;
top:auto;
width:1px;
height:1px;
overflow:hidden;}</pre>

## BOOTSTRAP
If you are experiencing a gap on the right side of part of the website (resulting in horizontal scroll bar) it is likely that you are missing a container on one of the sections. Check this before all else. If still having issues (particularly at only certain view points) ensure that the container class left and right margins are set to 15px. (If you want to remove the margins for section, do so on the inner element not the container)

## MISCELLANEOUS 

### Centering Content
If you want to have your content vertically aligned to center, set parent div to `display: table` with current div set to ```display:table cell; vertically-align: middle;```

### Clicking "through" divs
This is particularly useful when you have an absolutely positioned icon over a select input which does not activate the drop down when clicking the icon. 

To access the element below another element set higher element with property/value `pointer-events:none`.

