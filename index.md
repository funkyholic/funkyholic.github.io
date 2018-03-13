---
layout: default
---



<div class="post-list">
	<h1>Recent posts:</h1>
	<ul>
	  {% for post in site.posts %}
	  <li class="box">
	  	<span>{{post.date | date:"%Y-%m-%d"}}</span>
	    <a href="{{post.url}}">{{post.title}}</a>
	    
	    
	  </li>
	  {% endfor %}
	</ul>
	
	<div class="info">
	
	</div>
	
</div>

<div class="bot"></div> 