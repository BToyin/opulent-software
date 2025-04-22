---
title: "Flutter: Bottom App Bar with Riverpod Code Generation"
date: 2025-04-22T20:07:00.000Z
author: Toyin A.
image: /images/blog/hqdefault.jpg
type: blog
---
Every app should have some sort of bottom app bar. These should generally be very simple and take the user to where they need to go based on clicking on the icons you present to them.

In this article, I walk you through a very simple example of how to add riverpod state management to your flutter bottom app bar so that you may maintain separation between the business logic (state management) and the UI presentation. This tutorial uses riverpod code generation as this is the recommended way of creating providers.

Firstly, I will assume here that you already have your flutter app initialised already and project ready to go. To further get things moving along with this article, I will also assume you have initialised riverpod and downloaded the required dependencies. Please follow the steps outlined here to do so:

[https://riverpod.dev/docs/introduction/getting_started ](https://riverpod.dev/docs/introduction/getting_started)

[](https://riverpod.dev/docs/introduction/getting_started)After following the above article, you should have the following:

* installed the riverpod dependencies
* Wrapped your application in ProviderScope to access providers and state

Now, i will insist that you create a separate widget class for your bottom AppBar so that you can reuse it across your app.

As such, your `main.dart` file should look something like:

```
void main() => runApp(
    ProviderScope(
      child: MyApp(),
    )
);

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
	    home: Scaffold(
	      body: {// your widget body},
	      bottomNavigationBar: BottomNavBar(),
	    ),
		 );
  }
}
```

What you should take notice of here is the BottomNavBar widget we will create as a separate widget to hold our state. Here, we will employ Riverpod. So let’s get right to that.

Let’s create a new dart file called bottom_nav_bar.dart and create the widget.

```
class BottomNavBar extends ConsumerWidget {
  const BottomNavBar({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final currentIndex = ref.watch(navIndexProvider);
    final navNotifier = ref.read(navIndexProvider.notifier);

    return BottomAppBar(
      child: Row(
        children: [
          IconButton(
            icon: const Icon(Icons.home),
            color: currentIndex == 0 ? Colors.blue : Colors.grey,
            onPressed: () => navNotifier.updateIndex(0),
          ),
          IconButton(
            icon: const Icon(Icons.account_circle_outlined),
            color: currentIndex == 1 ? Colors.blue : Colors.grey,
            onPressed: () => navNotifier.updateIndex(1),
          ),
          IconButton(
            icon: const Icon(Icons.settings),
            color: currentIndex == 2 ? Colors.blue : Colors.grey,
            onPressed: () => navNotifier.updateIndex(2),
          ),
        ],
      ),
    );
  }
}
```

* The BottomNavBar widget extends `ConsumerWidget` (rather than StatelessWidget) provided by riverpod
* The build method now takes `Widget ref` object courtesy of riverpod. This ref object gives us access to the providers we create.
* We create 2 variables at the top of the build method.

  * The first is to watch the provider so that we can update our state
  * The 2nd is the notifier object, which basically just exists to give us access to the functions we create inside our provider class

Your editor should still give you several errors at this point because we haven’t created our provider yet. So let’s do that now. Let’s create the provider separately so we can maintain separation of concerns (good architecture yay).

* Create a new directory called providers. 
* Inside the directory, create the provider dart file called `nav_index_provider.dart`. 

This should be as below:

```
import 'package:riverpod_annotation/riverpod_annotation.dart';

part 'nav_index_provider.g.dart';

@riverpod
class NavIndex extends _$NavIndex {
  @override
  int build(){ return 0;} // Initial index

  void updateIndex(int newIndex) => state = newIndex;
}
```

* Note the `‘part 'nav_index_provider.g.dart';` at the top of the import file. This is required for the code generation to create the provider file for us. 
* The class is annotated with `@riverpod` which marks the class as a provider and therefore will generate the provider class for us
* Make the provider class extend from the same `$_ClassName`
* We initialise the index inside the build method by returning 0
* Then create the `updateIndex` function that is used to update the state

You should still observe some compile errors thrown by your code editor. This is because we are not yet running the code generator that will create the provider class for us. We do this by running `dart run build_runner watch -d` . After running this, you should find a new file being created for you called `nav_index_provider.g.dart`.

Now if you head back to your BottomNavBar class, you should find that all compile errors are resolved. This is because the provider now exists (since the class has been generated) and our function `updateIndex()` has also been created.

Now we can go back to our main method to ensure that our app body updates whenever the user clicks on a tab. We can do this simply by updating our app body content depending on the currentIndex. Here, you should create separate widgets for each of the different screens that should be accessible via the bottom nav bar, put them in a list and assign the appropriate index to them and then make some final updates to the main.dart file. Let’s take it step by step.

Create a new file called `home_screen.dart`. Inside this file, you will return the body for your home screen. For the sake of this tutorial this will be as simple as below:

```

import 'package:flutter/material.dart';

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.blue, // brand color
      body: Center(
        child: Text('HOME SCREEN'),
      ),
    );
  }
}
```

* Do the same for other screens such as `profile_screen` and `settings_screen`. Make them distinguishable by altering the text i.e. ‘SETTINGS SCREEN’ etc

You should now have 3 more dart files, home_screen.dart, profile_screen.dart, settings_screen.dart. You can create as many screens as you’d like.

Now we can make changes to the main.dart file to toggle between these screens and use riverpod to do this.

```
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:practice_app/bottom_nav_bar.dart';
import 'package:practice_app/home_screen.dart';
import 'package:practice_app/profile_screen.dart';
import 'package:practice_app/providers/nav_index_provider.dart';
import 'package:practice_app/settings_screen.dart';

void main() => runApp(
    ProviderScope(
      child: Home(),
    )
);

class MyyApp extends ConsumerWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {

    final bodies = [
      HomeScreen(),
      ProfileScreen(),
      SettingsScreen(),
    ];

    final bottomNavIndex = ref.watch(navIndexProvider);

    return MaterialApp(
      home: Scaffold(
        body: bodies[bottomNavIndex],
        bottomNavigationBar: BottomNavBar(),
      ),
    );
  }
}
```

* We extend `ConsumerWidget` here now. This is because we want to watch our navIndexProvider just like we did in our BottomNavBar widget.
* We create a bodies array to hold the different screen widgets we’ve created
* We also hold the bottomNavIndex in a variable by simple watching the provider. Since the provider returns an Integer, it’ll serve to access the different items in our bodies array.
* Now in our Scaffold, we just called bodies\[index]

And voila. The bottom nav bar should be working perfectly. Remember that we initialised our navIndexProvider with 0 which means our app will always boot up and show the `HomeScreen()`. 

I hope this has been somewhat useful. Please feel free to get in touch with any questions. 

Peace and Blessing,

Toyin
