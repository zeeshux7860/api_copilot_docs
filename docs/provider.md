# Using Provider

API Copilot seamlessly integrates with the Provider package, a popular state management solution for Flutter. Here's how the folder structure looks like when using Provider:

## Folder Structure

- **model**: Contains data models that represent the structure of the data received from the API.
- **provider**: Manages the state of the application and business logic related to API interactions. This is used if you choose the Provider state management solution.
- **query**: Contains functions that handle HTTP requests to API endpoints.
- **util**: Contains utility functions for common tasks like HTTP requests, file handling, and global configurations.

### Util Folder

#### get_callback_provider.dart

- **Description**: This file defines the `GetCallBackProvider` class, which extends `CallBackProvider`. It provides a centralized access point for data models obtained from different providers.
- **Use Case**: It facilitates the retrieval of data models from various providers, such as products, cart, and dummy API providers.
- **Lazy Initialization**: It uses lazy initialization to ensure that only one instance of `GetCallBackProvider` is created.

#### navigator_observer.dart

- **Description**: This file defines the `NavigationObserver` class, which extends `NavigatorObserver`. It observes navigation events in the app and initializes the context for providers accordingly.
- **Use Case**: It handles navigation events and ensures that providers have access to the correct context during navigation.
- **Navigation Methods**: It overrides methods like `didPop`, `didPush`, `didRemove`, `didReplace`, and `didStartUserGesture` to manage navigation events effectively.

#### provider_callback.dart

- **Description**: This file defines the `CallBackProvider` class, serving as a base class for other provider classes. It provides methods for initializing the context and accessing provider instances.
- **Use Case**: It initializes the context for providers and provides access to provider instances with and without listening for changes.
- **Context Initialization**: It initializes the context for providers to ensure proper access to provider instances.

#### provider.dart

- **Description**: This file defines the `ProviderApp` widget, which initializes and provides various providers to the widget tree.
- **Use Case**: It combines multiple providers using the `MultiProvider` widget for managing the state of products, the cart, and a dummy API.
- **Provider Creation**: It uses the `ChangeNotifierProvider` to create instances of provider classes for managing state.


## Example Code

Example of `main.dart` file for Provider:

```dart
import 'package:flutter/material.dart';
import 'package:myapp/api/util/navigator_observer.dart';
import 'package:myapp/api/util/provider.dart';
import 'package:myapp/provider.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ProviderApp(
      child: MaterialApp(
        navigatorObservers: [NavigationObserver()],
        initialRoute: '/',
        routes: {
          '/': (context) => const ProviderPage(),
        },
      ),
    );
  }
}
```


In this code:

1. **ProviderApp**: The `ProviderApp` widget is used as the root widget of the application. It wraps the `MaterialApp` widget to provide access to providers throughout the widget tree. By encapsulating the entire application with `ProviderApp`, it ensures that the state management provided by the providers is accessible to all widgets within the app.

2. **NavigationObserver**: The `NavigationObserver` is added to the `navigatorObservers` list of the `MaterialApp`. This observer listens to navigation events in the app, such as when screens are pushed, popped, or replaced. It ensures that the context for the providers is initialized correctly during navigation, allowing widgets to access the application's state even when navigating between screens.

By using `ProviderApp` and `NavigationObserver` together, the application sets up its state management infrastructure and ensures that the state remains consistent throughout the navigation flow. This promotes a clean and organized approach to managing state and handling navigation events in the app.


## Fetching and Displaying Data from API Using Provider


1. **Initialize State**
```dart
@override
void initState() {
  // Fetch data from API when widget initializes
  callBackProvider.productsProviderWithoutListener.getAll();
  super.initState();
}
```

1. **Declare Variable to Hold Data**

```dart
var ds = callBackProvider.getAllModel;
```

1. **Conditional Rendering**

```dart
// If data is null, show loading indicator; else, display product list
ds == null
  ? const Center(child: CircularProgressIndicator())
  : ListView.builder(
      itemCount: ds.products!.length,
      itemBuilder: (context, index) {
        // Build each product item using ListTile
        return ListTile(
          title: Text(ds.products![index].title!),
          leading: CircleAvatar(
            // Display product thumbnail using NetworkImage
            backgroundImage: NetworkImage(ds.products![index].thumbnail!),
          ),
          // Uncomment to show product description as subtitle
          // subtitle: Text(ds.products![index].description!),
        );
      },
    );
```
Explanation:

- **InitState:** This code fetches data from the API as soon as the widget initializes, ensuring data retrieval starts immediately.
- **Variable Declaration:** `ds` holds the data retrieved from the API, allowing easy access to product information.
- **Conditional Rendering:** This code conditionally renders either a loading indicator or a list of products based on the availability of data. This informs the user about the loading process.
- **ListView.builder:** Efficiently displays the list of products fetched from the API. Each product is represented by a ListTile widget, showing its title and thumbnail image. Optionally, you can display the product description as well.



### Full Source Code Here

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

