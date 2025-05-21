
When changing orientation we see large layout shifts occurring in the new
archicture. Upon close investigation we isolated the problem to be related to
react-native-screen's Screen component. 

https://github.com/user-attachments/assets/21db04b3-357e-4445-bed8-df640023ea5f

Note, while in this reproducible this is fast. In a large app with busy threads
the layout shift is way more prominent giving users a broken App sensation.

At first we thought this was particularly related to absolute layout positioning
but further investigation proved us wrong. 

We found that this onLayout updateState call [in FabricEnabledViewGroup
onLayout](https://github.com/software-mansion/react-native-screens/blob/f79fcae0a35d3f9db23d437c2354ede2ff1906b8/android/src/fabric/java/com/swmansion/rnscreens/FabricEnabledViewGroup.kt#L34-L63)
triggered by the [ScreenContainer's onLayout](https://github.com/software-mansion/react-native-screens/blob/f79fcae0a35d3f9db23d437c2354ede2ff1906b8/android/src/main/java/com/swmansion/rnscreens/ScreenContainer.kt#L46-L56) causes a layout change that is different from the layout represented in the Shadow Tree. 
In consequence it causes a large layout shift.


We don't understand yet why this updateState exists but removing it does seem to
fix things, see video below:

https://github.com/user-attachments/assets/ed54021c-bb3e-4388-bd5d-bfa3aa9d3b7b


The question now is:
1. Why does this updateState exist?
2. if we need to keep it how do we ensure possibly schedule the update to occur
   together with its children layout to avoid the layout shift? 
3. is this maybe just a problem of incorrect dimenisions here?
4. does Fabric now make this code unnecessary given I didn't find any bugs in
   our App by testing it without this logic?



This is a new [**React Native**](https://reactnative.dev) project, bootstrapped using [`@react-native-community/cli`](https://github.com/react-native-community/cli).

# Getting Started

> **Note**: Make sure you have completed the [Set Up Your Environment](https://reactnative.dev/docs/set-up-your-environment) guide before proceeding.

## Step 1: Start Metro

First, you will need to run **Metro**, the JavaScript build tool for React Native.

To start the Metro dev server, run the following command from the root of your React Native project:

```sh
# Using npm
npm start

# OR using Yarn
yarn start
```

## Step 2: Build and run your app

With Metro running, open a new terminal window/pane from the root of your React Native project, and use one of the following commands to build and run your Android or iOS app:

### Android

```sh
# Using npm
npm run android

# OR using Yarn
yarn android
```

### iOS

For iOS, remember to install CocoaPods dependencies (this only needs to be run on first clone or after updating native deps).

The first time you create a new project, run the Ruby bundler to install CocoaPods itself:

```sh
bundle install
```

Then, and every time you update your native dependencies, run:

```sh
bundle exec pod install
```

For more information, please visit [CocoaPods Getting Started guide](https://guides.cocoapods.org/using/getting-started.html).

```sh
# Using npm
npm run ios

# OR using Yarn
yarn ios
```

If everything is set up correctly, you should see your new app running in the Android Emulator, iOS Simulator, or your connected device.

This is one way to run your app — you can also build it directly from Android Studio or Xcode.

## Step 3: Modify your app

Now that you have successfully run the app, let's make changes!

Open `App.tsx` in your text editor of choice and make some changes. When you save, your app will automatically update and reflect these changes — this is powered by [Fast Refresh](https://reactnative.dev/docs/fast-refresh).

When you want to forcefully reload, for example to reset the state of your app, you can perform a full reload:

- **Android**: Press the <kbd>R</kbd> key twice or select **"Reload"** from the **Dev Menu**, accessed via <kbd>Ctrl</kbd> + <kbd>M</kbd> (Windows/Linux) or <kbd>Cmd ⌘</kbd> + <kbd>M</kbd> (macOS).
- **iOS**: Press <kbd>R</kbd> in iOS Simulator.

## Congratulations! :tada:

You've successfully run and modified your React Native App. :partying_face:

### Now what?

- If you want to add this new React Native code to an existing application, check out the [Integration guide](https://reactnative.dev/docs/integration-with-existing-apps).
- If you're curious to learn more about React Native, check out the [docs](https://reactnative.dev/docs/getting-started).

# Troubleshooting

If you're having issues getting the above steps to work, see the [Troubleshooting](https://reactnative.dev/docs/troubleshooting) page.

# Learn More

To learn more about React Native, take a look at the following resources:

- [React Native Website](https://reactnative.dev) - learn more about React Native.
- [Getting Started](https://reactnative.dev/docs/environment-setup) - an **overview** of React Native and how setup your environment.
- [Learn the Basics](https://reactnative.dev/docs/getting-started) - a **guided tour** of the React Native **basics**.
- [Blog](https://reactnative.dev/blog) - read the latest official React Native **Blog** posts.
- [`@facebook/react-native`](https://github.com/facebook/react-native) - the Open Source; GitHub **repository** for React Native.
