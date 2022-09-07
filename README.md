# Dolby.io Communications SDK for Flutter - Getting Started

🚀 SDK Beta

The Flutter SDK is a part of the [Beta Program](https://docs.dolby.io/communications-apis/docs/overview-beta-programs).

This guide explains how to create a basic **audio-only** conference application for mobile devices using the Dolby.io Communications SDK for Flutter. This starter project provides the foundation upon which you can add additional features as you build out your own solutions for events, collaboration, education, or live streaming. 

> Currently, this guide explains how to create an audio-only conference application. Instructions for implementing video will be added in the future."

The application lets you create, join, and leave the conference with enabled audio. You can also see the participants list.

# Prerequisites

This project uses the Flutter SDK and requires a basic knowledge of Flutter. If you need help with Flutter development, see the [Flutter documentation](https://docs.flutter.dev/).

Make sure that you have:

- A Dolby.io account. If you do not have an account, you can [sign up](https://dolby.io/signup) for a free account.
- An iOS or Android device, or a device emulator
- Flutter SDK 3.0.3 or later configured to work on iOS and Android
- Android Studio for Android development
- Xcode and CocoaPods for iOS development
- The [client access token](https://docs.dolby.io/communications-apis/docs/overview-developer-tools#client-access-token) copied from the [Dolby.io dashboard](https://dashboard.dolby.io/). To create the token, log in to the Dolby.io dashboard, create an application and navigate to the API keys section.

To check if there are any additional dependencies that you need to install to complete the setup, use the flutter doctor ([macOS](https://docs.flutter.dev/get-started/install/macos#run-flutter-doctor), [Windows](https://docs.flutter.dev/get-started/install/windows#run-flutter-doctor)) command.

For more information about iOS SDK and Android SDK requirements, see the [Supported Environments](https://docs.dolby.io/communications-apis/docs/overview-supported-environments) document.

# Build the application

Each step in this guide focuses on an important component of a basic **audio-only** conference application. You can either follow along by starting from the boiler plate code in the root of the project or start with the final full source code listing and read the explanations below to understand how it works.

## 0. Create a new project

> Step 0 - Create a new project and install the dependencies
> * Create a new Flutter project
> * Add the required package dependencies
> * Download and install the dependencies

Create a new Flutter project using the following command:

```shell
flutter create dolbyio_getting_started
```

After a few minutes, you should have a template and installed components needed for the project to run. Once the project is created, go to the newly created folder:

```shell
cd dolbyio_getting_started
```

Add the following package dependencies to the project:
- Dolby.io Communications SDK for Flutter
- The **permission_handler** that allows requesting all required permissions for iOS and Android

```shell
flutter pub add dolbyio_comms_sdk_flutter permission_handler
```

Download and install all dependencies, including iOS and Android SDK binaries:

```shell
flutter packages get
```

### Additional configuration for Android

The Dolby.io Communications SDK for Flutter is compatible only with Android 21+. To set the Android version, add the following code to the `android/app/build.gradle` file:

```gradle
android {
    compileSdkVersion 33
    defaultConfig {
        minSdkVersion 21
    }
}
```

### Additional configuration for iOS

On iOS, the minimum deployment version needs to be set to iOS 12. You can check the version by opening the `ios/Podfile` file and making sure that it contains the following line:

```
platform :ios, '12.0'
```

Additionally, disable the **bitcode** support in the same file by updating the last block of code with the following:

```
post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)

    # Disable bitcode
    target.build_configurations.each do |config|
      config.build_settings['ENABLE_BITCODE'] = 'NO'
    end
  end
end
```

## 1. Add a basic layout

> Step 1 - Replace the default layout
>
> Replace the content of the **lib/main.dart** file with a basic layout for a conference application.

Open the `lib/main.dart` file in your favorite text editor and replace its content with the following code:

```dart
// Step 2: Import the permission handler package
// Step 3: Import the Dolby.io Communications SDK for Flutter
import 'package:flutter/material.dart';
 
import 'dart:math';
import 'dart:core';
import 'dart:developer' as developer;
 
void main() {
  runApp(const MaterialApp(home: FlutterScreen()));
}
 
class FlutterScreen extends StatefulWidget {
  const FlutterScreen({Key? key}) : super(key: key);
 
  @override
  State<FlutterScreen> createState() => _FlutterScreenState();
}
 
class _FlutterScreenState extends State<FlutterScreen> {
  // Step 3: Instantiate the SDK here
 
  TextEditingController usernameController = TextEditingController();
  TextEditingController conferenceNameController = TextEditingController();
 
  // Generate a client access token from the Dolby.io dashboard
  // and insert the token to the accessToken variable
  String accessToken = '';
 
  bool isLeaving = false;
  bool isJoining = false;
  bool isInitializedList = false;
 
  // Step 7: Store the participants list here
 
  @override
  void initState() {
    super.initState();

    // Step 2: Request the microphone and camera permissions
 
    // Step 3: Call initializeSdk()

    // Step 4: Call openSession()
 
    // Step 7: Call updateParticipantsList()
  }
 
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Flutter SDK'),
        centerTitle: true,
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            if (!isInitializedList) ...[
              Expanded(
                  flex: 2,
                  child: SingleChildScrollView(
                    child: Column(
                      children: [
                        TextField(
                          controller: usernameController,
                          readOnly: true,
                        ),
                        const SizedBox(height: 12),
                        TextField(
                          decoration: const InputDecoration(hintText: 'Conference name'),
                          controller: conferenceNameController,
                        ),
                        const SizedBox(height: 12),
                        ElevatedButton(
                          onPressed: () async {
                            // Step 5: Call joinConference()
                          },
                          child: isJoining
                            ? const Text('Joining...')
                            : const Text('Join the conference'),
                        ),
                      ],
                    ),
                  )),
              const Divider(
                thickness: 2,
              ),
            ],
            Expanded(
                flex: 5,
                child: isInitializedList
                    ? Column(children: [
                        Text(
                          'Conference name: ${conferenceNameController.text}',
                          style: const TextStyle(
                              fontWeight: FontWeight.w400, fontSize: 16),
                        ),
                        const SizedBox(
                          height: 16,
                        ),
                        const Align(
                            alignment: Alignment.centerLeft,
                            child: Text(
                              'List of participants:',
                              style: TextStyle(
                                  color: Colors.blue,
                                  fontWeight: FontWeight.w600),
                            )),
                        const SizedBox(
                          height: 16,
                        ),

                        // Step 7: Display the list of participants

                        const SizedBox(
                          height: 12,
                        ),
                        ElevatedButton(
                          style: ElevatedButton.styleFrom(primary: Colors.red),
                          onPressed: () async {
                            // Step 6: Call leaveConference()
                          },
                          child: isLeaving
                            ? const Text('Leaving...')
                            : const Text('Leave the conference'),
                        )
                      ])
                    : const Center(child: Text("Join the conference to see the list of participants.")))
          ],
        ),
      ),
    );
  }
 
  // Step 3: Define the initializeSdk function

  // Step 4: Define the openSession function

  // Step 5: Define the joinConference function

  // Step 6: Define the leaveConference function

  // Step 7: Define the updateParticipantsList method
}
```

## 2. Request permissions

> Step 2 - Import the permission_handler package and request permissions
> * Request permissions for a microphone and camera when the application loads
> * Inform end users of iOS devices that your application requires access to a microphone and camera

The application must request permissions to access the device's microphone and camera. To request all required permissions on iOS and Android, you need to add additional code to the `lib/main.dart` file.

First, add the following import statement at the top of the file:

```dart
// Step 2: Import the permission handler package
import 'package:permission_handler/permission_handler.dart';
```

Then, request permissions by adding the following code in the **initState()** function:

```dart
// Step 2: Request the microphone and camera permissions
[
    Permission.bluetoothConnect,
    Permission.microphone,
    Permission.camera,
].request();
```

At this point, when you run the application for the first time, you will receive a pop-up window asking you to grant permissions for the microphone and camera to use them in the conference.

### Additional iOS permissions

On iOS, establish privacy permissions by adding the following keys to the `ios/Runner/Info.plist` file:

- Privacy - Microphone Usage Description
- Privacy - Camera Usage Description

Additionally, make sure that the bundle name pointed by `Runner.xcodeproj` is correct.

## 3. Initialize the SDK

> Step 3 - Initialize the SDK
> * Import the Dolby.io Communications SDK for Flutter
> * Create an instance of the SDK
> * Initialize the SDK with a secure authentication method

In order to use the conferencing functionalities of the SDK, you need to include the Dolby.io Communications SDK for Flutter in your application. To do this, add the following code at the beginning of the `lib/main.dart` file:

```dart
// Step 3: Import the Dolby.io Communications SDK for Flutter
import 'package:dolbyio_comms_sdk_flutter/dolbyio_comms_sdk_flutter.dart';
```

Create an instance of the SDK at the beginning of the **_FlutterScreenState** class:

```dart
// Step 3: Instantiate the SDK here
final dolbyioCommsSdk = DolbyioCommsSdk.instance;
```

Initialize the SDK using the secure authentication method that uses a token in the application. For more information, see the [Initializing the SDK document](doc:initializing-javascript). For the purpose of this sample application, use a [client access token](doc:overview-developer-tools#client-access-token) generated from the Dolby.io dashboard.

To initialize the SDK, add the following code before the **joinConference** method:

```dart
// Step 3: Define the initializeSdk function
Future<void> initializeSdk() async {
  // Initialize the SDK with the access token
  await dolbyioCommsSdk.initializeToken(accessToken, () async {
    // Request a new access token here
    return accessToken;
  });
}
```

Call the **initializeSdk** function by adding the following code in the **initState** method:

```dart
// Step 3: Call initializeSdk()
initializeSdk();
```

## 4. Open a session

> Step 4 - Open a session to connect your application with the Dolby.io backend
>
>Add the **openSession()** method to your application and call it from the **initState()** block.

A **session** is a connection between the client application and the Dolby.io backend. When opening a session, you should provide a **name**. Commonly, this is the name of the participant who established the session. In this example, we use a random name.

Add the following code after the implementation of the **initializeSdk** function:

```dart
// Step 4: Define the openSession function
Future<void> openSession() async {
  // Generate a random username
  int randomNumber = Random().nextInt(1000);
  usernameController.text = "user-$randomNumber";
 
  // Open a new session for the local participant
  var participantInfo = ParticipantInfo(usernameController.text, null, null);
  await dolbyioCommsSdk.session.open(participantInfo);
}
```

Call the created function by adding the following code after initializing the SDK in the **initState()** method.

```dart
// Step 4: Call openSession()
openSession();
```

## 5. Create and join a conference

> Step 5 - Use the joinConference() method
> 
> Add the **joinConference()** method to your application and call it whenever a user clicks the **Join the conference button**.

A conference is a multi-person call where participants exchange audio with one another. To distinguish between multiple conferences, you should assign a conference alias or name. When multiple participants join a conference of the same name using a token for the same Dolby.io application, they will be able to meet in a call. 

Locate the **joinConference** function and add the following implementation to the function to allow creating and joining conferences:

```dart
// Step 5: Define the joinConference function
Future<void> joinConference() async {
  setState(() => isJoining = true);

  // Create conference options
  var params = ConferenceCreateParameters();
  params.dolbyVoice = true;
  var createOptions = ConferenceCreateOption(conferenceNameController.text, params, 0);

  // Join the conference with audio and video
  var joinOptions = ConferenceJoinOptions();
  joinOptions.constraints = ConferenceConstraints(true, true);

  // Join the conference
  var conference = await dolbyioCommsSdk.conference.create(createOptions);
  conference = await dolbyioCommsSdk.conference.join(conference, joinOptions);

  // Check the conference status
  if (conference.status == ConferenceStatus.joined) {
    setState(() => isJoining = false);
    developer.log('Joined to conference.');
  } else {
    developer.log('Cannot join to conference.');
  }
}
```

Call the created function by adding the following code on the callback of the **Join the conference** button.

```dart
// Step 5: Call joinConference()
await joinConference();
```

## 6. Leave the conference

> Step 6 - Use the leaveConference() method
>
> Add the **leaveConference()** method to your application and call it whenever a user clicks the **Leave the conference button**.

To leave the conference, locate the **leave** function and add the following implementation to the function:

```dart
// Step 6: Define the leaveConference function
Future<void> leaveConference() async {
  setState(() => isLeaving = true);

  await dolbyioCommsSdk.conference.leave(options: null);
  
  setState(() => isInitializedList = true);
  setState(() => isLeaving = true);
}
```

Call the created function by adding the following code on the callback of the **Leave the conference** button.

```dart
// Step 6: Call leaveConference()
await leaveConference();
```

## 7. Display the participants list

> Step 7 - Add the participants list to the layout
> * Create a variable for storing the list of participants
> * Add the participants list to the layout
> * Listen to the changes on the list of participants using the **updateParticipantsList()** method

First, create a variable in the **_FlutterScreenState** class for storing the list of participants.

```dart
// Step 7: Store the list of participants
List<Participant> participants = [];
```

Then, define how you want to present the list of participants in the UI by adding the following code to the **build** function:

```dart
// Step 7: Display the list of participants
Material(
  elevation: 8,
  child: ListView.builder(
    shrinkWrap: true,
    itemCount: participants.length,
    itemBuilder: (context, index) {
      return ListTile(
        title: Text("${participants[index].info!.name} (${participants[index].status?.name})"),
        leading: const Icon(
          Icons.person,
          color: Colors.black,
        ),
      );
    },
  ),
),
```

After joining a conference, the application must listen to changes on the list of participants to update the UI. To update the participants list, define the **updateParticipantsList** method at the end of the class:

```dart
// Step 7: Define the updateParticipantsList method
void updateParticipantsList() {
  dolbyioCommsSdk.conference.onParticipantsChange()
    .listen((params) async {
      try {
        var conference = await dolbyioCommsSdk.conference.current();
        var participantsList = await dolbyioCommsSdk.conference.getParticipants(conference);
        
        setState(() => participants = participantsList);
        setState(() => isInitializedList = true);
      } catch (error) {
        developer.log("Error during initializing participant list.", error: error);
      }
    });
}
```

Call the created function by adding the following code after opening the session, in the **initState()** method.

```dart
// Step 7: Call updateParticipantsList()
updateParticipantsList();
```

At this stage, your basic application should be ready. You should be able to join a conference, see participants and their statuses, and leave the conference.

# Run the application

Search for the **String accessToken** variable at the beginning of the **_FlutterScreenState** class and copy the **client access token** that you copied from the Dolby.io dashboard.

Make sure that your device is available:

```shell
flutter devices
```

Run your application:

```shell
flutter run
```
