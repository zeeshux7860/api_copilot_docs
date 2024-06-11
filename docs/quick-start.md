### Quick Start Guide for Using API Copilot

Follow these steps to quickly integrate APIs into your Flutter application using API Copilot:

- **Using API Copilot**
<!-- 1. **Install API Copilot**: Ensure you have API Copilot installed in your Flutter project. You can install it via pub.dev or by adding it as a dependency in your pubspec.yaml file. -->


?> **Required** Please make sure to install required package.

### Required Packages for State Management

| State Management    | Required Packages                         |
|---------------------|-------------------------------------------|
| GetX                | flutter pub add http  <br> flutter pub add get  <br> flutter pub add shared_preferences |
| Provider            | flutter pub add provider  <br> flutter pub add http  <br> flutter pub add shared_preferences |
| Without State Management | flutter pub add http  <br> flutter pub add shared_preferences |



### Importing Postman Collections
1. Log in to API Copilot.
2. Navigate to the "Import" section.
3. Upload your Postman collection file.
4. Confirm the import to proceed.

### Generating Code
1. After importing your Postman collection, select it from the list.
2. Choose the desired settings for code generation.
3. Click on the "Generate Code" button.
4. Review and download the generated Dart code.

### Managing State (Optional)
If you prefer to use state management in your application:
1. Enable state management features in the settings.
2. Choose between Provider or GetX as your state management solution.
3. Follow the prompts to integrate state management into your code.


---






<!-- State
    Managment **GetX**.
 -->


# Quick Start Guide for Using API Copilot

Integrate APIs into your Flutter application effortlessly with API Copilot. Follow these steps to get started:

---

[GetX](#get) | [Provider](#provider) | [Default](#default)

---


## <a name="get"></a> Default Without State Management Implementation

This guide explains how to use the default state management in your Flutter application without any external state management packages.

---

### 1. Install Required Packages

Make sure to add the necessary packages to your `pubspec.yaml` file:

```yaml
dependencies:
  http:
  get:
```

### 2. Example Code

```dart
import 'package:flutter/material.dart';
import 'package:myapp/api/callback/products.dart';
import 'package:myapp/api/model/products/get_all.dart';

class DefaultPage extends StatefulWidget {
  const DefaultPage({super.key});

  @override
  State<DefaultPage> createState() => _DefaultPageState();
}

class _DefaultPageState extends State<DefaultPage> {
  GetAllModel? ds;
  @override
  void initState() {
    getData();
    super.initState();
  }

  getData() async {
    ds = await CallBackProducts().getAll();
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Page'),
      ),
      body: ds == null
          ? const Center(
              child: CircularProgressIndicator(),
            )
          : ListView.builder(
              itemCount: ds!.products!.length,
              itemBuilder: (context, index) {
                return ListTile(
                  title: Text(ds!.products![index].title!),
                  leading: CircleAvatar(
                    backgroundImage: NetworkImage(ds!.products![index].thumbnail!),
                  ),
                );
              },
            ),
    );
  }
}
```


## <a name="get"></a> GetX Implementation

To use GetX for state management, follow these steps:

### 1. Install Required Packages

Make sure to add the necessary packages to your `pubspec.yaml` file:

```yaml
dependencies:
  http:
  get:
  shared_preferences:
```

### 2. Example Code

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:myapp/api/util/get_callback.dart';

class GetxPage extends StatefulWidget {
  const GetxPage({super.key});

  @override
  State<GetxPage> createState() => _GetxPageState();
}

class _GetxPageState extends State<GetxPage> {
  @override
  void initState() {
    callBackGetx.productsController.getAll();
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Obx(
      () {
        var ds = callBackGetx.getAllModel.value;
        return Scaffold(
          appBar: AppBar(
            title: const Text('Home Page'),
          ),
          body: ds == null
              ? const Center(
                  child: CircularProgressIndicator(),
                )
              : ListView.builder(
                  itemCount: ds.products!.length,
                  itemBuilder: (context, index) {
                    return ListTile(
                      title: Text(ds.products![index].title!),
                      leading: CircleAvatar(
                        backgroundImage: NetworkImage(ds.products![index].thumbnail!),
                      ),
                      // subtitle: Text(ds.products![index].description!),
                    );
                  },
                ),
        );
      },
    );
  }
}
```

## <a name="provider"></a> Provider Implementation

To use Provider for state management, follow these steps:

### 1. Install Required Packages

Make sure to add the necessary packages to your `pubspec.yaml` file:

```yaml
dependencies:
  provider:
  http:
  shared_preferences:
```

### 2. Example Code

```dart
import 'package:flutter/material.dart';
import 'package:myapp/api/util/provider_callback.dart';

class ProviderPage extends StatefulWidget {
  const ProviderPage({super.key});

  @override
  State<ProviderPage> createState() => _ProviderPageState();
}

class _ProviderPageState extends State<ProviderPage> {
  @override
  void initState() {
    callBackProvider.productsProviderWithoutListener.getAll();
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    var ds = callBackProvider.getAllModel;
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Page'),
      ),
      body: ds == null
          ? const Center(
              child: CircularProgressIndicator(),
            )
          : ListView.builder(
              itemCount: ds.products!.length,
              itemBuilder: (context, index) {
                return ListTile(
                  title: Text(ds.products![index].title!),
                  leading: CircleAvatar(
                    backgroundImage: NetworkImage(ds.products![index].thumbnail!),
                  ),
                  // subtitle: Text(ds.products![index].description!),
                );
              },
            ),
    );
  }
}

```