---
date: 2017-10-26T00:00:00+00:00
title: "Publish to FTP(S)"
author: "christophschlosser"
tags: [ "publish", "ftp", "ftps" ]
repo: "christophschlosser/drone-ftps"
logo: "ftps.svg"
image: "cschlosser/drone-ftps"
---

The FTPS plugin can be used to publish artifiacts over FTP(S). The below pipeline configuration demonstrates simple usage:

```yaml
pipeline:
  deploy:
    image: cschlosser/drone-ftps
    hostname: example.com:21
    secrets: [ ftp_username, ftp_password ]
```

The default configuration is SSL encryption and strong SSL validation (FTPS).
Some FTP server have SSL encryption, but may be misconfigured and transfer will fail due to the certificate validation.
In this case the validation may be skipped:


```diff
pipeline:
  deploy:
    image: cschlosser/drone-ftps
    hostname: example.com:21
    secrets: [ ftp_username, ftp_password ]
+   verify: false
```

The SSL encryption may be disabled, if the FTP server does not support SSL:

```diff
pipeline:
  deploy:
    image: cschlosser/drone-ftps
    hostname: example.com:21
    secrets: [ ftp_username, ftp_password ]
+   secure: false
```

Example configuration using dest_dir to specify where to put the files on the remote server:

```diff
pipeline:
  deploy:
    image: cschlosser/drone-ftps
    hostname: example.com:21
    secrets: [ ftp_username, ftp_password ]
    secure: true
+   dest_dir: /var/www/mysite
```

Example configuration using src_dir to upload only the static site generated:

```diff
pipeline:
  deploy:
    image: cschlosser/drone-ftps
    hostname: example.com:21
    secrets: [ ftp_username, ftp_password ]
    secure: true
    dest_dir: /var/www/mysite
+   src_dir: /mysite/static
```

Example configuration using exclude to prevent dotfiles from getting uploaded:

```diff
pipeline:
  deploy:
    image: cschlosser/drone-ftps
    hostname: example.com:21
    secrets: [ ftp_username, ftp_password ]
    secure: true
    dest_dir: /var/www/mysite
    src_dir: /mysite/static
+   exclude:
+     - ^\.git/$
+     - ^\.gitignore$
+     - ^\.drone.yml$
```

Example configuration using include to only upload HTML, CSS and JS files:

```diff
pipeline:
  deploy:
    image: cschlosser/drone-ftps
    hostname: example.com:21
    secrets: [ ftp_username, ftp_password ]
    secure: true
    dest_dir: /var/www/mysite
    src_dir: /mysite/stati
    exclude:
      - ^\.git/$
      - ^\.gitignore$
      - ^\.drone.yml$
+   include:
+     - ^*.css$
+     - ^*.js$
+     - ^*.html$
```

# Secret Reference

FTP_PASSWORD
: password used to login to the FTP server with the specified user

FTP_USERNAME
: Username used to login to the FTP server

# Parameter Reference

hostname
: FTP host including the port

verify
: if set to true the SSL certificate validation is enforced, otherwise no validation (default true)

secure
: if set to true FTPS is enforced, otherwise plain FTP is used (default true)

dest_dir
: where to put the files on the remote server (default /)

src_dir
: which local directory should be used for the upload (default ./)

exclude
: egrep like pattern matching to exclude files from uploading (default none)

include
: egrep like pattern matching to include files to upload (default none)
