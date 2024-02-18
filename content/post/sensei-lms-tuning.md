+++
title = "Sensei is a Learning Platform Plugin tuning"
date = "2015-08-14"
author = "Principal"
description = ""
tags = ["Wordpress"]
categories = ["Development"]
+++

Sensei is a Learning Platform Plugin, with build-in Woocommerce integration. It is also developed by Woothemes, company which was recently purchased by Automatic (Owner of Wordpress) to simplify building of e-commerce websites.
Sensei is premium plugin, means you need to buy it, but its also GPL, and has its development open on https://github.com/woothemes/sensei.

By The Default Sensei Supports only multiple choice, fill in the blank, and true and false, auto-grading. However in the recent project I have worked on I have encountered need for auto-grading the questions of the single line, multi-line and file upload types. So in order to do it we need to make couple of things.

I recommend create <a href="http://fiatdesign.com/wordpress/wordpress-creating-site-specific-plugin/">Site specific plugin</a> for this modification.

First allow in administrator menu for this question types to be gradable automatically. That achieved with simple jquery.

Create the JS file, with this content:
and save it as sensei/sensei-admin-scripts.js
<pre>jQuery(document).ready(function($){
	$( 'input#quiz_grade_type' ).removeAttr( 'disabled' );
	$( 'input#quiz_grade_type' ).closest('p.form-field').removeClass( 'disabled' );
	$( 'input#quiz_grade_type_disabled' ).val( 'enabled' );
});
</pre>
Then we create an index.php file with our plugin content
<pre>
&lt;?php
add_action( 'admin_enqueue_scripts', 'load_custom_sensei_admin_script');

add_filter( 'sensei_autogradable_question_types', function($supported){return array_merge( $supported, array('multi-line','single-line','file-upload'));});

add_filter( 'sensei_grade_question_auto', 'sensei_autograde_question_score', 10, 4 );

function load_custom_sensei_admin_script() {
	wp_enqueue_script( 'custom-sensei-admin-script', plugin_dir_url( __FILE__ ).'sensei/sensei-admin-scripts.js');
}

?&gt;
</pre>
<strong>Update: 8/19/15</strong>
In future 2.0 Release it is planned to have ability to set questions without grading, enhancement which I have purposed back in july (https://github.com/woothemes/sensei/issues/962).

So this logic will be changed. But for now this solution works perfectly.
+++
title = "The Ultimate Guide to Eyelash Extensions: Enhancing Your Natural Beauty"
date = "2015-08-14"
author = "Principal"
description = "Discover everything you need to know about eyelash extensions, from benefits and the application process to aftercare tips."
tags = ["Wordpress", "Beauty", "Lashes", "Beauty Tips"]
categories = ["Beauty"]
+++

Sensei is a Learning Platform Plugin, with build-in Woocommerce integration. It is also developed by Woothemes, company which was recently purchased by Automatic (Owner of Wordpress) to simplify building of e-commerce websites.
Sensei is premium plugin, means you need to buy it, but its also GPL, and has its development open on https://github.com/woothemes/sensei.

By The Default Sensei Supports only multiple choice, fill in the blank, and true and false, auto-grading. However in the recent project I have worked on I have encountered need for auto-grading the questions of the single line, multi-line and file upload types. So in order to do it we need to make couple of things.

I recommend create <a href="http://fiatdesign.com/wordpress/wordpress-creating-site-specific-plugin/">Site specific plugin</a> for this modification.

First allow in administrator menu for this question types to be gradable automatically. That achieved with simple jquery.

Create the JS file, with this content:
and save it as sensei/sensei-admin-scripts.js
<pre>jQuery(document).ready(function($){
	$( 'input#quiz_grade_type' ).removeAttr( 'disabled' );
	$( 'input#quiz_grade_type' ).closest('p.form-field').removeClass( 'disabled' );
	$( 'input#quiz_grade_type_disabled' ).val( 'enabled' );
});
</pre>
Then we create an index.php file with our plugin content
<pre>
&lt;?php
add_action( 'admin_enqueue_scripts', 'load_custom_sensei_admin_script');

add_filter( 'sensei_autogradable_question_types', function($supported){return array_merge( $supported, array('multi-line','single-line','file-upload'));});

add_filter( 'sensei_grade_question_auto', 'sensei_autograde_question_score', 10, 4 );

function load_custom_sensei_admin_script() {
	wp_enqueue_script( 'custom-sensei-admin-script', plugin_dir_url( __FILE__ ).'sensei/sensei-admin-scripts.js');
}

?&gt;
</pre>
<strong>Update: 8/19/15</strong>
In future 2.0 Release it is planned to have ability to set questions without grading, enhancement which I have purposed back in july (https://github.com/woothemes/sensei/issues/962).

So this logic will be changed. But for now this solution works perfectly.