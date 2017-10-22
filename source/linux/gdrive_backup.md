---
title: "用Google Drive备份Vps数据"
layout: page
date: 2017-08-26 21:40:12
---

[TOC]

Recently, I have a plan to order a new vps service, but some data need migration and backup to google drive. So I found a tool [gdrive](https://github.com/prasmussen/gdrive), Let's begin!

## Install GDrive Client ##

This tool can be used across multiple platform, Linux, Raspberry pi, Windows...

```bash
wget -O https://docs.google.com/uc?id=0B3X9GlR6EmbnQ0FtZmJJUXEyRTA&export=download
```

**PS: Find a client from [here](https://github.com/prasmussen/gdrive#downloads), I used CentOS x64, select [gdrive-linux-x64 v2.1.0](https://docs.google.com/uc?id=0B3X9GlR6EmbnQ0FtZmJJUXEyRTA&export=download)**

Then, executable cli script

```bash
chmod +x gdrive
```

And move scipt `mv gdrive /usr/local/bin/`

## Access Google Token ##

Run command `gdrive about`, If this is your first run gdrive command, it prompt you to link your google drive account and acess token, so copy link to any browser(Chrome) login to your account. It will return a string like `Enter verification code: 4/9hsRWERSTsds17gGHS12dpoSJhhslkNMsnkSk41se1gxfB`, this token will store `~/.gdrive/`.

## Backup Data ##

Totally basic on your bussiness, if you want backup database data, or your website source file or some environment configurations, all of these can be write a script to do this work.

_Below, it's a sample:_

### Backup Docker Data ###

Create a bash script `backup-docker.sh`:

```bash
#!/bin/bash

cd /var/docker/compose

# stop the containers
docker-compose stop

# if you are not install p7zip, you can first run yum install p7zip, whether you can use tar command to compress
# zip them up encrypted (yes, the password has to be after the -p switch with no space)
yum install p7zip
7za a -t7z -mx2 -pPassword1234!@#$ /tmp/docker-backup-$(date '+%Y-%m-%d').7z /var/docker/

# start the containers
docker-compose start

# upload to google drive, if you upload to parent folder must add [-p | --parent] id set
gdrive upload -f /tmp/docker-backup-$(date '+%Y-%m-%d').7z -p 09KVkjJK83exTfkQ3cEJKFDSkjvadkdghJKDGKnRGJKVD932jkaknvKDVR28 --delete /tmp/docker-backup-$(date '+%Y-%m-%d').7z

exit 0
```

If you use `tar` command, other part is same:

```bash

# tar the dokuwiki directory
tar -zcf "docker-backup-$(date '+%Y-%m-%d').tar.gz" /var/docker/
# encrypt the tar
openssl aes-256-cbc -a -salt -in "docker-backup-$(date '+%Y-%m-%d').tar.gz" -out "docker-backup-$(date '+%Y-%m-%d').tar.gz.enc" -pass 'pass:Password1234!@#$'

```

Then, create a `crontab` job:

```bash
chmod +x backup-docker.sh
echo "0 1 * * * root ~/backup-docker.sh" >> /etc/crontab

```

### Backup Database Data ##

See the dump sql script(eg. use mysqldump):

```bash
# dump all databases
mysqldump --all-databases > /tmp/dump.sql

# dump only specify databases
mysqldump --databases blog wordpress q2a > /tmp/dump.sql

# dump specify tables from database
mysqldump blog bg_user bg_post bg_comment > /tmp/dump.sql
```

Then, compress dump file and encrypt file:

```bash
tar -zcf /tmp/mysql-backup-$(date '+%Y-%m-%d').tar.gz /tmp/dump.sql
openssl aes-256-cbc -a -salt -in /tmp/mysql-backup-$(date '+%Y-%m-%d').tar.gz -out /tmp/mysql-backup-$(date '+%Y-%m-%d').tar.gz.enc -pass 'pass:Password1234!@#$'
```

In the end, run gdrive upload:

```bash
gdrive upload -f /tmp/mysql-backup-$(date '+%Y-%m-%d').tar.gz.enc -p 09KVkjJK83exTfkQ3cEJKFDSkjvadkdghJKDGKnRGJKVD932jkaknvKDVR28 --delete /tmp/mysql-backup-$(date '+%Y-%m-%d').tar.gz
```

## Upload File ##

From the sample, you know how to use upload command, It's very simple, the detail command parameter see [here](#gdrive-command)

```bash
gdrive upload -f /var/www/html/*
```

## Download File ##

First, run `gdrive list`, it will show GDrive file list:

```bash
Id                              Title                              Size      Created
0B-Jkp8Bk3zNcTkQxeGxZaFRiMzA    vps-backup-2017-08-26.tar.gz.enc   55.3 MB   2017-08-26 10:36:15
0B-Jkp8Bk3zNcMXJBQ0tUdk5XOVU    tools                              0.0 B     2017-08-26 09:21:40
0B-Jkp8Bk3zNcQnozOWV5OFRyNWs    temp                               0.0 B     2017-08-26 09:21:25
0B-Jkp8Bk3zNcc3RhcnRlcl9maWxl   Getting started                    1.6 MB    2016-03-30 01:48:28
```
And then, select your file `Id` and run download command, thats it!

```bash
gdrive download -i 0B-Jkp8Bk3zNcc3RhcnRlcl9maWxl
```

## Share File ##

It's simliar with download command `gdrive share -i 0B-Jkp8Bk3zNcc3RhcnRlcl9maWxl`

## GDrive Commands ##

```bash
Usage: gdrive [global options] <verb> [verb options]

Global options:
        -a, --advanced     Advanced Mode -- lets you specify your own oauth client id and secret on setup
        -c, --config       Set application path where config and token is stored. Defaults to ~/.gdrive
        -v, --version      Print version
        -h, --help         Show this help

Verbs:
    delete:
        -i, --id           File Id (*)
    download:
        -i, --id           File Id (*)
            --format       Download file in a specified format (needed for google docs)
        -s, --stdout       Write file content to stdout
            --force        Overwrite existing file
            --pop          Download latest file, and remove it from google drive
    folder:
        -t, --title        Folder to create (*)
        -p, --parent       Parent Id of the folder
            --share        Share created folder
    info:
        -i, --id           File Id (*)
            --bytes        Show size in bytes
    list:
        -m, --max          Max results
            --include-docs Include google docs in listing
        -t, --title        Title filter
        -q, --query        Query (see https://developers.google.com/drive/search-parameters)
        -s, --shared       Show shared status (Note: this will generate 1 http req per file)
        -n, --noheader     Do not show the header
            --bytes        Show size in bytes
    quota:
            --bytes        Show size in bytes
    share:
        -i, --id           File Id (*)
    unshare:
        -i, --id           File Id (*)
    upload:
        -f, --file         File or directory to upload (*)
        -s, --stdin        Use stdin as file content (*)
        -t, --title        Title to give uploaded file. Defaults to filename
        -p, --parent       Parent Id of the file
            --share        Share uploaded file
            --mimetype     The MIME type (default will try to figure it out)
            --convert      File will be converted to Google Docs format
        -C, --chunksize    Set chunk size in bytes. Minimum is 262144, default is 4194304. Recommended to be a power of two.
    url:
        -i, --id           File Id (*)
        -p, --preview      Generate preview url (default)
        -d, --download     Generate download url
```

Reference:

[Backing up Files to Google Drive via the CLI](https://timothy-quinn.com/backing-up-files-to-google-drive-via-the-cli/)

[Install Gdrive Google Drive Linux Client – Upload/Download/Share a file](http://bashworkz.com/google-drive-linux-client-upload-download-share-a-file/)

