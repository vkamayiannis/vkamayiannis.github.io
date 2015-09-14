---
layout: page
title: Get in touch
permalink: /contact/
---

<form action="//formspree.io/kamayiannis@yahoo.com" method="POST">
    <input type="hidden" name="_next" value="{{ site.baseurl }}/thanks"/>
    <div>
      <input type="text" name="name" placeholder = "Your name">
    </div>
    <div>
      <input type="email" name="_replyto" placeholder = "Your email">
    </div>
    <div>
      <textarea name="body" rows="10" cols="50" placeholder = "Your message"></textarea>
    </div>
    <div>
      <input type="submit" value="Send">
    </div>
</form>