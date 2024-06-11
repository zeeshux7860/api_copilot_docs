# How to Work with API Copilot

API Copilot simplifies the process of integrating APIs into your Flutter application. Here's a detailed guide on how to use API Copilot to generate the necessary code.

## Step-by-Step Guide

### 1. Open API Copilot

Start by opening the API Copilot tool.

### 2. Import Postman Collection

You will need to import your Postman collection to generate the necessary code.

- **Note**: Ensure that CORS (Cross-Origin Resource Sharing) is enabled for your API if required. This is especially important if you are running the API on localhost. Failure to enable CORS can result in issues when making API requests.

### 3. Select Postman Collection

After importing your Postman collection, proceed with the following steps:

1. **Enter Flutter Project Name**: 
   - You will see a text field to enter the name of your Flutter project.

2. **Specify Backend Integration Code Location**: 
   - By default, the generated code is placed in the `lib` folder.
   - If you prefer to place the code in a specific subfolder, enter the folder name. For example, entering `api` will place the code in `lib/api`.

3. **Choose State Management Solution**: 
   - You will have a dropdown menu with three options:
     - **Default**: Generates code without any state management.
     - **Provider**: Generates code using the Provider state management solution.
     - **GetX**: Generates code using the GetX state management solution.

### 4. Generate Code

Once you have entered the necessary details:

1. **Click the "Generate Code" Button**: 
   - API Copilot will verify your requests to ensure they are correct.
   - The code generation process will start if everything is verified successfully.

2. **Download Generated Code**: 
   - The generated code will be downloaded automatically as a `.zip` file.

### 5. Extract and Integrate Code

1. **Extract the .zip File**: 
   - Extract the contents of the `.zip` file.
   
2. **Paste the Code into Your Flutter Project**: 
   - Copy the extracted files and paste them into the `lib` folder of your Flutter project, or the specific folder you designated earlier.

### 6. Review and Modify Code

- Navigate to the designated folder (e.g., `lib/api`) to see the generated code.
- You can modify the generated code if necessary to suit your application requirements.

## Setup

Before you start using API Copilot, you need to install the required packages depending on your state management choice:

### Using GetX for State Management

```bash
flutter pub add http
flutter pub add get
flutter pub add shared_preferences
```


### Using Provider for State Management

```bash
flutter pub add provider
flutter pub add http
flutter pub add shared_preferences
```


### Using Provider Without State Management

```bash
flutter pub add http
flutter pub add shared_preferences
```

