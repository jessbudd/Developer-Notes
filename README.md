# Dev Tricks, Notes & Hacks

This repo is a list of notes for my future reference.


## Shortcuts
To get to the cpanel login of most pages type `":2082"` to the end of the url address.

To get to the webmail login of most pages type `":2096"` to the end of the url address.

To get the IP of a site, type `"nslookup [site address]"` into CLI.


## SNIPPETS

### Hide nav on scroll down, show on scroll up
<pre>
var prevScrollpos = window.pageYOffset;
window.onscroll = function() {
  var currentScrollPos = window.pageYOffset;
  if (prevScrollpos > currentScrollPos) {
    document.getElementById("navbar").style.top = "0";
  } else {
    document.getElementById("navbar").style.top = "-100px"; //however tall your nav is
  }
  prevScrollpos = currentScrollPos;
}
</pre>

### Accessible smooth scrolling (moves keyboard focus)
<pre>
    $(function() {
      $('a[href="#id"]').click(function() {
        if (location.pathname.replace(/^\//,'') == this.pathname.replace(/^\//,'') && location.hostname == this.hostname) {
          var target = $(this.hash);
          target = target.length ? target : $('[name=' + this.hash.slice(1) +']');
          if (target.length) {
            $('html, body').animate({
              scrollTop: target.offset().top
            }, 1000);
            target.focus(); // Setting focus
            if (target.is(":focus")){ // Checking if the target was focused
              return false;
            } else {
              target.attr('tabindex','-1'); // Adding tabindex for elements not focusable
              target.focus(); // Setting focus
            };
            return false;
          }
        }
      });
    });
    </pre>

## WORDPRESS

### Add PHP to text widget
See gist: https://gist.github.com/jessbudd/11658238ffdcd3e3b2ea1ac6678e9a94

### Deregister css/js of plugin on front page only
see gist: https://gist.github.com/jessbudd/293b8012740183fefae092c074c92c38

https://mor10.com/how-to-remove-wp-geo-plugin-from-specific-pages/

### Conditional to only perform function on posts instead of pages and custom post types
<pre>
   if(is_single() &&  get_post_type() == 'post') mk_get_view('blog/components', 'blog-single-bold-hero');
</pre>

### Add New User Via FTP
Add the following code to the functions.php file.

<pre>function wpb_admin_account(){
$user = 'Username';
$pass = 'Password';
$email = 'email@domain.com';
if ( !username_exists( $user )  && !email_exists( $email ) ) {
$user_id = wp_create_user( $user, $pass, $email );
$user = new WP_User( $user_id );
$user->set_role( 'administrator' );
} }
add_action('init','wpb_admin_account');
</pre>


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

### GeneratePress: Add contact or button to header
Adding a button or contact details in the header to the right of logo. See gist for code and instruction:
https://gist.github.com/jessbudd/e1ffe65b634f5704c5fcc2f4d382f158

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

### Create New User From Cpanel

See Gist

https://gist.github.com/jessbudd/7d9a4fa4c5cd843b4cd8931f4221da93

## Woocommerce 

### Add Icon to Cart Button 
Add font-awesome to head link and this code to custom css:

<pre>	.woocommerce a.button::after, .woocommerce button.button::after, .woocommerce input.button::after {
		font-family: 'FontAwesome';
		content: ' \f08a'; <--font awesome code-->
		}
	</pre>
	
### Query if woocommerce member logged in

```
 <?php

    if ( wc_memberships_is_user_member() ) {
	    echo 'Marco';
	} else {
	    echo 'Polo';
	}
	;?>
```
### Move product tabs to short description area
https://gist.github.com/jessbudd/1a65c1d57a0e017cdb0c945b50ec14a3

### Add Recent Products to Theme Template
See gist: https://gist.github.com/jessbudd/66335479ac69391e038d85dfc95e94a4

### Add Most Popular Products to Theme Template 
See gist: https://gist.github.com/jessbudd/66232f577743fd5fa1b542c48b102f20

### Add Backorder Badge To Out of Stock Products 
See gist: https://gist.github.com/jessbudd/32b2bc6450750643683b21baa8b89274

This works on the product archive page only. Single product pages will have a text warning applied in the description when the item is set to "in stock" and backorders "allowed, but notify customer".



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

### Disable google map scrolling on iframes

Add a div with an .overlay exactly before each gmap iframe insertion, see:

```
 <div class="overlay" onClick="style.pointerEvents='none'"></div>
  <iframe src="https://mapsengine.google.com/map/embed?mid=some_map_id" width="640" height="480"></iframe>
```
CSS

<pre>
.overlay {
   background:transparent; 
   position:relative; 
   width:640px;
   height:480px; /* your iframe height */
   top:480px;  /* your iframe height */
   margin-top:-480px;  /* your iframe height */
}
</pre>
The div will cover the map, preventing pointer events from getting to it. But if you click on the div, it becomes transparent to pointer events, activating the map again.

### Centering Content
If you want to have your content vertically aligned to center, set parent div to `display: table` with current div set to ```display:table cell; vertically-align: middle;```

### Clicking "through" divs
This is particularly useful when you have an absolutely positioned icon over a select input which does not activate the drop down when clicking the icon. 

To access the element below another element set higher element with property/value `pointer-events:none`.

## Command Line Tips

### Resize all jpgs in folder to max width

sips -Z 640 *.jpg

or

sips --resampleWidth 1024 *.jpg
sips --resampleHeight 768 *.jpg
sips -z 768 1024 *.jpg # -z takes height as a first argument

https://www.lifehacker.com.au/2012/11/batch-resize-images-quickly-in-the-mac-os-xs-terminal/
https://hackernoon.com/save-time-by-transforming-images-in-the-command-line-c63c83e53b17


## Internet Explorer
### Word Breaks
`<wbr>` is not supported by any version of IE. If word-wrap:word-break and word-break: word-all do not work, you can insert the following code after the element:
`<img src="transp.gif" width="0" height="0" alt="">`

### IE only styling
<pre>     @media all and (-ms-high-contrast: none), (-ms-high-contrast: active) {
     		  /*insert styles here*/
            }
       }</pre>


### Edge only styling
<pre>     @media all and (-ms-ime-align: auto) {
      		 /*insert styles here*/
            }
       }</pre>



### HTML

Some HTML5 tags do not work in any version of IE (eg `<main>`) and you need to explicitly add the display property to the css. 


### Auto Updating Year on Static Sites

 ```<script type="text/javascript">document.write("© Collins Moore " + new Date().getFullYear()) ;</script>```
  
 
