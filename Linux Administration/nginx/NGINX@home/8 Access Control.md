# White Listing
Way to have strict access control on your files on your webserver is with the help of whitelisting.
![6dc864ceefbce6f896eee97c7d6813ad.png](../_resources/6dc864ceefbce6f896eee97c7d6813ad.png)
Let's assume this is an example.com web server and you've /admin portal. Now, you don't want everyone to access the /admin page.

You might only want to allow only certain IP addresses to access that particular page. The way with which you can do this is called whitelisting.

There are 3 clients over here with unique IP addresses. So, as a web server, what you can do is that you can allow access to a certain resource only if the client has particular IP addresses.  i.e you say only the client with IP address `10.0.5.10` can access the /admin page of the web server and everyone else is denied.

So, only 1 particular computer can access the particular admin panel and no one else.

Syntax

```cmd
location /admin {
    allow 172.18.10.5;
    deny all;
}
```
`allow` directive is saying to allow this IP address, deny directive is saying to deny everyone else.

```cmd
rev-proxy.conf
server{
    server_name mywebsitename;
    location /{
        root /usr/share/nginx/html;
    }
    location /admin{
        root /usr/share/nginx/html;
        index index.html;
        allow 192.168.1.75;
        deny all;
    }
}
```
Now, to do the test, you need this folder `/usr/share/nginx/html/admin/index.html`.
From the host Ip address, if I do
`curl -I 192.168.1.75/admin`, I'll get `200 OK`. Meanwhile, if I curl from any other IP address, I will get `403 Forbidden`.

Now, say you want to allow lots of IP addresses, one straightforward way to do this is as follows.
```cmd
rev-proxy.conf
server{
    server_name mywebsitename;
    location /{
        root /usr/share/nginx/html;
    }
    location /admin{
        root /usr/share/nginx/html;
        index index.html;
        allow 192.168.1.75;
		allow 192.168.1.76;
        deny all;
    }
}
```
This isn't an elegant solution. A rather elegant approach is to create a new file named `whitelist.conf` and include it in `rev-proxy.conf`.
```cmd
rev-proxy.conf
server{
    server_name mywebsitename;
    location /{
        root /usr/share/nginx/html;
    }
    location /admin{
        root /usr/share/nginx/html;
        index index.html;
        include /etc/nginx/conf.d/whitelist.conf;
        deny all;
    }
}
```

```cmd
whitelist.conf
allow 192.168.1.74;
allow 192.168.1.75;
```
# limit_connection module
This is useful for servers which are providing download related content.
So, if you've a server with lots of files which people would be downloading, then `limit_conn` module will be a very useful tool for you.
Say the bandwidth of your web server is 100Mbps and you expect 10 users at a time to download the file. Assume a scenario where out of the 10 users, 2 of them have 50Mbps connection and the rest 8 of them have 2Mbps connection. 

Problem here is that 2 users with 50Mbps connection will occupy 100Mbps bandwidth of the server and thus choke the server.
So, rest 8 people when they try to download the file, either the download will be slow or the server will return 503.

So, during these types of scenarios, it's very important to limit the amount of connection speed that you give to each individual user.

So, a better approach here would be that you give 10Mbps of bandwidth to each user, then essentially all the 10 users will be able to download the file.

But if you don't have any restrictions on the amount of download speed a user can have, then typically the people who will have very fast download speed will choke up the whole server.

## Lab
Download a 100MB test file from this site.
https://speed.hetzner.de/
Upload this to your server.
Go to the configuration
```cmd
rev-proxy.conf
server{
    listen 80;
    location /{
        root /usr/share/nginx/html;
        index index.html;
    }
    location /downloads{
        root /usr/share/nginx/html;
    }
}
```
Say its IP Address is `192.168.1.75`
Now, go to another server and
`wget 192.168.1.75/downloads/100MB.bin`
It'll be downloaded in fulll speed of your host network.
![7526df2d434f9f2dfee0a85f5724424a.png](../_resources/7526df2d434f9f2dfee0a85f5724424a.png)
Now, let us introduce `limit_conn` module and put up a restriction.
```cmd
rev-proxy.conf
server{
    listen 80;
    location /{
        root /usr/share/nginx/html;
        index index.html;
    }
    location /downloads{
        root /usr/share/nginx/html;
        limit_rate 50k;
    }
}
```
![cc254a18d518c3599990e2f7fed212bb.png](../_resources/cc254a18d518c3599990e2f7fed212bb.png)
It'll be downloaded with around 50KBps speed.

Again, consider a scenario where the same file will be downloaded multiple times in the same server with the current configurations.
Here's the speed in instance 1 of the server.
![606d29a5d9c31ada2e293291899de524.png](../_resources/606d29a5d9c31ada2e293291899de524.png)
Here's the speed in instance 2 of the server.
![0ef5db86234301c3f4e936fed7c7a18d.png](../_resources/0ef5db86234301c3f4e936fed7c7a18d.png)
So, apparently when you combine both, they're hogging 100KBps which you probably don't want. So, this is not ideal. So, we've to limit the amount of speed that we give to each and every individual connections. We can also do that with nginx.

