---
layout: post
title: ☕Cozy Flutter on the Linux Command Line☕
categories: Flutter App-Development
---

## Things to know before continuing
1. This installation guide was created using Manjaro-i3, so instructions should be identical for Arch users.
2. Instuctions for setting up a comfortable Android Studio is also included.
3. The editor I use is vim, so the pluggins I reference will be for that.

## Flutter Setup on the Terminal
A lot of this configuration is pulled from the [Flutter Instalation Guide](https://flutter.dev/docs/get-started/install/linux), so if there is anything that does not work here, be sure to reference the instalation guide there first.

1. Create a directory `/home/$USER/Android/`, this will end up being the location for the Android SDK and for Flutter (I personally like them being grouped together and Android Studio makes that directory by default, so no harm in using it).
2. Download the latest \*.tar.gz file from the [Flutter Instalation Guide](https://flutter.dev/docs/get-started/install/linux) or clone it from their git repo `git clone https://github.com/flutter/flutter.git` into the `/home/$USER/Android/` directory.
3. Add the Flutter path to your shell config file. If you use bash, the command would look something like 
```
echo export PATH="$PATH:path/to/flutter > ~/.bashrc
```
4. Run `flutter doctor`. This should output that you are missing the Android SDK and a "device" (which is the Android emulator).
5. Install Android Studio from the [AUR](https://aur.archlinux.org/packages/android-studio/). I use [yay](https://github.com/Jguer/yay), so it would look like:
```
yay -S android-studio
```

There is an Android SDK download that is available in the [AUR](https://aur.archlinux.org/packages/android-sdk/), but I was unable to get it to launch an emulator. Unfortunately, installing chonky Android Studio is the easiest way (that I have found).

6. Run Android Studio and go through the setup wizard.
7. When the instalation is complete, click Tools > Android > AVD Manager and create a virtual device.
8. In the terminal move to the directory `/home/$USER/Android/Sdk/emulator/emulator`.
9. `./emulator -list-avds` will output the name of the emulator you just created.
10. If you run `./emulator -avd [name_of_emulator]` the emulator should show up.
11. Create an alias in your shell configuration file `alias phone='~/Android/Sdk/emulator/emulator -avd [name_of_phone]`
12. Set up an editor 
	* If you use vim, you can intall plugins for [Dart support](https://github.com/dart-lang/dart-vim-plugin) and [Flutter commands](https://github.com/thosakwe/vim-flutter). As far as I am aware, there is currently no auto complete feature like that of Android Studio. 
	* [Visual Studio Code is supported officially](https://flutter.dev/docs/get-started/editor?tab=vscode)

If all went well, you should have a ☕cozy☕ terminal based Flutter workflow.

## Android Studio Setup
The only reason the section exists is because this is where I lived (for Flutter) for the first month or so. This setup uses vim keys and the like which makes for a semi-comfy coding experience. I would recommend this first if you are new because it lets you try Flutter without too much setting up. At any rate, after going through the Android-Studio wizard:
1. Go to plugins
2. Install Flutter (It will include Dart support as a dependency)
3. Install IdeaVim
4. Install Material Theme UI
5. Restart Android Studio
6. Create new Flutter project

Now you can configure it to your liking!
