# Giftless + S3 + gunicorn + Basic Auth + Heroku

This is not much more than Giftless configured to use an S3 backend. The added
Procfile will make it run almost-out-of-the-box (read on) on Heroku, but you can
also run it from your local computer, on a private server, or wherever you wish.
As long as it runs Python, of course.

## What you will need

- A machine that can run Python apps
- An S3 bucket
- An AWS access key to access the bucket

## Running locally - preparation

Fork this repo and clone it on your computer. Turn it into a Python virtual environment
and install the requirements.

```
git clone [your giftless git url]
cd giftless
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Run on Heroku - preparation

Fork this repo. Create a new app for heroku (cli or web interface). Heroku supports
deploying from github repositories. Follow the instructions upon app creation to enable.

## Configure your app

- edit giftless.yaml and put in the name of your S3 bucket
- set the following environment variables, either locally or as a config for your heroku app
  - AWS_ACCESS_KEY_ID:     EXAMPLEAKIAXHKSUMKEW
  - AWS_SECRET_ACCESS_KEY: EXAMPLEficsLoem2yCK0T1ey9TTt8BfC9G6FH
  - GIFTLESS_CONFIG_FILE:  giftless.yaml

## Run your app
### Locally

```
gunicorn giftless.wsgi_entrypoint:app
```

### Heroku

commit your changes and push to github. Within a couple of minutes, Heroku will build and deploy
your app.

## Use your app

Create a git repository and enable LFS on it as per the [official documentation](https://git-lfs.github.com/).

Giftless itself also has good [Getting Started doc](https://giftless.datopian.com/en/latest/quickstart.html).

## Secure your app

If you decide Git LFS is for you (I think working with it is a bit finicky) and want to keep using it, 
you will have to think about securing your app. Out of the box, Giftless supports read-only or full
read-write for anonymous users, and authentication with JWT tokens.

Allowing anonymous users full read-write access to your git-lfs server is OK for your local laptop,
but doing so on a public platform like Heroku is _obviously_ a bad idea.

JWT tokens can be pretty secure, but are a bit hard to configure, and maybe a bit overkill as well.
I have added a simple WSGI middleware that supports http[s] basic authentication. Enabling is is really
simple, by adding an env var: 
```
WSGI_AUTH_CREDENTIALS=foo:bar
```
for a user _foo_ with password _bar_. Documentation [here](https://pypi.org/project/wsgi-basic-auth/).

If you use basic auth, you might want to look into [git credentials](https://git-scm.com/docs/gitcredentials) 
to help you store and look up your password.



## Learn more

_Giftless can do much more than use an S3 backend. And if thinks aren't working, checking their official
documentation is the best first step you can take._

Giftless a Python implementation of a [Git LFS][1] Server. It is designed
with flexibility in mind, to allow pluggable storage backends, transfer
methods and authentication methods.

Giftless supports the *basic* Git LFS transfer mode with the following
storage backends:

* Local storage
* [Google Cloud Storage](https://cloud.google.com/storage)
* [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/)
  with direct-to-cloud or streamed transfers
* [Amazon S3 Storage](https://aws.amazon.com/s3/)

In addition, Giftless implements a custom transfer mode called `multipart-basic`,
which is designed to take advantage of many vendors' multipart upload
capabilities. It requires a specialized Git LFS client to use, and is currently 
not supported by standard Git LFS. 

See the [giftless-client](https://github.com/datopian/giftless-client) project
for a compatible Python Git LFS client. 

Additional transfer modes and storage backends could easily be added and
configured.

[1]: https://git-lfs.github.com/

Documentation
-------------
* [Installation Guide](https://giftless.datopian.com/en/latest/installation.html)
* [Getting Started](https://giftless.datopian.com/en/latest/quickstart.html) 
* [Full Documentation](https://giftless.datopian.com/en/latest/)
* [Developer Guide](https://giftless.datopian.com/en/latest/development.html)

License
-------
Copyright (C) 2020, Datopian / Viderum, Inc.

Giftless is free / open source software and is distributed under the terms of
the MIT license. See [LICENSE](LICENSE) for details.
