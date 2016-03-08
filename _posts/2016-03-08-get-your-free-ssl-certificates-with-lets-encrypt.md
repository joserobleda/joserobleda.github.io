---
layout: post
title: "Get your free SSL certificates with Let's Encrypt."
excerpt: 
comments: true
tags: [web, tech, ssl, crypt]
categories: aws
image:
  feature: feature-maths.jpeg
  credit: Roman Mager
  creditlink: https://unsplash.com/roman_lazygeek
date: 2016-03-08T08:05:00+01:00
---

We've been paying for the ssl certs for years, but not any more. [**Let's Encrypt**](https://letsencrypt.org/) is here, and we can get our certificates for free. 

Let's see how it works. 

### Understanding Let's Encrypt

Let's Encrypt allows you to issue signed certificates for your domains. It handles the authorization by its client, which should be installed one of your servers. Your domains **must point to that server** in order to be authorized.


### Install

**Mac users warn**: You better use a Vagrant with some debian distribution, it will make your day a lot of easier.

You can follow the official [getting starting guide](https://letsencrypt.org/getting-started/), but it is as simple as run:

```
 git clone https://github.com/letsencrypt/letsencrypt
````

### Generate your certs

After moving to the newly created `letsencrypt` directory we can just run `./letsencrypt-auto --help` to see if everything works fine.

As we said before, Let's encrypt needs to validate your ownership of the domains you will validate. It can do it by itself or by using your current webserver. 

If you have your web located a `/var/www` and you want a certificate for `cool-domain.com` you can run the following command:

````
./letsencrypt-auto certonly --webroot -w /var/www -d cool-domain.com
```` 

**And that's it!**  
You can now access your certificate files under the directory `/etc/letsencrypt/live/cool-domain.com/`


### Tips

- Note that your certificates will expire in about 90 days
- You're limited to issue about 5 certificates per week
- You can issue test certificates if you want to test adding the `--test-cert` flag
- If you are under a non-debian machine you can use the `--debug` flag to run in experimental way


<span style="font-size:12px">
PD: We can also get our free certs with [AWS Certificate Manager](https://console.aws.amazon.com/acm), but it only works inside AWS services, and it is not cross-region. Do you know more services like this? Leave me a comment!
</span>
