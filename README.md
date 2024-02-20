# README
Google solution challenge project
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
