---
#
# By default, content added below the "---" mark will appear in the home page
# between the top bar and the list of recent posts.
# To change the home page layout, edit the _layouts/home.html file.
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: default
---
<script>
var url = window.location.href;

var wp = /\?(p|cat)=[0-9]{2,3}/;

if (wp.test(url)) {
	url = url.replace("?cat=11", "");
	url = url.replace("?cat=12", "#pubs");
	
	url = url.replace("?p=207", "2011/10/24/revisiting-asvos");
	url = url.replace("?p=301", "#hdfusion");
	url = url.replace("?p=362", "#asvo");
	url = url.replace("?p=372", "#asvo");

	url = url.replace(wp, "");

	window.location.href = url;
}
</script>
