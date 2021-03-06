<img src="resources/logo.png" width="300">

[![Build Status](https://travis-ci.org/ZerataX/matrix-registration.svg?branch=master)](https://travis-ci.org/ZerataX/matrix-registration) [![Coverage Status](https://coveralls.io/repos/github/ZerataX/matrix-registration/badge.svg)](https://coveralls.io/github/ZerataX/matrix-registration) [![Matrix Chat](https://img.shields.io/badge/chat-%23matrix--registration%3Admnd.sh-brightgreen.svg)](https://matrix.to/#/#matrix-registration:dmnd.sh)
# matrix-registration

a simple python application to have a token based matrix registration

if you like me encountered the situation where you want to invite your friends to your homeserver, but neither wanted to open up public registration nor create accounts for every individual user yourself, this project should be the solution.

with this project you can just quickly generate tokens on the fly and share them with your friends to allow them to register to your homeserver.

## setup
```bash
git clone https://github.com/ZerataX/matrix-registration.git
cd matrix-registration

virtualenv -p /usr/bin/python3.6 .
source ./bin/activate
pip install -r requirements.txt

cp config.sample.yaml config.yaml
```
and edit config.yaml

### nginx reverse-proxy
an example nginx setup to expose the html form and the api endpoint on the same URL, based on whether a POST or GET request was made.
```nginx
location /register {
    alias resources/example.html;

    if ($request_method = POST ) {
        proxy_pass http://localhost:5000;
    }
}
```
## usage
```bash
python -m matrix_registration -h
```

if you've started the api server and generated a token you can register an account with curl, e.g.:
```bash
curl -X POST \
     -F 'username=test' \
     -F 'password=verysecure' \
     -F 'confirm=verysecure' \
     -F 'token=DoubleWizardSki' \
     http://localhost:5000/register
```
or a simple html form, see the sample [resources/example.html](resources/example.html)

the html page looks for the query paramater `token` and sets the token input field to it's value. this would allow you to directly share links with the token included, e.g.:
`https://homeserver.tld/register.html?token=DoubleWizardSki`


For more info check the [wiki](https://github.com/ZerataX/matrix-registration/wiki)
