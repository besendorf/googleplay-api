<!--# Google play python API [![Build Status](https://travis-ci.org/NoMore201/googleplay-api.svg?branch=master)](https://travis-ci.org/NoMore201/googleplay-api)-->

This project contains an unofficial API for google play interactions. The code mainly comes from
[GooglePlayAPI project](https://github.com/NoMore201/googleplay-api) which is not
maintained anymore. An authentication error was fixed.

# Build

This is the recommended way to build the package, since setuptools will take care of
generating the `googleplay_pb2.py` file needed by the library (check the `setup.py`)

```
$ python setup.py build
```

# Usage

Check scripts in `test` directory for more examples on how to use this API.

```
from gpapi.googleplay import GooglePlayAPI

mail = "mymail@google.com"
passwd = "mypasswd"

api = GooglePlayAPI(locale="en_US", timezone="UTC", device_codename="hero2lte")
api.login(email=mail, password=passwd)

result = api.search("firefox")

for doc in result:
    if 'docid' in doc:
        print("doc: {}".format(doc["docid"]))
    for cluster in doc["child"]:
        print("\tcluster: {}".format(cluster["docid"]))
        for app in cluster["child"]:
            print("\t\tapp: {}".format(app["docid"]))
```

For first time logins, you should only provide email and password.
The module will take care of initializing the api, upload device information
to the google account you supplied, and retrieving 
a Google Service Framework ID (which, from now on, will be the android ID of your fake device).

For the next logins you **should** save the gsfId and the authSubToken, and provide them as parameters
to the login function. If you login again with email and password, this is the equivalent of
re-initializing your android device with a google account, invalidating previous gsfId and authSubToken.
