---
layout: default
---


<div class="float_wrapper">
	<div class="post-list">
		<h2>Recent posts:</h2>
		<ul>
		  {% for post in site.posts %}
		  <li class="box">
		  	<span>{{post.date | date:"%Y-%m-%d"}}</span>
		    <a href="{{post.url}}">{{post.title}}</a>
		    
		    
		  </li>
		  {% endfor %}
		</ul>
	</div>
	
	<div class="side">
			<h2>Links:</h2>
			  <ul>
			  	<li class="box">
			  		<a href="{% link aboutme.md %}">About Me</a>
			  	</li>
				</ul>
		
	</div>
	
	<div class="bot"></div> 

</div>

