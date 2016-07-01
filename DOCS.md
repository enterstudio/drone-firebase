# Deploy a project to Firebase

Use the Firebase plugin to deploy files to a project hosted using Firebase.
The following parameters are used to configure this plugin:

* `token` - The token to use for login
* `project_id` - (optional) The project alias to deploy to. If not set, uses the
  default specified in the `.firebaserc` file. Note: This does currently not
  work for Firebase project IDs, so you must run `firebase --add` to add aliases
  for your different environments. Then use this field to specify the alias name
  such as `default`, `staging` or `production`.
* `message` - (optional) The message to use for your commit. You can use
  variables available from Drone.io, such as $$COMMIT as part of the message.
  The message does not have to be quoted.
* `targets` - (optional) The type of deployment to be done. Must be a comma
  separated list of the following types: `hosting`, `database`, and `storage`.
* `dryrun` - (optional) A bool indicating whether the deploy commands should be
  executed, or just printed to stdout.
* `debug` - (optional) A bool indicating whether commands should run in debug
  mode. *WARNING: This will print all information about requests such as
  authentication information!*

Sample configuration in the `.drone.yml` file:

```yaml
deploy:
  firebase:
    image: google/drone-firebase
    token: >
      $$FIREBASE_TOKEN
    project_id: staging
    message: Autodeploy of commit $$COMMIT
    targets: hosting
```

And your `.drone.sec` file should then be created with:

```yaml
environment:
  FIREBASE_TOKEN: >
    1/deadbeefforrealz-4
```

Only `token` is required, and the value for `project_id` refers to a project
alias `staging` defined in the `.firebaserc` file of the project.

## Token

The authorization token can be generated by using the Firebase CLI. To install
that, if you already have Node.js and npm (the Node Package Manager) installed,
you can run:

```bash
npm install -g firebase-tools
```

Otherwise, you need to install `npm` first. See the
[Firebase CLI documentation](https://firebase.google.com/docs/cli/) for
instructions on how to do that.

To create a token to be used by your continuous integration server, you can then
run the following command:

```bash
firebase login:ci
```

This will bring up a browser window for allow you to create a token. If you
run this command a place where you do not have a browser available, you can run
it with:

```bash
firebase login:ci --no-localhost
```

This will enable you to still get the token, by going to the URL manually, and
then copy-pasting the result back to the console.

For more information, take a look at the
[Firebase CLI command reference](https://firebase.google.com/docs/cli/#command_reference).