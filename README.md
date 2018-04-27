# List of useful terminal commands for Mac Development

## GIF's

[Install gifify](https://github.com/vvo/gifify)

```bash
gifify {PATH_TO_INPUT} -o {PATH_TO_OUTPUT}
```

## GIT

* To completely remove a file from git commit history run these two commands

```bash
git filter-branch --tree-filter 'rm -f {FILE_PATH}' HEAD
git update-ref -d refs/original/refs/heads/master
```

* To reset local branch to remote

```bash
git fetch origin
git -B {branch-name}
git reset --hard origin/{branch-name}
```

## iOS

`xcodebuild --help`

* To list the uuid of signing certs

```bash
security find-identity -v -p codesigning
```

* To view uuid's of provisioning profiles

```bash
/{root}/Library/MobileDevice/Provisioning Profiles
```
then `less {file}` will give you enough info to determine the proper file

* Archive a X-Code build, replace workspace with `-project {PROJECT_NAME}.xcodeproj` if it's not a workspace instance

* Archive path will be the output file location used for exporting an IPA

```bash
xcodebuild -workspace {PROJECT_NAME}.xcworkspace -scheme {PROJECT_NAME} -configuration Release -archivePath ${archivePath} archive -quiet -sdk iphoneos
```

* Export an IPA

```bash
xcodebuild -exportArchive -archivePath ${archivePath} -exportPath ${exportPath} -exportOptionsPlist ${exportOptionsPlistPath}
```

* Export options plist example

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

* Direct download link for exterprise apps

```bash
itms-services://?action=download-manifest&url={URL_TO_MANIFEST_PLIST.plist}
```

* Push Noticiations `.p12` to `.pem`

```bash
openssl pkcs12 -in pushcert.p12 -out pushcert.pem -nodes -clcerts
```

## React-Native

* Bundle JavaScript

```bash
./node_modules/.bin/react-native bundle --entry-file index.js --platform ios --dev false --bundle-output ios/{PROJECT_NAME}/main.jsbundle
```

* Build Android App

```bash
cd android && ./gradlew clean && ./gradlew assembleRelease && cd ../
```

## Heroku

*  Create/Download Postgres `.dump` file

```bash
heroku pg:backups:capture
heroku pg:backups:download
```

## Postgres

* Copy a `.dump` to localhost

```bash
pg_restore --verbose --clean --no-acl --no-owner -h localhost -d {DATABSE_NAME} {PATH_TO_DUMP_FILE}
```

* Debug localhost connections

```bash
postgres -D /usr/local/var/postgres
```

* Reset locally

[TO COMPLETELY RESET POSTGRESQL LOCALLY](https://medium.com/@bitadj/completely-uninstall-and-reinstall-psql-on-osx-551390904b86)

## Chrome

* Unregister service workers `chrome://serviceworker-internals/`


# OTHER USERFUL THINGS

## Redis

This is nice:
```js
/*  Some Constants File */
export const REDIS_PORT_OR_URL = isDev
  ? Number.parseInt(process.env.REDIS_PORT, 10)
  : process.env.REDISCLOUD_URL;


/* Some other file */
import redis from 'redis';

import { REDIS_PORT_OR_URL } from '../constants';

const client = redis.createClient(REDIS_PORT_OR_URL, { no_ready_check: true });

export default client;

```

* `start:redis` start local server with chrome GUI

```bash
"start:redis": "concurrently --kill-others-on-fail \"yarn start:redis:server\" \"yarn start:redis:commander\" \"wait-on http://localhost:5002 && yarn open:chrome:redis:commander\""

"start:redis:server": "redis-server"

"start:redis:commander": "./node_modules/.bin/redis-commander -p 5002 "

"open:chrome:redis:commander": "open http://localhost:5002/"
```
## NPM

* [List config:](https://docs.npmjs.com/misc/config)
```bash
npm config list
```

* Set config:
```bash
npm set {key} {value}
```

.npmrc in root overrides local settings

* Private package settings
```bash
always-auth = true /* usually false */
metrics-registry = https://registry.npmjs.org/
registry = https://registry.npmjs.org/
```
