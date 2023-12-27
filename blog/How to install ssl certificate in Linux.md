
# First understand the certificate provider.
## digicert certificate
`cat  domain_name_certificate.crt CA_Bundle.crt > crt2023.crt`
domain_name_certificate.crt is the main certificate.
CA_Bundle.crt is combination of intermediate+CA root.
## sectigo certificate order
ssl certificate 
domain_name 
sectigo
usertrust
AAA

# after concatenating
- Copy and paste the certificates in /etc/nginx/conf.d/vhosts/ssl (The location of earlier ssl certificate) To find `grep ssl_certificate *.conf`
- Take a backup of earlier certificate.
Test the chain is successful or not.


```
$ openssl x509 -noout -modulus -in certificate.crt | openssl md5

Print the md5 hash of the Private Key modulus:
$ openssl rsa -noout -modulus -in private.key | openssl md5

the stdin output should match
```
# Reload nginx
`systemctl reload nginx`

# Test if certificate is reflected(sometimes blocked by Web Application Firewall in Nginx)
```
curl domain_name_without_http -vI
```

# With certbot
## To install
`yum -y install certbot-nginx`
## For certificate
`certbot --nginx -d domain_name`
## For renewal
`certbot renew`

## To check if certbot is active
```
systemctl status certbot-nginx
```
## To restart nginx if there is no systemctl nginx service
```
/usr/sbin/nginx -s reload
```
