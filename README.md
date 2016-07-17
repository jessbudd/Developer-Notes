# My Tricks & Notes

This repo is a list a list of hacks, work arounds and tricks I'd like to keep handy for future reference.

##IE HACKS
###Word Breaks
<wbr> is not supported by any version of IE. If word-wrap:word-break and word-break: word-all do not work, you can insert the following code after the element:
<img src="transp.gif" width="0" height="0" alt="">

##Shortcuts
To get to the cpanel login of most pages type ":2082" to the end of the url address.
To get to the webmail login of most pages type ":2096" to the end of the url address.
To get the IP of a site, type "nslookup [site address]" into CLI.


##WORDPRESS

###Sub-Menus Not Appearing
If sub-menus show in HTML and it's not a CSS issue (is the element set to display:none?), then it's likely a PHP or wordpress config issue.
To check php arrays wrap in a var_dump eg
<pre><?php var_dump(wp_nav_menu( array( 'theme_location' => 'primary', 'menu_class' => 'nav-menu', 'walker' => $walker ))); ?></pre>

###Genesis: Add Widget Area Easily
Put this in functions.php. This example is for a slider that appears on all pages, but could be modified easily. The key thing to note is the add_action line, it controls where your widget will go.
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

###Gensis: Customis Footer
This requires additions to functions.php.To replace the go-to-top section (left-hand side of footer):
<pre>/* Customize the footer Return to Top */
add_filter('genesis_footer_backtotop_text', 'custom_footer_backtotop_text');
function custom_footer_backtotop_text() { ?>
<div class="gototop">
<p>Replacement Text</p>
</div>
<?php }</pre>
To replace the credits section (right-hand side of footer):
<pre>/* Customize the credits */
add_filter( 'genesis_footer_creds_text', 'custom_footer_creds_text' );
function custom_footer_creds_text() { ?>
<div class="creds">
<p>Copyright © <?php echo date('Y'); ?>
 · website developed by <a href="http://jessica-budd.com" target="_blank">
<img src="/wp-content/uploads/2014/06/mh-logo-150-clear.png" style="width:32px;height:32px;vertical-align:middle;"/>Jessica Budd</a>
</p></div>
<?php }</pre>
.... can be used for the footer icon.


###Genesis:Customis Post Meta Data
Post meta is the author name, post date etc associated with each post.
This code, in functions.php, will remove the post meta area completely.
<pre>//* Customize Entry Meta Filed Under and Tagged Under - http://www.basicwp.com/?p=268
add_filter( 'genesis_post_meta', 'ig_entry_meta_footer' );
function ig_entry_meta_footer( $post_meta ) {
	$post_meta = '';
	return $post_meta;
}</pre>
This code, in functions.php, will remove the author name and leave the date, comments and an edit link.
<pre>/** Customise the post meta */
add_filter( 'genesis_post_info', 'mhm_meta_post_info' );
function mhm_meta_post_info($post_info) {
	if (!is_page()) {
	$post_info = '[post_date] [post_comments] [post_edit]';
		return $post_info;
	}
}</pre>
