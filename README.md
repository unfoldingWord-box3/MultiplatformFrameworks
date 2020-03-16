# Multiplatform Frameworks
This is a research project to evaluate multi-platform frameworks.

*Update 2020-03-16* I started the evaluation with the latest tooling, namely, Ionic. As it turns out, their `capacitor` tool works really well and is even somewhat backward compatible with `cordova`. Thus the evaluation was shortened to only study Ionic's Capacitor. As of this writing, the following has been lightly tested:
- Win10, amd64
- Linux, amd64


## Requirements

Here is a list of requirements.

1. Supports React applications
2. Supports both JavaScript and TypeScript
3. Supports the following target platforms:
- Win10
- MacOS
- iOS
- Android
- Linux
- PWA (Progressive Web App)

## Evaluation Criteria

The following features will distinguish one framework from another:

- Conversion of existing React Web apps to the supported platforms
- Detect updates to code and facilitates update process
- Enables work offline and sync when back online
- Can use `yarn`

## [Original] List of Frameworks Evaluated

This is the initial list of frameworks to evaluate:

- [Cordova](https://cordova.apache.org/)
- [React Native](https://reactnative.dev/)
- [Ionic](https://ionicframework.com/blog/announcing-ionic-react/)
