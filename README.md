# Marathon Cloud GitHub Action

With this action you can easily run your Android instrumentation tests.

[Marathon Cloud](https://marathonlabs.io) is designed to run all your automated tests in just 15 minutes, 
no matter how many tests you have. 

## Quick example

A really basic example building an app apk, test apk and running tests:

```yaml
name: Run tests
on: push
jobs:
  run-tests:
    runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3
    - name: Build app
      run: ./gradlew assembleDebug assembleAndroidTest
    - name: Run tests
      uses: MarathonLabs/action-test@0.1.1
      with:
        apiKey: ${{ secrets.MARATHON_CLOUD_API_TOKEN }}
        application: app/build/outputs/apk/debug/app-debug.apk
        testAppplication: app/build/outputs/apk/androidTest/debug/app-debug-androidTest.apk
        githubToken: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

|             Name             | Description                                             | Default | Example                                                             |
| :--------------------------: | ------------------------------------------------------- | ------- | ------------------------------------------------------------------- |
|     `apiKey` (required)      | Marathon Cloud API key                                  |         | `cafebabe`                                                          |
|   `application` (required)   | Application binary path, e.g. apk file for Android      |         | `app/build/outputs/apk/debug/app-debug.apk`                          |
| `testApplication` (required) | Test application binary path, e.g. apk file for Android |         | `app/build/outputs/apk/androidTest/debug/app-debug-androidTest.apk` |
|     `output` (optional)      | Output folder path                                      |         | `output`                                                                  |
|      `link` (optional)       | Link to commit                                          |         |                                                                   |
| `version` (optional)         | marathon-cloud cli version to use                       | `latest`| `0.1.1`                                                             |
| `githubToken` (optional)     | GitHub token                                            |         | `${{ secrets.GITHUB_TOKEN }}`                                       |


## marathon-cloud version

If the `version` is not set, or is one of `latest` or `*`, the action will try to use the latest version of marathon-cloud.
However, due to the GitHub API rate limiting settings, this action requires to pass in the `GITHUB_TOKEN` input. If this input variable is not set, one will see error similar to the following:

```
Run MarathonLabs/setup-marathon-cloud@{version}
/home/runner/work/_actions/MarathonLabs/setup-marathon-cloud/{version}/lib/index.js:14812
            throw new Error('GITHUB_TOKEN is not set, unable to resolve the latest version of marathon-cloud');
```

## Developing

The action source is located at [/src](/src). The action is written in TypeScript and compiled to a single javascript file with [`ncc`][ncc]. It's expected to checkin `lib/index.js` to the repository.

To setup the development environment, run the following commands:

```bash
$ npm install
```

To build the action script, run the following command:

```bash
$ npm run build
```

To test the action, we can use the workflow [Test workflow](https://github.com/MarathonLabs/setup-marathon-cloud/actions/workflows/test-marathon-cloud.yaml) to trigger a build.

[ncc]: https://github.com/vercel/ncc
[marathon-cloud]: https://github.com/MarathonLabs/marathon-cloud-cli

## LICENSE

MIT
