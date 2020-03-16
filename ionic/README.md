# Ionic

## Findings

Ionic:
- uses "normal-looking" React. It is based on `react-dom` (contrast with React Native, which does not).
- prefers TypeScript.
- prefers hooks to classes
- on using `yarn`: 
    - `yarn` resulted in a complete package rebuild
    - `yarn start` worked OK, but served from port 3000 instead of ionic's normal 8100
    - It did complain and advise against mixing package managers, so I started over and will try to abide with NPM. See more yarn below.
- Ionic makes a tool named `capacitor` that is used take existing react web apps and create binaries for ios, Android, and Electron (desktop). Link: https://capacitor.ionicframework.com/docs/

So the rest of this is focused on `capacitor`.

From the docs:

>Capacitor is a spiritual successor to Apache Cordova and Adobe PhoneGap, with inspiration from other popular cross-platform tools like React Native and Turbolinks, but focused entirely on enabling modern web apps to run on all major platforms with ease. Capacitor has backwards-compatible support for many existing Cordova plugins.

## Capacitor

To add to an existing web app:
- via NPM:
```
cd my-app
npm install --save @capacitor/core @capacitor/cli
```
- via yarn:
```
yarn add @capacitor/core @capacitor/cli
```

I'll use my book-package-app for this.

```
$ cd book-package-app
$ yarn add @capacitor/core @capacitor/cli
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 9 new dependencies.
info Direct dependencies
├─ @capacitor/cli@1.5.1
└─ @capacitor/core@1.5.1
info All dependencies
├─ @capacitor/cli@1.5.1
├─ @capacitor/core@1.5.1
├─ cli-spinners@1.3.1
├─ compare-versions@3.6.0
├─ ora@1.4.0
├─ plist@3.0.1
├─ xml2js@0.4.23
├─ xmlbuilder@9.0.7
└─ xmldom@0.1.31
Done in 33.47s.
$
$ npx cap init
? App name book-package-app
? App Package ID (in Java package format, no dashes) io.github.unfoldingword
√ Initializing Capacitor project in C:\Users\mando\Projects\unfoldingWord\book-package-app 
in 10.16ms


*   Your Capacitor project is ready to go!  *

Add platforms using "npx cap add":

  npx cap add android
  npx cap add ios
  npx cap add electron

Follow the Developer Workflow guide to get building:
https://capacitor.ionicframework.com/docs/basics/workflow
$
```

This created a JSON config:
```
{
  "appId": "io.github.unfoldingword",
  "appName": "book-package-app",
  "bundledWebRuntime": false,
  "npmClient": "yarn",
  "webDir": "public",
  "cordova": {}
}
```

Note: I had to change the webDir value to `build`.

Also, review the `package.json` that is created. Be sure the `name` property has the right application name and description.

Then I added the Electron target platform:
```
$ npx cap add electron
- Adding Electron project in: C:\Users\mando\Projects\unfoldingWord\book-package-app\electr√ Adding Electron project in: C:\Users\mando\Projects\unfoldingWord\book-package-app\electron in 25.34ms
√ Installing NPM Dependencies in 86.64s
√ add in 86.68s
√ Copying web assets from public to electron\app in 21.48ms
√ Copying capacitor.config.json in 3.16ms
√ copy in 37.86ms
√ update electron in 120.00μp

mando@DESKTOP-0V8P6MM MINGW64 ~/Projects/unfoldingWord/book-package-app (master)
$
```

Note 1: everytime a change is made to the app, you have to copy the `build` folder to the platform folders.
Thus: `npx cap copy`

Note 2: everytime the electron folder is updated from `build`, you have to correct the `index.html` file.
This file is located in `./electron/app/index.html`. All the assets are given as web URLs with a `book-package-app` base.
For example: `/book-package-app/favicon.ico`. All such references, must be changed to simply ".".
Thus the aforementioned becomes: `./favicon.ico`.

To run,  `npx cap open electron`. 


### Packaging the App

- began by installing electron globally (as opposed to a dev dependency within the app)
`npm install electron -g`
- To verify:
```
$ electron --version

v8.1.1
$
```

Then I was able run by:
```
$ cd book-package-app
$ cd electron
$ electron .
```

Next, install something called the "electron-packager". This will create a ".exe" for Windows or an appropriate executable for other platforms. Install globally, otherwise the command will be buried somewhere and will not be on your path.
```
$ npm install electron-packager -g
$ electron-packager --version
Electron Packager 14.2.1
Node v10.16.3
Host Operating system: win32 10.0.18362 (x64)
$ 
```
- Next package the app:
```
$ pwd
/c/Users/mando/Projects/unfoldingWord/book-package-app/electron
$ electron-packager .
Packaging app for platform win32 x64 using electron v7.1.14
Wrote new app to C:\Users\mando\Projects\unfoldingWord\book-package-app\electron\capacitor-app-win32-x64
$ electron-packager .
Packaging app for platform win32 x64 using electron v7.1.14
Wrote new app to C:\Users\mando\Projects\unfoldingWord\book-package-app\electron\book-package-app-win32-x64
$ 
```
- This creates a folder named (by default): `book-package-app-win32-x64`. Inside this folder will be a Windows executable:
`book-package-app.exe`.
- NOTE: the icon for the app is the react icon... how can I change this to be the unfoldingWord icon?
- You can use the "all" option to generate executables for all supported platforms!
```
$ electron-packager . --all
... elided ...
Wrote new apps to:
C:\Users\mando\Projects\unfoldingWord\book-package-app\electron\book-package-app-linux-ia32
C:\Users\mando\Projects\unfoldingWord\book-package-app\electron\book-package-app-win32-ia32
C:\Users\mando\Projects\unfoldingWord\book-package-app\electron\book-package-app-linux-x64
true
C:\Users\mando\Projects\unfoldingWord\book-package-app\electron\book-package-app-linux-armv7l
C:\Users\mando\Projects\unfoldingWord\book-package-app\electron\book-package-app-linux-arm64
C:\Users\mando\Projects\unfoldingWord\book-package-app\electron\book-package-app-win32-arm64
$ 
```



### Starting with "new" Ionic Web App

Using the `tabs` demo app...

from https://capacitor.ionicframework.com/docs/getting-started/with-ionic:
- created app with `ionic start myApp tabs --capacitor`
- verified working with `ionic serve`
- did a build with `ionic build`
- initialized capacitor with `npx cap init myApp org.unfoldingword.myApp`
  - used npm client
- changed webDir value in capacitor.config.json to `build`
- added "electron" as a target platform with `npx cap add electron`
- ionic build has some omissions that must be fixed by hand:
  - in `electron/app/index.html`, remove the `<base href="/" />` tag
  - in `electron/package.json`, add a property `"homepage": "."`
- started electron client with `npx cap open electron`
