# Using Default

When using the default option with API Copilot, the folder structure and `main.dart` file remain unchanged from your existing project setup. Here's how the typical folder structure looks like:

## Folder Structure

- **model**: Contains data models that represent the structure of the data received from the API.
- **callback**: The files inside the callback folder demonstrate a use case where API calls are handled using a callback mechanism. These files provide a method to fetch data from APIs and convert them into model objects in a Flutter application.
- **query**: Contains functions that handle HTTP requests to API endpoints.
- **util**: Contains utility functions for common tasks like HTTP requests, file handling, and global configurations.

### Callback Folder (for Without State Management)

The files inside the callback folder demonstrate a use case where API calls are handled using a callback mechanism. These files provide a method to fetch data from APIs and convert them into model objects in a Flutter application.

The `CallBackProducts` class provided as an example contains async functions that fetch data from API endpoints. These functions include `getAll`, `singleProduct`, and `searchProducts`, mainly intended for fetching All Products, Single Product, and searching Products, respectively.

The use case of this example is to demonstrate the usage of a callback mechanism when retrieving data from a server. Callback functions are asynchronous, allowing API calls to be executed asynchronously, and when a response is received, it can be processed accordingly.

This file encapsulates the logic for API calls, making the code cleaner and more maintainable. Additionally, it is ready to handle errors so that if any exceptions occur, they can be properly handled.


## Example Code

Example of `main.dart` file for Default:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return  MaterialApp(
        navigatorObservers: [NavigationObserver()],
        initialRoute: '/',
        routes: {
          '/': (context) => const ProviderPage(),
        },
    );
  }
}
```


## Fetching and Displaying Data from API Using Provider


1. **Initialize State**
```dart
@override
void initState() {
  getData(); 
  super.initState();
}
```

1. **Declare Variable to Hold Data**

```dart
GetAllModel? ds;
```

3. **Conditional Rendering**

```dart
// Conditional rendering based on the availability of data
ds == null
  // If data is null, display a loading indicator
  ? const Center(
      child: CircularProgressIndicator(),
    )
  // If data is available, display a ListView of products
  : ListView.builder(
      itemCount: ds!.products!.length,
      itemBuilder: (context, index) {
        // Construct ListTile for each product
        return ListTile(
          title: Text(ds!.products![index].title!),
          leading: CircleAvatar(
            backgroundImage: NetworkImage(ds!.products![index].thumbnail!),
          ),
        );
      },
    );

```

Explanation:

- **Stateful Widget:**
The `WithoutStateManagement` widget is a stateful widget responsible for managing its state and UI rendering.
- **InitState:**
  In the `initState` method, the `getData` function is called to fetch data from the API as soon as the widget is inserted into the widget tree.

- **getData Function:**
  The `getData` function asynchronously fetches data from the API using the `CallBackProducts().getAll()` method. Once the data is retrieved, it updates the state (`ds`) using `setState`, triggering a rebuild of the widget to reflect the updated data.

- **ListView.builder:**
  The `ListView.builder` widget efficiently constructs a list of products based on the length of the `ds.products` list. Each product is represented by a `ListTile` widget displaying its title and thumbnail image.

- **Use Case:**
  This implementation is suitable for scenarios where data fetching and state management are handled within the widget itself. However, it may lead to code duplication and decreased maintainability as similar state management logic needs to be implemented across multiple widgets.

### Full Source Code Here

```dart
import 'package:flutter/material.dart';
import 'package:myapp/api/callback/products.dart';
import 'package:myapp/api/model/products/get_all.dart';

class WithoutStateMangement extends StatefulWidget {
  const WithoutStateMangement({super.key});

  @override
  State<WithoutStateMangement> createState() => _WithoutStateMangementState();
}

class _WithoutStateMangementState extends State<WithoutStateMangement> {
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
