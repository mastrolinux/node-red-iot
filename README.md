## Node-RED for Arduino IoT Cloud, ready to deploy on Heroku

The idea of the repo is to apply a single patch on top of the latest version of
Node-RED source code, then add some config vars and deploy it to Heroku.

Steps are:

1. clone this repo
```
    git clone https://github.com/mastrolinux/node-red-iot.git
```
2. Clone Node-RED

```    
    git clone https://github.com/node-red/node-red node-red-patched
    cd node-red-patched
    git checkout 1.1.0
    git checkout -b iot-patch
```

3. Apply my patch
```
    git am < ../node-red-iot/*.patch
``` 

4. Install Node-RED

    npm install
    npm run build

5. Create a password with Node-RED Admin
```
    ./node_modules/node-red-admin/node-red-admin.js hash-pw
```

6. Set env vars and start Node-RED, create a `.env` file in your current dir with some info of
**your choice** and then export the vars

```
    NODERED_PASSWORD_HASH='yourpassword-hash-from-the-node-red-admin-output'
    NODERED_USERNAME='admin'
    NODERED_CREDENTIAL_SECRET='averylongsecretofyourchoice'

    export $(cat .env | xargs)
    npm start
```

7. (Optional, suggested) Deploy it on Heroku

```
    heroku login
    heroku create
    cat .env | xargs heroku config:set
    git push heroku iot-patch:master
```

7. If you then add flows and credentials please remember to commit them on your own current private repo, Heroku does not have a permanent file-system, so adding them to your git repo is necessary to avoid loosing your work.