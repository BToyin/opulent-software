---
title: Setting up Firebase for your Flutter project (2025)
date: 2025-03-21T05:39:00.000Z
author: Toyin
image: /images/blog/download.jpeg
type: blog
---
To provide a little bit of context, Firebase is a cross platform google solution that offers a range of services that can be used to build out a flutter app’s backend functionality such as file storage, authentication and databases and more.

In this post I’ll be running you through the steps to adding firebase as a backend for your flutter app and resolving some common issues.

As prerequisites you will require the following:

* flutter app to add firebase to + Flutter SDK installed on machine
* A google account to create firebase project.

#### **Creating a Firebase Project:**

Firstly, go [here](https://firebase.google.com/) and ‘Get Started’. Follow steps to sign in and create a new firebase project. You can name this whatever your flutter app is named. Follow the setup steps, allowing the defaults and create your project. 

#### **Adding Firebase to your Flutter App:**

After creating the firebase project, you will need to add that firebase project as the backend for your flutter app. On Firebase Console, click Flutter icon for adding an App

* The first step is to install the Firebase CLI. Run the command below:

```
curl -sL https://firebase.tools | bash
```

* Now run `firebase login` - this should open a browser window to log into your google account. 

Since you should already have your flutter app and SDK, go to Step 2 of the set up.

At the root of your flutter project, run the 2 provided commands. These basically link your flutter project with your firebase project and enable the use of several platforms.

**N.B**: You will need to rerun `flutterfire configure` any time you add a new firebase service or product to your flutter app to ensure everything stays up to date (see below).

#### Initialising Firebase in your app

1. You need to add the firebase core package so run: `flutter pub add firebase_core`
2. Because you’ve added a new firebase service, you need to run `flutterfire configure` again. 
3. now in `lib/main.dart` you can initialise firebase by adding the below imports and amending your main method to look like the below:

```
// Add these imports to top of file
import 'package:firebase_core/firebase_core.dart';
import 'firebase_options.dart';

.
.
void main() async {
  WidgetsFlutterBinding.ensureInitialized();

  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );

  runApp(const MyApp());
}
```

* Add the `ensureInitialized()` method as the first line in your main method followed by the firebase `Firebase.initializeApp()` method. **Add these in this order or the app will not run.**
* Make your main method `async`

With these things in place, you should be able to just run your flutter app.

#### **Dealing with Errors when running Flutter app**

After running through the initialisation steps, It’s probably a good idea to just run your flutter app on the available devices to make sure everything still works. Hopefully you get no errors but a common one you may come across when attempting to run on MacOs or IOS (iphone simulator) is:

`Error: The pod "Firebase/CoreOnly" required by the plugin "firebase_core" requires a higher minimum iOS deployment version than the plugin's reported minimum version.
To build, remove the plugin "firebase_core", or contact the plugin's developers for assistance.
Error running pod install`

* Basically the crux here is that your ios runner version is lower than that required by firebase to run.

Just a couple of steps is needed to resolve this (from my experience)

1. in the root of your project, delete the` /ios/Podfile.lock`
2. Then in `/ios/Podfile `change the target platform `13.0 `- If it’s commented out then uncomment and change to `13.0`. 
3. Now terminal, cd into `/ios` and run `pod install --repo-update`
4. This should resolve this error.

**N.B:** You may come across a similar error when trying to run on MacOS platform. Follow the exact same steps as above (just use the `/macos` directory instead) and update the target platform to `10.14`

The minimum required platform version for each dependency can be found under their respective json files here: `ios(or macos)/Pods/Local Podspecs`

* Look at each Json file and see the min version, then change your platform inside the podfile to highest of those min versions. 

After initialising firebase and running your app with no errors on different platforms, you can begin to add plugins for the firebase services that you want to use. This is a simple matter of adding the dependency for that service and re-running `flutterfire configure.` Follow the documentation for each plugin and APIs to implement its features.

I hope this is helpful in cutting through some of the documentation and simplifying things.

Peace and Blessings

Toy.
