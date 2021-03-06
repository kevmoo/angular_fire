<p align="center">
  <h1 align="center">angular_fire</h1>
  <p align="center">
    An <em>unofficial</em> library for <a href="https://webdev.dartlang.org/angular">AngularDart</a> and <a href="https://firebase.google.com">Firebase</a>.
  </p>
</p>

[![pub package](https://img.shields.io/pub/v/angular_fire.svg)](https://pub.dartlang.org/packages/angular_fire)
[![Build Status](https://travis-ci.org/matanlurey/angular_fire.svg)](https://travis-ci.org/matanlurey/angular_fire)

* [Install](#install)
* [Usage](#usage)
  * [Services](#services)
    * [FirebaseAuth](#firebase-auth)
  * [Directives](#directives)
    * [GoogleSignInComponent](#google-sign-in-component)
    * [IfFirebaseAuthDirective](#if-firebase-auth-directive)
* [Contributing](#contributing)
  * [Testing](#testing)

## Install

The current stable release of `angular_fire` works best with the following:

```yaml
dependencies:
  angular2: ^3.1.0
  angular_fire: ^0.1.0
  firebase: ^3.1.0
```

To get started, you need to, at minimum, include the Firebase SDK:

```html
<body>
  <script src="https://www.gstatic.com/firebasejs/4.0.0/firebase.js"></script>
</body>
```

<!-- TODO: Add an `example` folder. -->

## Usage

### Services

#### FirebaseAuth

A high-level authentication service. First setup for dependency injection:

```dart
import 'package:angular2/angular2.dart';
import 'package:angular2/platform/browser.dart';
import 'package:angular_fire/angular_fire.dart';
import 'package:firebase/firebase.dart' as sdk;

main() {
  bootstrap(AngularFireExample, <dynamic>[
    provide(
      FirebaseAuth,
      useValue: new FirebaseAuth(
        sdk.initializeApp(
          apiKey: '...',
          authDomain: '...',
          databaseURL: '...',
          storageBucket: '...',
        ),
      ),
    ),
  ]);
}
```

Then inject into your app and use. See [GoogleSignInComponent](#google-sign-in-component)
below for an example.

### Components

#### GoogleSignInComponent

Displays a rendered sign in box for Google authentication that follows the
[branding guidelines](https://developers.google.com/identity/branding-guidelines).

<img src="https://cloud.githubusercontent.com/assets/168174/26565270/896f1ac6-449e-11e7-8e7a-967547e5fb65.png" height="600" />
<br>

```dart
import 'package:angular2/angular2.dart';
import 'package:angular_fire/angular_fire.dart';

@Component(
  selector: 'angular-fire-example',
  directives: const [
    GoogleSignInComponent,
  ],
  template: r'''
    <google-sign-in (trigger)="signIn()">
    </google-sign-in>
  ''',
)
class AngularFireExample {
  final FirebaseAuth _auth;
  
  void onTrigger() {
    _auth.signIn();
  }
}
```

**NOTE**: To use this component, you must have the brand assets in your
`web/assets` directory, or use the `[assetPath]` property, or the 
`googleSignInAssetPath` token at `bootstrap` time to configure the location of
your assets - for example on an external CDN.

#### IfFirebaseAuthDirective

Like `ngIf`, but shows content if the value matches the current authentication:

```html
<div *ifFirebaseAuth="true; let currentUser = currentUser">
  Logged in as: {{currentUser.displayName}}.
  <button (click)="signOut()">Sign Out</button>
</div>

<div *ifFirebaseAuth="false">
  Waiting for sign in...

  <br>

  <google-sign-in
      (trigger)="signIn()">
  </google-sign-in>
</div>
```

## Contributing

We welcome a diverse set of contributions, including, but not limited to:
* [Filing bugs and feature requests][file_issue]
* [Send a pull request][pull_request]
* Or, create something awesome using this API and share with us and others!

For the stability of the API and existing users, consider opening an issue
first before implementing a large new feature or breaking an API. For smaller
changes (like documentation, bug fixes), just send a pull request.

[file_issue]: https://github.com/matanlurey/angular_fire/issues/new
[pull_request]: https://github.com/matanlurey/angular_fire/pulls/new

### Testing

Run the (simple) test suite in Dartium. It doesn't currently run on Travis:

```shell
$ pub run angular_test -p dartium
```
