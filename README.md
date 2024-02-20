# README
git add README.md
git commit -m "Add project information to README.md"
git push origin main
# CalCalator

CalCalator is a smartwatch application designed to help users monitor their calorie consumption using a blood sugar monitor. It utilizes the Google Cloud Health API to retrieve data on calories consumed and blood sugar levels. The application employs Gemini AI to create an algorithm that links the two datasets for accurate calorie tracking.

## Technologies Used
- Flutter
- Firebase (Firestore)
- Google Cloud Health API
- Gemini AI

## Features
- Integration with blood sugar monitor for real-time measurement.
- Utilization of Google Cloud Health API for retrieving calorie consumption and blood sugar level data.
- Implementation of Gemini AI algorithm for correlating blood sugar levels with calorie consumption.
- Storage of user data using Firebase Firestore.

## Installation
To install and run the CalCalator application locally, follow these steps:

1. Clone this repository: `git clone https://github.com/your-username/CalCalator.git`
2. Navigate to the project directory: `cd CalCalator`
3. Run `flutter pub get` to install dependencies.
4. Ensure you have a Firebase project set up and configured with Firestore.
5. Run the application on your preferred device/emulator: `flutter run`

## Usage
- Open the CalCalator application on your smartwatch.
- Connect the blood sugar monitor to the app for real-time measurement.
- The app will display your calorie consumption based on the blood sugar levels retrieved from the monitor and Google Cloud Health API.


Code :
import 'package:flutter/material.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'package:google_sign_in/google_sign_in.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Smartwatch Calorie Tracker',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: LoginPage(),
    );
  }
}

class LoginPage extends StatelessWidget {
  final FirebaseAuth _auth = FirebaseAuth.instance;
  final GoogleSignIn googleSignIn = GoogleSignIn();

  Future<UserCredential> _signInWithGoogle() async {
    try {
      final GoogleSignInAccount googleSignInAccount = await googleSignIn.signIn();
      final GoogleSignInAuthentication googleSignInAuthentication =
          await googleSignInAccount.authentication;

      final AuthCredential credential = GoogleAuthProvider.credential(
        accessToken: googleSignInAuthentication.accessToken,
        idToken: googleSignInAuthentication.idToken,
      );

      return await _auth.signInWithCredential(credential);
    } catch (error) {
      print("Error signing in with Google: $error");
      return null;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Login'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () async {
            UserCredential userCredential = await _signInWithGoogle();
            if (userCredential != null) {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => HomeScreen(user: userCredential.user),
                ),
              );
            }
          },
          child: Text('Sign in with Google'),
        ),
      ),
    );
  }
}

class HomeScreen extends StatelessWidget {
  final User user;

  HomeScreen({this.user});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Welcome, ${user.displayName}'),
            ElevatedButton(
              onPressed: () {
                // Implement integration with blood sugar monitor
              },
              child: Text('Measure Blood Sugar'),
            ),
            ElevatedButton(
              onPressed: () {
                // Implement integration with Google Cloud Health API
              },
              child: Text('Sync with Health API'),
            ),
            ElevatedButton(
              onPressed: () {
                // Navigate to calorie consumption page
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => CalorieConsumptionPage(),
                  ),
                );
              },
              child: Text('View Calorie Consumption'),
            ),
          ],
        ),
      ),
    );
  }
}

class CalorieConsumptionPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Calorie Consumption'),
      ),
      body: Center(
        child: Text('Display calorie consumption data here'),
      ),
    );
  }
}