In order to do that, we've to define a zone similar to the zone that we've defined earlier for shared memory and buffer.

```cmd
limit_conn_zone $binary_remote_addr zone=addr:10m;
server{
    listen 80;
    location /{
        root /usr/share/nginx/html;
        index index.html;
    }
    location /downloads{
        root /usr/share/nginx/html;
        limit_rate 50k;
        limit_conn addr 1;
    }
}
```
This means that binary remote addr (alternative to remote addr). Basically, this means that the IP address of client who will be downloading the file.  IDK wtf it means.
So, if from same IP address, if you get two download request, the latter will get 503 Service Temporarily Unavailable.
![97e904aba704e59ec2d3c0f578d5b467.png](../_resources/97e904aba704e59ec2d3c0f578d5b467.png)
![140033cc7569062a435eb0ba71c28cba.png](../_resources/140033cc7569062a435eb0ba71c28cba.png)
This is how websites that have premium subscription for downloads use this nginx configuration.
```cmd
limit_conn_zone $binary_remote_addr zone=addr:10m;
server{
    listen 80;
    location /{
        root /usr/share/nginx/html;
        index index.html;
    }
    location /downloads{
        root /usr/share/nginx/html;
        limit_rate 50k;
        limit_conn addr 1;
        limit_rate_after 50m,
    }
}
```
When you add `limit_rate_after 50m`, it means till 50MB there is no limit in downloading, but once 50MB file is downloaded, then there is limit in downloading as per the configuration.
# Basic Authentication
![6b9246f435cf5225fa22f40e1e7221f7.png](../_resources/6b9246f435cf5225fa22f40e1e7221f7.png)

>The realm is used to describe the protected area or to indicate the scope of protection. This could be a message like "Access to the staging site" or similar, so that the user knows to which space they are trying to get access to.

Source: https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication

Remember that base64 can be decoded, so always use basic authentication on the top of ssl.
# Basic Authentication Practical
```cmd
rev-proxy.conf
server{
    server_name mywebsitename;
    location /{
        root /usr/share/nginx/html;
        index index.html;
    }
    location /admin{
        root /usr/share/nginx/html;
        index index.html;
        auth_basic "Basic Authentication";
        auth_basic_user_file "/etc/nginx/.htpasswd";
    }
}
```

```cmd
htpaswd -c /etc/nginx/.htpasswd admin
type password twice
cat .htpasswd
```
Go to browser and open the page `ip_address/admin`. Then credentials will be asked. 

# Understanding hashing
Hash changes entirely even if 1 extra character is added or deleted.
```cmd
# cat nepal
nepal
# cat nepalaya
nepalaya
# md5sum nepal
31ed3e3527b9098f9138b33709b24c13  nepal
# md5sum nepalaya
1bf0bf424b20ca04e1a4cb953f2cba43  nepalaya
```
Hashing algorithm will tell us if the file is modified or not.
Viruses attach to the file and change the structure of the file.
So, it's through the help of hashing, it's possible for us to know whether a file is modified or not.

In production environment, generally files would not be modified. So, in production environment for all the important files, there is a program which monitors whether the file is changed or not. And if the file is changed, it will immediately email to the system administrator saying that this file is modified.

## Difference between encryption and hashing?
Encryption is two way function whereas hashing is one way function. You can't recover the original text after it is hashed. But you can recover the original text, after it's encrypted via decryption.

## Benefit of hashing?
- Your system files do not change. If it has changed, then there is a high probability that a virus has done it.
- You can store passwords in hashed value instead of encrypted value.
- Verify the integrity of the software that you download.
![7cdb5295f84179edabff98d0dc63ec53.png](../_resources/7cdb5295f84179edabff98d0dc63ec53.png)
Linux stores the hashes.
![b4d5d8641651c0a0d50da298fbe61328.png](../_resources/b4d5d8641651c0a0d50da298fbe61328.png)
Say you download ms-office from server to computer. File comes via  network to your computer. Say the server is in USA and client is in India. There is a huge communication channel. So, the file could be tampered by hacker in the journey of the packet from USA to India. So, we use MD5 in this case. 
Once you download ms-office from server to your laptop, now your job is to take a hash value of this and compare it with hash value that microsoft has provided.

Different hash value could mean two things
- File is corrupted. It was not downloaded correctly.
- Integrity has been changed in the network by a hacker.
# Digest Authentication
![b8bb289ea03e22d47918ffcf03cb9a2d.png](../_resources/b8bb289ea03e22d47918ffcf03cb9a2d.png)
One of the reasons to provide digest authentication  was to provide an advantage over the basic authentication.
I quote this from HTTP Developer's Handbook By Chris Shiflett
> Digest Authentication mitigates the risk of exposing the username and password by utilizing a one-way cryptographic algorithm(also commonly called a hash or a message digest). These algorithms are called one-way algorithms because they are practically impossible to reverse. Although this might seem like a bold claim, consider that MD5 (Message Digest 5, a popular one-way algorithm) always returns a 128-bit digest. Thus, if you were to create a message digest of the text of an entire book, it would be 128 bits in length. If it were possible to generate the text of this entire book from 128-bit message digest, MD5 would be an amazing compression algorithm.

