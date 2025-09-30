
📍 Flutter Geolocation App

A simple Flutter app that demonstrates how to:

Check if Location Services are enabled.

Request Location Permissions.

Retrieve the Latitude & Longitude of the device.

Show alerts using AwesomeDialog if location services are disabled.

✨ Features

✅ Detect if location services are enabled.
✅ Request permission from the user.
✅ Fetch current latitude & longitude.
✅ Display alerts in a beautiful dialog.
✅ Example code using Geolocator and AwesomeDialog.

🛠️ Installation

Clone the repository:

git clone <https://github.com/your-username/flutter-geolocation-app.git>
cd flutter-geolocation-app

Install dependencies:

flutter pub get

Run the app:

flutter run

📦 Dependencies

Make sure you add these in your pubspec.yaml:

dependencies:
  flutter:
    sdk: flutter
  geolocator: ^13.0.1
  awesome_dialog: ^3.2.1

⚙️ Android Setup

Add these permissions inside AndroidManifest.xml:

<!-- Required for location -->

<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<!-- For Android 10+ (Background Location) -->

<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION"/>

<!-- For Android 14+ (Foreground Service Location) -->

<uses-permission android:name="android.permission.FOREGROUND_SERVICE_LOCATION"/>

📱 Code Example
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:geolocator/geolocator.dart';
import 'package:awesome_dialog/awesome_dialog.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Geolocation Demo',
      home: const HomeScreen(),
    );
  }
}

class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  Future<void> getPosition() async {
    bool services;
    LocationPermission per;

    // Check if location services are enabled
    services = await Geolocator.isLocationServiceEnabled();
    if (!services) {
      if (kDebugMode) {
        print("Location services disabled: $services");
      }
      AwesomeDialog(
        context: context,
        title: "Alert",
        body: const Text("Turn Location on"),
      ).show();
    }

    // Check for permissions
    per = await Geolocator.checkPermission();
    if (kDebugMode) {
      print("Permission status: $per");
    }

    if (per == LocationPermission.denied) {
      per = await Geolocator.requestPermission();
      if (per == LocationPermission.whileInUse) {
        getLatandLang();
      }
    }
  }

  Future<Position> getLatandLang() async {
    return await Geolocator.getCurrentPosition().then((value) => value);
  }

  @override
  void initState() {
    getPosition();
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () async {
            Position cl = await getLatandLang();
            if (kDebugMode) {
              print("Latitude: ${cl.latitude}");
              print("Longitude: ${cl.longitude}");
            }
          },
          child: const Text("Get Lat and Long"),
        ),
      ),
    );
  }
}
