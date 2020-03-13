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
```
cd my-app
npm install --save @capacitor/core @capacitor/cli
# yarn add @capacitor/core @capacitor/cli
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

To run,  `npx cap open electron`


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


## Demos

I completed the getting started demo using the sample "tabs" app.

Next I installed the sample "conference" app with `ionic start kitchenSink` and responded to the prompts to use a react app to use the "conference" app. This time I am going to try to use yarn exclusively. Thus:
- remove/rename `package-lock.json`
- run `yarn`; this command completely rebuilt the packages and created a `yarn.lock` file
- run `yarn start`; this yielded a compile time error (a type problem).
- restarted from scratch to see if ionic itself yielded the same error. No error... thus use of 'yarn' is questionable.
- the preceding also means we may not be able to deploy without an Ionic license.

## Getting Started Notes

Followed steps found here: 
https://ionicframework.com/blog/announcing-ionic-react/

Commands run to create the sample app:
```
npm i -g ionic
ionic start my-react-app
```

Once built, change to the directory `my-react-app` and run `ionic serve`.

Ionic apps are served from `localhost:8100`.

## Ionic Command Help 

Note that a number of the features are paid. Probably their revenue model.

```sh
$ ionic


   _             _
  (_) ___  _ __ (_) ___
  | |/ _ \| '_ \| |/ __|
  | | (_) | | | | | (__
  |_|\___/|_| |_|_|\___| CLI 5.4.16


  Usage:

    $ ionic <command> [<args>] [--help] [--verbose] [--quiet] [--no-interactive] [--no-color] [--confirm] [options]        

  Global Commands:

    completion ...................... (experimental) Enables tab-completion for Ionic CLI commands.
    config <subcommand> ............. Manage CLI and project config values (subcommands: get, set, unset)
    docs ............................ Open the Ionic documentation website
    info ............................ Print project, system, and environment information
    init ............................ (beta) Initialize existing projects with Ionic
    login ........................... Log in to Ionic
    logout .......................... Log out of Ionic
    signup .......................... Create an Ionic account
    ssh <subcommand> ................ Commands for configuring SSH keys (subcommands: add, delete, generate, list,
                                      setup, use)
    start ........................... Create a new project

  Project Commands:

    build ........................... Build web assets and prepare your app for any platform targets
    capacitor <subcommand> .......... Capacitor functionality (subcommands: add, copy, open, run, sync, update) (alias:    
                                      cap)
    cordova <subcommand> ............ Cordova functionality (subcommands: build, compile, emulate, platform, plugin,       
                                      prepare, requirements, resources, run)
    deploy <subcommand> ............. (paid) Appflow Deploy functionality (subcommands: add, build)
    doctor <subcommand> ............. Commands for checking the health of your Ionic project (subcommands: check, list,    
                                      treat)
    enterprise <subcommand> ......... (paid) Manage Ionic Enterprise features (subcommands: register)
    git <subcommand> ................ Commands relating to git (subcommands: remote)
    integrations <subcommand> ....... Manage various integrations in your app (subcommands: disable, enable, list)
                                      (alias: i)
    link ............................ Connect local apps to Ionic
    package <subcommand> ............ (paid) Appflow package functionality (subcommands: build)
    repair .......................... Remove and recreate dependencies and generated files
    serve ........................... Start a local dev server for app dev/testing (alias: s)
    ssl <subcommand> ................ (experimental) Commands for managing SSL keys & certificates (subcommands:
                                      generate)
```

Also this on Appflow function:
```
 Ionic Appflow, the mobile DevOps solution by Ionic

           Continuously build, deploy, and ship apps
        Focus on building apps while we automate the rest

        Learn more: https://ion.link/appflow
```