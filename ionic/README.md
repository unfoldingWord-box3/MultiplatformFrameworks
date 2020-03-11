# Ionic

## Findings

Ionic:
- uses "normal-looking" React. It is based on `react-dom` (contrast with React Native, which does not).
- prefers TypeScript.
- on using `yarn`: 
    - `yarn` resulted in a complete package rebuild
    - `yarn start` worked OK, but served from port 3000 instead of ionic's normal 8100
    - It did complain and advise against mixing package managers, so I started over and will try to abide with NPM.


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