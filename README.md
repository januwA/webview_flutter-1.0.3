# WebView for Flutter

- 来至[webview_flutter 1.0.3](https://pub.flutter-io.cn/packages/webview_flutter)
- 添加 onRequest API 监听WebView中所有的网络请求

> 关键代码在`FlutterWebViewClient.java`中，重写`WebViewClient`的`shouldInterceptRequest`，不能直接发送`methodChannel.invokeMethod("onRequest", args)`，需要使用`Handler`处理

```
dependencies:
  webview_flutter:
    git:
      url: https://github.com/januwA/webview_flutter-1.0.3.git
      ref: main
```

[![pub package](https://img.shields.io/pub/v/webview_flutter.svg)](https://pub.dartlang.org/packages/webview_flutter)

A Flutter plugin that provides a WebView widget.

On iOS the WebView widget is backed by a [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview);
On Android the WebView widget is backed by a [WebView](https://developer.android.com/reference/android/webkit/WebView).

## Usage
Add `webview_flutter` as a [dependency in your pubspec.yaml file](https://flutter.io/platform-plugins/).

You can now include a WebView widget in your widget tree. See the
[WebView](https://pub.dev/documentation/webview_flutter/latest/webview_flutter/WebView-class.html)
widget's Dartdoc for more details on how to use the widget.



## Android Platform Views
The WebView is relying on
[Platform Views](https://flutter.dev/docs/development/platform-integration/platform-views) to embed
the Android’s webview within the Flutter app. By default a Virtual Display based platform view
backend is used, this implementation has multiple
[keyboard](https://github.com/flutter/flutter/issues?q=is%3Aopen+label%3Avd-only+label%3A%22p%3A+webview-keyboard%22).
When keyboard input is required we recommend using the Hybrid Composition based platform views
implementation. Note that on Android versions prior to Android 10 Hybrid Composition has some
[performance drawbacks](https://flutter.dev/docs/development/platform-integration/platform-views#performance).

### Using Hybrid Composition

To enable hybrid composition, set `WebView.platform = SurfaceAndroidWebView();` in `initState()`.
For example:

```dart
import 'dart:io';

import 'package:webview_flutter/webview_flutter.dart';

class WebViewExample extends StatefulWidget {
  @override
  void initState() {
    super.initState();
    if (Platform.isAndroid) WebView.platform = SurfaceAndroidWebView();
  }

  @override
  Widget build(BuildContext context) {
    return WebView(
      initialUrl: 'https://flutter.dev',
    );
  }
}
```

`SurfaceAndroidWebView()` requires [API level 19](https://developer.android.com/studio/releases/platforms?hl=th#4.4). The plugin itself doesn't enforce the API level, so if you want to make the app available on devices running this API level or above, add the following to `<your-app>/android/app/build.gradle`:

```gradle
android {
    defaultConfig {
        // Required by the Flutter WebView plugin.
        minSdkVersion 19
    }
  }
```