> Note that the data can still be hacked if common username, password combination is being used like admin 1234 etc.

> The basic series of events for digest authentication is nearly identical to that of basic authentication and is illustrated below.

![c75d9fb7350b78dd556cf2f205d05424.png](../_resources/c75d9fb7350b78dd556cf2f205d05424.png)

> - The client makes the initial request for the protected resource as normal, including no authentication information, because it is initially unaware that the resource requires authentication.
> - The authentication begins with the server's 401 Unauthorized response, in which it includes the www-authenticate response header. This header explains the type of authentication required as well as several other details.
> - The client's second request includes the Authorization header, which includes the message digest as well as some additional information about the authentication.
> - Finally, the resource is returned in the second HTTP response. This response also includes the Authentication-Info header, which completes the mutual authentication.

More information
![6b91eda70eb7cb44528cbaff1e944fd0.png](../_resources/6b91eda70eb7cb44528cbaff1e944fd0.png)

![98aa6252a890821ccefdda9681efc364.png](../_resources/98aa6252a890821ccefdda9681efc364.png)

![37a4a3953a302e00044fb1f245af2206.png](../_resources/37a4a3953a302e00044fb1f245af2206.png)

![6559966679885c665cd168e667c8b111.png](../_resources/6559966679885c665cd168e667c8b111.png)
![5a97ccc70fbff6a445d4d14e2affddc7.png](../_resources/5a97ccc70fbff6a445d4d14e2affddc7.png)
![5b7aa324e6462ef4dee3bb0e5e405258.png](../_resources/5b7aa324e6462ef4dee3bb0e5e405258.png)
![fac4c7b3b9c2f7eae4e0e6e0370446e5.png](../_resources/fac4c7b3b9c2f7eae4e0e6e0370446e5.png)
![575561d5d840df3d1ba1023de184f570.png](../_resources/575561d5d840df3d1ba1023de184f570.png)
![4170d6ce4d0c4331c93f302ef53d01a2.png](../_resources/4170d6ce4d0c4331c93f302ef53d01a2.png)
![ae9492881c91b3e85e34d361f1e7b764.png](../_resources/ae9492881c91b3e85e34d361f1e7b764.png)
![a5b0b70e51b264a01a2a850c396fd92a.png](../_resources/a5b0b70e51b264a01a2a850c396fd92a.png)
![960a5439404a319099721d40b31921f7.png](../_resources/960a5439404a319099721d40b31921f7.png)
![6954fbf94f0bb5b41af4be57c33f1f09.png](../_resources/6954fbf94f0bb5b41af4be57c33f1f09.png)
![0dd50dab87190dae18024530d3203239.png](../_resources/0dd50dab87190dae18024530d3203239.png)
![c2f76871773ccfc9f8ac6223a744b311.png](../_resources/c2f76871773ccfc9f8ac6223a744b311.png)
![9dcd47995e80e097948f385d0807df29.png](../_resources/9dcd47995e80e097948f385d0807df29.png)
![d05ce8edf0ba366d7d11d975389dd952.png](../_resources/d05ce8edf0ba366d7d11d975389dd952.png)
![42c3859ad6983dfb8fb80e1e24c7c823.png](../_resources/42c3859ad6983dfb8fb80e1e24c7c823.png)
![5b63ecdda19c90dc8f6e255c6d22bf7e.png](../_resources/5b63ecdda19c90dc8f6e255c6d22bf7e.png)
![36aab7ca0ede15a2872be740ab7667ea.png](../_resources/36aab7ca0ede15a2872be740ab7667ea.png)
![d0867df52d0f3fa7499048406e5cc0a3.png](../_resources/d0867df52d0f3fa7499048406e5cc0a3.png)
![1248dfba0db919c4e34cdbc6d5263bf4.png](../_resources/1248dfba0db919c4e34cdbc6d5263bf4.png)
Instead of base64, it sends MD5 value.
How has value is calculated is shown in above figure.
## Nonce Value
It is used to prevent replay attacks. So, let's say a hacker captures this response and after 1 hour, he decides to send the same response back to the server.
Now, as the username, password and realms are correct, ideally the server should be able to verify it. So, this is one of the flaws and the reason why nonce value is added. In any programmed systems, we can take current datetime (or epochtime) and use it as a nonce. Thus this nonce value will keep changing on every interval.
So, even if a hacker captures this particular request and he replays it after half an hour, he won't be able to get authenticated because the nonce value has changed. And because the nonce value has changed, the whole MD5 value has also changed.
This is one the ways in which digest authentication works.
Nginx plus doesn't support the http digest authentication.

# GeoIP module
Say some websites are specific to clients in India, so there is no need to open up the website for the whole world. So, block website for specific countries.
`ngx_http_geoip_module` is used for it. This allows you to block website on certain countries.
May be you could use firewall to block some IP addresses. But it's not convenient to do that in firewall. Hence, you've to use GeoIP module which nginx provides which can block certain countries.
geoip is available only in nginx plus. So,

![nginxplus.png](../_resources/nginxplus.png)

