# flavor_example
Flutterで環境分けをやってみる。

こちらをのパッケージを追加する。
https://pub.dev/packages/flutter_flavor

### まずは作ってみよう!
環境を切り替えて使う本番用の`main_prod.dart`とテスト用の`main_dev.dart`を作成します。

## ただのカウンターアプリです

### 🚀本番用のProd:
```dart
import 'package:flutter/material.dart';

import 'package:flutter_flavor/flutter_flavor.dart';

void main() {
  FlavorConfig(
    name: "PRODUCTION",
    color: Colors.red,
    location: BannerLocation.bottomStart,
    variables: {
      "counter": 5,
      "baseUrl": "https://www.example.com",
    },
  );
  return runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return FlavorBanner(
      color: Colors.blue,
      location: BannerLocation.topStart,
      child: MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(
          primarySwatch: Colors.red,
        ),
        home: const MyHomePage(title: 'Flutter Prod'),
      ),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({
    Key? key,
    required this.title,
  }) : super(key: key);

  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = FlavorConfig.instance.variables["counter"];

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: FlavorConfig.instance.color,
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              '本番用アプリ:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

## 🔧テスト用の
```dart
import 'package:flutter/material.dart';

import 'package:flutter_flavor/flutter_flavor.dart';

void main() {
  FlavorConfig(
    name: "DEVELOP",
    color: Colors.green,
    location: BannerLocation.bottomStart,
    variables: {
      "counter": 5,
      "baseUrl": "https://www.example.com",
    },
  );
  return runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return FlavorBanner(
      color: Colors.blue,
      location: BannerLocation.topStart,
      child: MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(
          primarySwatch: Colors.green,
        ),
        home: const MyHomePage(title: 'Flutter Dev'),
      ),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({
    Key? key,
    required this.title,
  }) : super(key: key);

  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = FlavorConfig.instance.variables["counter"];

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: FlavorConfig.instance.color,
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'テスト用アプリ:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

## launch.jsonを作成する
`program`というkeyにlibディレクトリ配下の本番用とテスト用のファイルのパスを指定する。

```json
{
    "version": "0.2.0",
    "configurations": [
      {
        "name": "PROD-Flavor",
        "program": "lib/main_prod.dart",
        "request": "launch",
        "type": "dart"
      },
      {
        "name": "DEV-Flavor",
        "program": "lib/main_dev.dart",
        "request": "launch",
        "type": "dart"
      }
    ]
  }
```
