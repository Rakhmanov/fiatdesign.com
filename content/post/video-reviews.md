+++
title = "Video Reviews for Woocommerce & Site Comments, 2 Easy Solutions."
date = "2015-08-15"
author = "Principal"
description = ""
tags = ["Wordpress"]
categories = ["Development"]
+++

Have you ever wanted to include video reviews to your Woocommerce store? Or allow users to post videos with comments, which works same as reviews. Well now we will see how to achieve this with two ways.

First Solution is to have users to post comment with necessary html code for embed the video.
To make this work our default user role need to have necessary (unfiltered_html) permission. Dont be scared its easy to do with 2 lines of code. Make sure you selected correct user role, by default its 'subscriber', but may change with woocommerce as it creates role 'customer'.
<pre>$role = get_role( 'subscriber' );
$role-&gt;add_cap( 'unfiltered_html' );
</pre>
Now this allowed users to post unfiltered html to our website. Okay as you might already guessed it can result in some html code be posted. If you manually review each comment that should not be big problem.
And user needs to know how to acquire html for video embed to be posted in comments.
This would look something like this:&nbsp;<em>&lt;iframe width="560" height="315" src="https://www.youtube.com/embed/7V-fIGMDsmE" frameborder="0" allowfullscreen&gt;&lt;/iframe&gt;</em>.

However that's not it, I have promised 2 solution and here is the second and better solution.

We create filter that hooks in to the comment and checks for embeds.
Everything is done with build-in wordpress functionality, without giving permissions or html. All we need is link to the video, or other supported build in Wordpress embed service.
<pre>$wp_embed = new WP_Embed;
add_filter( 'comment_text', array( $wp_embed, 'autoembed' ), 8 );
</pre>