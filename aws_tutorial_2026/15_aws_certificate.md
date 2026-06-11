There are **3 common places where SSL certificates can be installed**.

Understanding these is very important for AWS interviews.

# 1. Certificate on Load Balancer (Most Common AWS Production Setup)

```text id="5f72ju"
User
 HTTPS
   ↓
ALB
(Certificate Here)
   ↓ HTTP
EC2
   ↓
Node.js
```

Certificate location:

```text id="o1tslp"
ACM
 ↓
Attached to ALB
```

Flow:

```text id="z7iyx4"
Browser
 ↓ HTTPS
ALB
 ↓ HTTP
EC2
```

This is called:

```text id="mp1x2o"
SSL Termination
```

### Advantages

* Easy certificate management
* ACM auto-renews certificates
* One certificate for many EC2s
* Most common AWS architecture

### Interview Answer

> In AWS production environments, SSL certificates are typically attached to the Application Load Balancer using ACM.

---

# 2. Certificate on Nginx (EC2)

```text id="x1nrlr"
User
 HTTPS
   ↓
Nginx
(Certificate Here)
   ↓
Node.js
```

EC2 contains:

```text id="owp8r6"
/etc/nginx/ssl/cert.pem
/etc/nginx/ssl/private.key
```

Nginx config:

```nginx
server {
    listen 443 ssl;

    ssl_certificate cert.pem;
    ssl_certificate_key private.key;
}
```

Flow:

```text id="9u6u5q"
Browser
 ↓ HTTPS
Nginx
 ↓ HTTP
Node.js
```

### Advantages

* Works without Load Balancer
* Common on small projects

### Disadvantages

* Must renew certificates yourself (unless automated)
* Every EC2 needs certificate configuration

---

# 3. Certificate Directly in Node.js Application

```text id="g3oh7q"
User
 HTTPS
   ↓
Node.js
(Certificate Here)
```

Example:

```javascript
const https = require("https");

https.createServer({
  key: fs.readFileSync("private.key"),
  cert: fs.readFileSync("cert.pem")
}, app).listen(443);
```

Flow:

```text id="m8j6gu"
Browser
 ↓ HTTPS
Node.js
```

### Advantages

* Simple learning setup

### Disadvantages

* Rare in production
* Harder to manage
* Every application handles SSL itself

---

# Production Evolution

### Beginner

```text id="n9j7tz"
Internet
 ↓
Node.js
```

Certificate in Node.js

---

### Small Server

```text id="r3qv5f"
Internet
 ↓
Nginx
 ↓
Node.js
```

Certificate in Nginx

---

### Enterprise AWS

```text id="pnl2k3"
Internet
 ↓
ALB
 ↓
Private EC2
 ↓
Nginx/Node.js
```

Certificate in ALB via ACM

---

# What Happens with ACM?

ACM certificates **cannot be directly installed on EC2 or Node.js**.

ACM certificates are designed to be attached to AWS-managed services such as:

* Application Load Balancer
* Network Load Balancer
* CloudFront
* API Gateway

So when people say:

```text id="muv4mn"
AWS Certificate Manager Certificate
```

they usually mean:

```text id="rqv6c9"
Certificate stored in ACM
       ↓
Attached to ALB
```

not:

```text id="n95i2w"
Certificate copied into EC2
```

---

# Never Forget Diagram

```text id="jv47e4"
OPTION 1 (Recommended)

User
 ↓ HTTPS
ALB (Certificate)
 ↓ HTTP
EC2


OPTION 2

User
 ↓ HTTPS
Nginx (Certificate)
 ↓ HTTP
Node.js


OPTION 3

User
 ↓ HTTPS
Node.js (Certificate)
```

For AWS interviews, if someone asks:

> "Where should SSL certificate be installed in a scalable AWS architecture?"

Answer:

> On the Load Balancer using ACM, with SSL termination at the Load Balancer and application servers running behind it.
