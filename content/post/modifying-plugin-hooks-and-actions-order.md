+++
title = "Modification of plugin's hooks and actions regardless of load order."
date = "2014-08-14"
author = "Principal"
description = ""
tags = ["Wordpress"]
categories = ["Development"]
+++

To have access to all other plugins hooks, we need run our script when wordpress done loading plugin.
Use following workaround.

Place your hooks in to the function, and call it when plugins_loaded.
<pre>add_action('plugins_loaded','modify_actions_custom');

function modify_actions_custom(){

}
</pre>
Reference:
<a href="https://codex.wordpress.org/Plugin_API/Action_Reference" target="_blank">Wordpress Codex with loading structure.</a>