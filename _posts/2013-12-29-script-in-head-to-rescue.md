---
layout: post
title: script in head to the rescue
description: "Migrating from Blogger to Github Pages."
tags: [Tech Zone, Blogger, Github, Jekyll]
comments: true
share: true
---

I finally finished migrating from my old blog hosted on blogspot to my own domain and this blog hosted on Github. And while I haven't been blogging much (my last post is few days short from exactly a year ago), I didn't want to lose whatever little exposure I had garnered.

First step was copying my posts (and changing them to markdown), which took some manual work but went fairly well. The tricky part though, was redirecting visitors to my new blog.

This is normally done with a 301 Moved Permanently, but unfortunately (and logically) I don't have access to do that on blogger. So a Google search told me to use `<meta http-equiv="refresh" content="0;URL=http://mydomain.com" >`. That did what it was supposed to do (a redirect), but by the time it redirected the page had already (almost) completely loaded, so it was a jarring experience, not to mention that a user faced with it for the first time might think something was broken and/or fishy.

Of course, a javascript redirect was always possible using 

	<script>
		window.location.replace('http://mydomain.com');
	</script>

but I had already dismissed it because it also didn't fire until page elements had already loaded. 

And then it hit me.

As a web developer, you're sworn an oath to never use a script tag in the head of the document because it blocks rendering (the browser stops rendering the page while it's dealing with javascript), making the user wait before they see the page's content. So what if I put my redirection script directly after the opening head tag?

	<!DOCTYPE HTML>

		<html>
			<head>
				<script>
					window.location.replace('http://mydomain.com');
				</script>
				<!-- rest of the page -->

Well that worked perfectly. I added some code to redirect to the correct page and not just the new blog's homepage, and it was easy since I kept the same slug. My old url's ended with '.html', that's why I sliced at -5.

	<script>
		var href = window.location.pathname;
	    window.location.replace(&#39;http://blog.acodingbrain.com&#39;+href.slice(href.lastIndexOf(&#39;/&#39;),-5));
	</script>


