# List of useful things for Mac Development

## ATOM

- Get is a bad spot and Atom doesn't open?

```bash
atom --clear-window-state
```

## GIF's

[Install gifify](https://github.com/vvo/gifify)

```bash
gifify {PATH_TO_INPUT} -o {PATH_TO_OUTPUT}
```

## GIT

- To completely remove a file from git commit history run these two commands

```bash
git filter-branch --tree-filter 'rm -f {FILE_PATH}' HEAD
git update-ref -d refs/original/refs/heads/master
```

<br>
<br>

- To reset local branch to remote

```bash
git fetch origin
git -B {branch-name}
git reset --hard origin/{branch-name}
```

<br>
<br>

- Delete remote branch

```bash
git push origin --delete {branch_name}
```

<br>
<br>

- Create a local copy of remote branch

```bash
git checkout -b <my_new_branch> <remote>/<branch_name>
```

<br>
<br>

- Change remote origin

```bash
git remote set-url origin git://new.url.here
```

<br>
<br>
* Ignore changes locally, but keep file in git (good for secrets)

```bash
git update-index --skip-worktree src/config/SECRETS.js
```

<br>
<br>

- Replace remote master with a new master by renaming and force pushing

```bash
git branch -m master old-master
git branch -m seotweaks master
git push -f origin master
```

<br>
<br>

- Delete all branches except master

```bash
git branch | grep -v "master" | xargs git branch -D
```

## iOS

`xcodebuild --help`

Does the above fail? Try this:
`xcode => preferences => locations => Command Line Tools`
and select your version
<br>
<br>

- To list the uuid of signing certs

```bash
security find-identity -v -p codesigning
```

<br>
<br>

- To view uuid's of provisioning profiles

```bash
/{root}/Library/MobileDevice/Provisioning Profiles
```

then `less {file}` will give you enough info to determine the proper file
<br>
<br>

- Archive a X-Code build, replace workspace with `-project {PROJECT_NAME}.xcodeproj` if it's not a workspace instance

- Archive path will be the output file location used for exporting an IPA

```bash
xcodebuild -workspace {PROJECT_NAME}.xcworkspace -scheme {PROJECT_NAME} -configuration Release -archivePath ${archivePath} archive -quiet -sdk iphoneos
```

<br>
<br>

- Export an IPA

```bash
xcodebuild -exportArchive -archivePath ${archivePath} -exportPath ${exportPath} -exportOptionsPlist ${exportOptionsPlistPath}
```

- Export options plist example

```xml
<plist version="1.0">
<dict>
  <key>compileBitcode</key>
  <false/>
  <key>method</key>
  <string>enterprise</string>
  <key>signingCertificate</key>
  <string>{UUID}</string>
  <key>provisioningProfiles</key>
  <dict>
    <key>{bundle_ID}</key>
    <string>{UUID}</string>
  </dict>
  <key>signingStyle</key>
  <string>manual</string>
  <key>stripSwiftSymbols</key>
  <true />
  <key>teamID</key>
  <string>{TEAM_ID}</string>
</dict>
</plist>
```

<br>
<br>

- Direct download link for exterprise apps

```bash
itms-services://?action=download-manifest&url={URL_TO_MANIFEST_PLIST.plist}
```

<br>
<br>

- Push Noticiations `.p12` to `.pem`

```bash
openssl pkcs12 -in pushcert.p12 -out pushcert.pem -nodes -clcerts
```

## React-Native

- Bundle JavaScript

```bash
./node_modules/.bin/react-native bundle --entry-file index.js --platform ios --dev false --bundle-output ios/{PROJECT_NAME}/main.jsbundle
```

<br>
<br>

- Build Android App

```bash
cd android && ./gradlew clean && ./gradlew assembleRelease && cd ../
```

## Heroku

- Create/Download Postgres `.dump` file

```bash
heroku pg:backups:capture
heroku pg:backups:download
```

```
heroku rollback v{whatever version} -r {whatever you call your remote}

git remote -v   <= will list remotes
```

## Postgres

- Copy a `.dump` to localhost

```bash
pg_restore --verbose --clean --no-acl --no-owner -h localhost -d {DATABSE_NAME} {PATH_TO_DUMP_FILE}
```

<br>
<br>

- Debug localhost connections

```bash
postgres -D /usr/local/var/postgres
```

<br>
<br>

- Reset locally

[TO COMPLETELY RESET POSTGRESQL LOCALLY](https://medium.com/@bitadj/completely-uninstall-and-reinstall-psql-on-osx-551390904b86)

## Chrome

- Unregister service workers `chrome://serviceworker-internals/`

## Redis

This is nice:

```js
/*  Some Constants File */
export const REDIS_PORT_OR_URL = isDev
  ? Number.parseInt(process.env.REDIS_PORT, 10)
  : process.env.REDISCLOUD_URL;

/* Some other file */
import redis from "redis";

import { REDIS_PORT_OR_URL } from "../constants";

const client = redis.createClient(REDIS_PORT_OR_URL, { no_ready_check: true });

export default client;
```

<br>
<br>

- `start:redis` start local server with chrome GUI

```bash
"start:redis": "concurrently --kill-others-on-fail \"yarn start:redis:server\" \"yarn start:redis:commander\" \"wait-on http://localhost:5002 && yarn open:chrome:redis:commander\""

"start:redis:server": "redis-server"

"start:redis:commander": "./node_modules/.bin/redis-commander -p 5002 "

"open:chrome:redis:commander": "open http://localhost:5002/"
```

## NPM

- [List config:](https://docs.npmjs.com/misc/config)

```bash
npm config list
```

<br>
<br>

- Set config:

```bash
npm set {key} {value}
```

<br>
<br>

.npmrc in root overrides local settings

- Private package settings

```bash
always-auth = true /* usually false */
metrics-registry = https://registry.npmjs.org/
registry = https://registry.npmjs.org/
```

## MONGO

- Initial Installation

```bash
brew update
brew install mongodb
sudo mkdir -p /data/db
sudo chmod 0755 /data/db && sudo chown $USER /data/db
```

- Mongo Shell


```bash
mongo
```

    * Within Mongo Shell
    ```bash
    show dbs  // will list localhost dbs
    ```

<br>
<br>
* To Delete a localhost db, within Mongo Shell
```bash
use <dbname from whatever show dbs lists>
db.dropDatabase()
```
<br>
<br>

- Copy a remote Mongo from the database URI with the below format

`mongodb://<username>:<password>@<url>:<port>/<database>`

```bash
mongodump -h <url>:<port> -d <database> -u <username> -p <password>
```

`mongodump` will create a folder, within the current directory, `dump/<database>/`
<br>
<br>

- Copy a dump directory to localhost Mongo

```bash
mongorestore -d <dbname, same as the one dropped> <path to dump database dump/<database>/>
```

## MYSQL

- start `mysql.server start`
- stop `mysql.server stop`
- Enter the `mysql -u root` shell after starting the mysql server
  - create database `create database {name};` <-- semi-colon IMPORTANT
  - exit `exit;`

```
mysql -u root -p
mysql> SET GLOBAL innodb_fast_shutdown = 1;
mysql> use mysql;
mysql> update user set plugin='mysql_native_password' where user='root';
mysql_upgrade -u root -p
```

- Client does not support authentication protocol requested by server on Sequelize migration???

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'
```

<br>
<br>
* MySQL all fucked up?  Try this:
```bash
mysqld stop
```
<br>
<br>
* Then this:
```bash
brew update
brew remove mysql
brew install mysql
```

## Rando

- Lists some good ip stuff

```bash
ifconfig
```

To stgream and save so to a file run

```bash
yarn start | tee filename.log
```

To debug dns and not found errors

8.8.8.8 is google's dns and it should give you the ip of the end server
```
nslookup domain 8.8.8.8
```
