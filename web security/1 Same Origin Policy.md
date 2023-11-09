# Same origin
Two links are said to have same origin if
- their protocols are same
- the host is same
- the port is same
The path doesn't matter.
When the root HTML document of web application is loaded from a certain URL, the origin of that web application is said to be a combination of the scheme, hostname and port-number of that URL.
For instance, a web application loaded from https://www.example.com has scheme https, hostname www.example.com and, in this case implicit, port number 443. The origin for this web application is thus (https, www.example.com, 443).
# Same origin policy
In short, the same-origin policy enforces that documents that interact with each other have the same _origin_.
# Example
One such restriction is that scrips executing on `http://example.com` are not allowed to access resources on `http://subdomain.example.com`
(CORS is ignored so far.)
Its purpose is to isolate browser windows (and tabs) from each other so that, for example, when you go to example.com, the website will not be able to read your emails from gmail.com, which you may have open in another tab.

These URIs are considered to be of the _same_ origin:

- `https://www.appsecmonkey.com/`
- `https://www.appsecmonkey.com/blog/same-origin-policy/`
- `https://www.appsecmonkey.com:443/blog/same-origin-policy/`

And these are all of _different_ origins:

- `http://www.appsecmonkey.com/`
- `https://appsecmonkey.org/`
- `https://www.appsecmonkey.com:8080/`
# What is allowed by the same-origin policy, and what is not?
In general, writing is allowed, and reading is denied. 
# With CORS
The same-origin policy (SOP) dictates that any code executing inside this origin only has access to resources from that same origin, unless explicitly allowed otherwise by e.g. a Cross-Origin Resource Sharing (CORS) policy.
In the previous example, the web application from https://www.example.com:443 cannot retrieve the address book from a webmail application with different origin https://mail.example.com:443 running  in the same browser, unless the latter(mail) explicitly allows it.
# Web Security Consequence
Consider what would happen if the webmail application is written insecurely, so that an attacker can execute javascript in its origin: https://webmail.com:443. Because the attacker's code runs inside the same origin as the webmail application, it has access to the same resources and can also read and retrieve emails and contact information. Because of the power of Javascript, an attacker can do much more.
# CSRF
Let's suppose someone manages to trick you into visiting [https://faceboook.com](https://faceboook.com/) (extra "o"). On this website, there is an iframe which loads the content of the correct site [https://facebook.com](https://facebook.com/). As usual, you login into your Facebook account. Now, the malicious website (faceboook.com) can read your private messages or perform actions on your behalf with just a few Asynchronous JavaScript and XML (AJAX) requests. One such attack is known as Cross-Site Request Forgery (CSRF).
