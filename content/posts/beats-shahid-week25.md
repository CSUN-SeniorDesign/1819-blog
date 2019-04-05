---
title: "Shahid's Blog. Week 25."
date: 2018-4-5T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 25"]
layout: 'posts'
---

## Shahid's Blog. Week 25
#### What's new this week?
This week I worked on removing our dependency on Cloudflare for getting HTTPS working on our website.

#### Setting up HTTPS with a NodeJS application
1. Install ExpressJS
```
npm i express --save
```

2. Set the app to use express.
```
var express = require('express');
var app = express();
var fs = require('fs');
```

3. Create the certificate and move these files to a directory named encryption
```
mkdir encryption && cd encryption && openssl req -new -newkey rsa:2048 -nodes -out mydomain.csr -keyout private.key
```

4. Import these files into the application.
```
  var key = fs.readFileSync('encryption/private.key');
  var cert = fs.readFileSync( 'encryption/primary.crt' );
  var ca = fs.readFileSync( 'encryption/intermediate.crt' );

  var options = {
  key: key,
  cert: cert,
  ca: ca
  };
```

5. Make the application listen to port 443.
```
var https = require('https');
https.createServer(options, app).listen(443);
```

6. Redirect HTTP to HTTPS
```
  var http = require("http");
  http.createServer(app).listen(80);

  app.use(function(req, res, next) {
    if (req.secure) {
      next();
    } else {
    res.redirect('https://' + req.headers.host + req.url);
    }
  });
```

7. Run the application
```
node app.js
```
