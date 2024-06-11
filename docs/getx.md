# Using GetX

API Copilot provides seamless integration with GetX, a popular state management solution for Flutter. Here's how the folder structure looks like when using GetX:

## Folder Structure

- **model**: Contains data models that represent the structure of the data received from the API.
- **get_controller**: Manages the state of the application and business logic related to API interactions. This is used if you choose the GetX state management solution.
- **query**: Contains functions that handle HTTP requests to API endpoints.
- **util**: Contains utility functions for common tasks like HTTP requests, file handling, and global configurations.

### Util Folder (for GetX State Management)

#### get_callback_get_x.dart

- **Description**: This file defines the `GetCallBackGetx` class, which extends `CallBackGetx`. It provides a centralized access point for reactive variables (Rx) obtained from different GetX controllers.
- **Use Case**: It facilitates the retrieval of reactive variables from various GetX controllers, such as products, cart, and dummy API controllers.
- **Lazy Initialization**: It uses lazy initialization to ensure that only one instance of `GetCallBackGetx` is created.

#### get_callback.dart

- **Description**: This file defines the `CallBackGetx` class, serving as a base class for other GetX provider classes. It provides methods for initializing GetX controllers and accessing reactive variables.
- **Use Case**: It initializes GetX controllers and provides access to reactive variables using the GetX dependency injection mechanism.
- **Controller Initialization**: It initializes GetX controllers for managing state and API interactions.

#### get_x.dart

- **Description**: This file defines the `GetxApp` widget, which initializes GetX controllers and provides them to the widget tree.
- **Use Case**: It uses the GetX dependency injection mechanism to lazily initialize and register GetX controllers for managing state.
- **Controller Registration**: It registers GetX controllers using the `Get.lazyPut` method to ensure lazy initialization and efficient memory usage.


## Example Code
Example of `main.dart` file for GetX:

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:myapp/api/util/get_x.dart';
import 'package:myapp/get_x.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      initialRoute: '/',
      routes: {
        '/': (context) => const GetXPage(),
      },
    );
  }
}
```

In this code:

`GetxApp` is being used to initialize and provide controllers to the widget tree using the GetX state management solution. Let's break down its usage:

1. **Widget Initialization**: `GetxApp` is a `StatelessWidget` that wraps around the `MaterialApp` or `GetMaterialApp` widget. It takes a `child` widget as a parameter.

2. **Controller Initialization**: Inside the `build` method of `GetxApp`, controllers for managing state related to products, cart, and dummy API are lazily initialized using the `Get.lazyPut()` method from the GetX package. Lazily initializing controllers ensures that they are only created when they are first needed, optimizing memory usage.

3. **Builder Widget**: `GetxApp` returns a `Builder` widget as its child. This allows the controllers to be initialized before the child widget is built. The child widget will have access to the initialized controllers via the GetX dependency injection system.

By using `GetxApp`, controllers are initialized at the top of the widget tree and are made available to all child widgets. This ensures that the state management logic provided by these controllers can be accessed and utilized throughout the application easily.


## Fetching and Displaying Data from API Using GetX

1. **Initialize State**

```dart
// This method is invoked when the widget is first inserted into the widget tree.
@override
void initState() {
  // Call the getAll() method of productsController to fetch all products from the API.
  callBackGetx.productsController.getAll();
  super.initState();
}
```

1. **Declare Variable to Hold Data**

```dart
// Obx widget listens to changes in getAllModel and rebuilds the UI accordingly.
return Obx(
  () {
    // Retrieve the value of getAllModel from callBackGetx.
    var ds = callBackGetx.getAllModel.value;

```

1. **Conditional Rendering**

```dart
    ds == null
      // If data is not yet fetched, display a CircularProgressIndicator.
      ? const Center(
          child: CircularProgressIndicator(),
        )
      // Once data is fetched, display a ListView containing ListTile widgets for each product.
      : ListView.builder(
          itemCount: ds.products!.length,
          itemBuilder: (context, index) {
            return ListTile(
              // Display the title of the product.
              title: Text(ds.products![index].title!),
              // Display the thumbnail image of the product.
              leading: CircleAvatar(
                backgroundImage: NetworkImage(ds.products![index].thumbnail!),
              ),
              // Optionally, you can display the product description as well.
              // subtitle: Text(ds.products![index].description!),
            );
          },
        );
  },
);


```

Explanation:

- **InitState:**
  This segment of code triggers the retrieval of data from the API immediately upon the widget's initialization. It initiates the process by executing `callBackGetx.productsController.getAll()`.

- **Variable Declaration:**
  Here, the variable `ds` is initialized to hold the value retrieved from `callBackGetx.getAllModel.value`. This value represents the data fetched from the API.

- **Conditional Rendering:**
  The conditional rendering logic determines whether to display a loading indicator or the list of products based on the availability of data (`ds`). If `ds` is `null`, indicating that data has not been fetched yet, a circular progress indicator is shown. Otherwise, the list of products is displayed.

- **ListView.builder:**
  This section efficiently constructs a list of products using the `ListView.builder` widget. It dynamically creates list items on demand, based on the length of the `ds.products` list. Each list item is represented by a `ListTile` widget, displaying the title and thumbnail image of the product. Optionally, the product description can also be displayed.



### Full Source Code Here

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

