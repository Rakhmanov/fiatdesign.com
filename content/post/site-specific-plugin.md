+++
title = "Wordpress - Creating Site specific plugin"
date = "2015-08-14"
author = "Principal"
description = ""
tags = ["Wordpress"]
categories = ["Development"]
+++

Why create wordpress modification as a plugin? That allow us to keep theme files (function.php) clean of setup or plugin specific modifications. With that said lets begin.

Create a folder in the plugin directory of your wordpress website.
Create index.php file with following content as template for plugin:
<pre>&lt;?php
/*
Plugin Name: 
Description: 
*/

/* Below this line content of the plugin */

?&gt;</pre>
&nbsp;

As you can see its pretty simple. To get started, as it always with wordpress. However there are some difference from how plugin operates compared with theme.
<ul>
 	<li>Include paths
<ul>
 	<li>To include any of your file in to the plugin or a theme we use function that determines its location. That includes JS, CSS or images. In theme location is passed with get_template_directory_uri(),In theme they are passed with get_template_directory_uri(),
In child theme its get_stylesheet_directory_uri(),
while for plugins its: plugin_dir_url( __FILE__ ).
Followed by the file name with extension.</li>
</ul>
</li>
 	<li>Execution order
<ul>
 	<li>Plugins are loaded before themes, therefore some of the hooks may not be available as plugins themselves are executed in wordpress determined order. So your site specific plugin maybe executed before required hooks or action becomes available. <a href="/wordpress/plugin-load-order/">Little snippet on how to solve it.</a></li>
</ul>
</li>
</ul>
Keep this things in mind when moving the code from function.php to your new site specific plugin. If you have encountered any other problems with migration to plugin from theme, feel free to comment.