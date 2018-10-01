# react-native-view-pdf

[![npm](https://img.shields.io/npm/l/express.svg)](https://github.com/rumax/react-native-PDFView)
[![npm version](https://badge.fury.io/js/react-native-view-pdf.svg)](https://badge.fury.io/js/react-native-view-pdf)
[![CircleCI](https://circleci.com/gh/rumax/react-native-PDFView.svg?style=shield)](https://circleci.com/gh/rumax/react-native-PDFView)
[![codecov](https://codecov.io/gh/rumax/react-native-PDFView/branch/master/graph/badge.svg)](https://codecov.io/gh/rumax/react-native-PDFView)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

Library for displaying [PDF documents](https://acrobat.adobe.com/us/en/acrobat/about-adobe-pdf.html) in [react-native](http://facebook.github.io/react-native/)

- android - uses [Android PdfViewer](https://github.com/barteksc/AndroidPdfViewer). Stable version `com.github.barteksc:android-pdf-viewer:2.8.2` is used. Targets minSdkVersion 19 and above

- ios - uses [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview).
Targets iOS9.0 and above

- zero NPM dependencies

## Getting started

`$ npm install react-native-view-pdf --save`

### Mostly automatic installation

`$ react-native link react-native-view-pdf`

### Manual installation


#### iOS

1. Add ReactNativeViewPDF project to your project
2. Under your build target general settings, add the library to your Linked Frameworks and Libraries
3. If you run into any issues, confirm that under Build Phases -> Link Binary With Libraries the library is present

##### Using CocoaPods

In your Xcode project directory open Podfile and add the following line:

```
pod 'RNPDF', :path => '../node_modules/react-native-view-pdf'
```

And install:

```
pod install
```

#### Android

1. Open up `android/app/src/main/java/[...]/MainApplication.java`
  - Add `import com.rumax.reactnative.pdfviewer.PDFViewPackage;` to the imports at the top of the file
  - Add `new PDFViewPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
    ```
    include ':react-native-view-pdf'
    project(':react-native-view-pdf').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-view-pdf/android')
    ```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
    ```
    compile project(':react-native-view-pdf')
    ```

##### Note for Android
  The Android project tries to retrieve the following properties:
   - compileSdkVersion
   - buildToolsVersion
   - minSdkVersion
   - targetSdkVersion

  from the `ext` object if you have one defined in your Android's project root (you can read more about it [here](https://docs.gradle.org/current/userguide/writing_build_scripts.html#example_using_extra_properties)). If not, it falls back to its current versions (check [the gradle file](./android/build.gradle) for additional information).

#### Windows
[ReactWindows](https://github.com/ReactWindows/react-native)

N/A

## Demo

Android | iOS
------- | ---
![Android](https://github.com/rumax/react-native-PDFView/raw/master/demo/res/android_pdf.gif) | ![iOS](https://github.com/rumax/react-native-PDFView/raw/master/demo/res/ios_pdf.gif)


### Quick Start

```
// With Flow type annotations (https://flow.org/)
import PDFView from 'react-native-view-pdf';
// Without Flow type annotations
// import PDFView from 'react-native-view-pdf/lib/index';

const resources = {
  file: Platform.OS === 'ios' ? 'test-pdf.pdf' : '/sdcard/Download/test-pdf.pdf',
  url: 'https://www.ets.org/Media/Tests/TOEFL/pdf/SampleQuestions.pdf',
  base64: 'JVBERi0xLjMKJcfs...',
};

export default class App extends React.Component {
  render() {
    const resourceType = 'base64';

    return (
      <View style={{ flex: 1 }}>
        {/* Some Controls to change PDF resource */}
        <PDFView
          fadeInDuration={250.0}
          style={{ flex: 1 }}
          resource={resources[resourceType]}
          resourceType={resourceType}
          onLoad={() => console.log(`PDF rendered from ${resourceType}`);}
          onError={() => console.log('Cannot render PDF', error)}
        />
      </View>
    );
  }
}
```

Use the [demo](https://github.com/rumax/react-native-PDFView/tree/master/demo) project to:

- Test the component on both android and iOS
- Render PDF using URL, BASE64 data or local file
- Handle error state

### Props

Name | Android | iOS | Description
---- | ------- | --- | -----------
resource | ✓ | ✓ | A resource to render. It's possible to render PDF from `file`, `url` or `base64`
resourceType | ✓ | ✓ | Should correspond to resource and can be: `file`, `url` or `base64`
onLoad | ✓ | ✓ | Callback that is triggered when loading is completed
onError | ✓ | ✓ | Callback that is triggered when loading has failed. And error is provided as a function parameter
style | ✓ | ✓ | A [style](https://facebook.github.io/react-native/docs/style)
fadeInDuration | ✓ | ✓ | Fade in duration (in ms, defaults to 0.0) to smoothly fade the webview into view when pdf loading is completed
urlProps | ✓ | ✓ | Extended properties for `url` type that allows to specify HTTP Method, HTTP headers and HTTP body
onPageChanged | ✓ | ✗ | Callback that is invoked when page is changed. Provides `active page` and `total pages` information.

### Development tips

On android for the `file` type it is required to request permissions to
read/write. You can get more information in [PermissionsAndroid](https://facebook.github.io/react-native/docs/permissionsandroid)
section from react native or [Request App Permissions ](https://developer.android.com/training/permissions/requesting) from android
documentation. [Demo](https://github.com/rumax/react-native-PDFView/tree/master/demo)
project provides an example how to implement it using Java, check the [MainActivity.java](https://github.com/rumax/react-native-PDFView/blob/b84913df174d3b638d2d820a66ed4e6605d56860/demo/android/app/src/main/java/com/demo/MainActivity.java#L12) and [AndroidManifest.xml](https://github.com/rumax/react-native-PDFView/blob/b84913df174d3b638d2d820a66ed4e6605d56860/demo/android/app/src/main/AndroidManifest.xml#L6) files.

Before trying `file` type in demo project, open `sdcard/Download` folder in `Device File Explorer` and store the `test-pdf.pdf` document that you want to render (you can find an example in demo/ios/test-pdf.pdf).

In [demo](https://github.com/rumax/react-native-PDFView/tree/master/demo) project you can also run the simple server to serve PDF file. To do this navigate to `demo/utils/` and start the server
`node server.js`. (*Do not forget to set proper IP adress of the server
in `demo/App.js`*)

## License

[MIT](https://opensource.org/licenses/MIT)

## Authors
- [sanderfrenken](https://github.com/sanderfrenken)
- [rumax](https://github.com/rumax)

### Other information

- Generated with [react-native-create-library](https://github.com/frostney/react-native-create-library)
- Zero JavaScript dependency. Which means that you do not bring other dependencies to your project
- If you think that something is missing or would like to propose new feature, please, discuss it with authors
- Please, feel free to ⭐️ the project. This gives us a confidence that you like it and we did a great job by publishing and supporting it 🤩
- [If you are using ProGuard, add following rule to proguard config file:](https://github.com/barteksc/AndroidPdfViewer#proguard)

```
    -keep class com.shockwave.**
```
