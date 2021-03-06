# Notifee - Testing Project

Tests are powered by [Jet ✈️](https://github.com/invertase/jet).

> **Note**: instructions in this file assume you're running terminal commands from the root of the project and not from inside the tests directory.

## Requirements

- Make sure you have Xcode installed (tested with Xcode 10+).
- Make sure you have NodeJS installed (Node 8.4.0 and up is required).
- Make sure you have all required dependencies installed:

  - [Apple Sim Utils](https://github.com/wix/AppleSimulatorUtils):

    ```bash
    brew tap wix/brew
    brew install wix/brew/applesimutils
    ```

> **Note**: If Homebrew complains about a conflict in the `wix/brew` tap, run `brew untap wix/brew && brew tap wix/brew` and try installing again

---

## Cleaning dependencies

You might find yourself in a situation where you want to start from a clean slate. The following will delete all `node_modules` and project `build` folders.

```bash
yarn lerna:clean
yarn build:all:clean
```

---

### Step 1: Install test project dependencies

```bash
yarn
yarn tests_rn:ios:pod:install
```

---

### Step 2: Start Packager Script

Start the React Native packager using the script provided;

```bash
yarn tests_rn:packager:jet
```

> ⚠️ It must be this script only that starts the RN Packager, using the default RN packager command will not work.

> ⚠️ Also ensure that all existing packagers are terminated and that you have no React Native debugger tabs open on your browsers.

> This packager will automatically rebuild on any JS changes to the library code. You don't need to restart this, leave it running whilst developing.

---

### Step 3: Build Native App

As always; the first build for each platform will take a while. Subsequent builds are much much quicker ⚡️

> ⚠️ You must rebuild native every time you make changes to native code (anything in /android /ios directories).

#### Android

```bash
yarn tests_rn:android:build
```

#### iOS

```bash
yarn tests_rn:ios:build
```

---

### Step 4: Setting up android emulator and iOS simulator

To run android tests you will need to create a new emulator and name it `TestingAVD` (You can't rename existing one).
This emulator will need to be up and running before you start your android tests from Step 5.

With iOS Detox will start a simulator for you by default or run tests in an open one.

---

### Step 5: Finally, run the tests

This action will launch a new simulator (if not already open) and run the tests on it.

> 💡 iOS by default will background launch the simulator - to have
> it launch in the foreground make sure any simulator is currently open, `Finder -> Simulator.app`.

> 💡 Android by default looks for a pre-defined emulator named `TestingAVD` - make sure you have one named the same setup on Android Studio.
> Or you can change this name in the `package.json` of the tests project (don't commit the change though please).
> **DO NOT** rename an existing AVD to this name - it will not work, rename does not change the file path currently so Detox will
> fail to find the AVD in the correct directory. Create a new one with Google Play Services.

#### Android

```bash
yarn tests_rn:android:test
```

#### iOS

```bash
yarn tests_rn:ios:test
```

The `tests_rn:${platform}:test` commands uninstall any existing app and installs a fresh copy. You can
run `tests_rn:${platform}:test-reuse` instead if you don't need to re-install the app (i.e only making JS code changes).
Just remember to use `test-${platform}` if you made native code changes and rebuilt - after installing once you can
go back to using the `reuse` variant.

The `tests_rn:${platform}:cover` variant of the yarn scripts will additionally run tests with coverage.
Coverage is output to the root directory of the project: `notifee/coverage`,
open `notifee/coverage/lcov-report/index.html` in your browser after running tests
to view detailed coverage output.

---

### Running specific tests

Mocha supports the `.only` syntax, e.g. instead of `describe(...) || it(...)` you can use `describe.only(...) || it.only(...)` to only run that specific context or test.

Another way to do this is via adding a `--grep` option to e2e/mocha.opts file, e.g. `--grep auth` for all tests that have auth in the file path or tests descriptions.

> 💡 Don't forget to remove these before committing your code and submitting a pull request

For more Mocha options see https://mochajs.org/#usage

---

### Linting & Typechecking files

Runs eslint and respective type checks on project files

```bash
yarn validate:all:js
yarn validate:all:ts
yarn validate:all:flow
```

---
