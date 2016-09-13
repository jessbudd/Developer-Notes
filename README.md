# My Tricks & Notes

This repo is a list of hacks, work arounds and tricks to keep handy for future reference.

##IE HACKS
###Word Breaks
<wbr> is not supported by any version of IE. If word-wrap:word-break and word-break: word-all do not work, you can insert the following code after the element:
`<img src="transp.gif" width="0" height="0" alt="">`

##Shortcuts
To get to the cpanel login of most pages type `":2082"` to the end of the url address.

To get to the webmail login of most pages type `":2096"` to the end of the url address.

To get the IP of a site, type `"nslookup [site address]"` into CLI.




##WORDPRESS

###Sub-Menus Not Appearing
If sub-menus show in HTML and it's not a CSS issue (is the element set to display:none?), then it's likely a PHP or wordpress config issue.
To check php arrays, wrap in a var_dump eg
```<?php var_dump(wp_nav_menu( array( 'theme_location' => 'primary', 'menu_class' => 'nav-menu', 'walker' => $walker ))); ?>```


###Genesis: Add Widget Area Easily
Put this in functions.php. This example is for a slider that appears on all pages, but could be modified easily. The key thing to note is the add_action line, it controls where the widget will go.
<pre>```/** Slider widget area for every page */
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
} ```</pre>

###Woocommerce - Add Icon to Cart Button 
Add font-awesome to head link and this code to custom css:

```	.woocommerce a.button::after, .woocommerce button.button::after, .woocommerce input.button::after {
		font-family: 'FontAwesome';
		content: ' \f08a'; <--font awesome code-->
	}```


###Local Dev Site Redirects to Live Site
It's likely you've forgotten to change the site url. There are four methods to do this (https://codex.wordpress.org/Changing_The_Site_URL), the most simple being adding this code to wp-config file:

<pre>define('WP_HOME','http://example.com');
define('WP_SITEURL','http://example.com')</pre>


## BOOTSTRAP
If you are experiencing a gap on the right side of part of the website (resulting in horizontal scroll bar) it is likely that you are missing a container on one of the sections. Check this before all else. If still having issues (particularly at only certain view points) ensure that the container class left and right margins are set to 15px. (If you want to remove the margins for section, do so on the inner element not the container)
