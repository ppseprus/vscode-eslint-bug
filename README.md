# VSCode ESLint Bug

In VSCode, when an incorrect config breaks the ESLint server, the "Restart ESLint Server" command does not work.

https://github.com/eslint/eslint/issues/18592

## Versions
Node version: 20.12.2
npm version: 10.5.2
Global ESLint version: none
Local ESLint version: 9.5.0
VSCode extension version: 2.4.4
VSCode version: 1.90.1
Operating System: macOS Sonoma 14.5 (23F79)

## ESLint Config
```js
// eslint.config.js
export default [{
  rules: {
    "no-unused-vars": "error",
  }
}]
```

## Steps to Reproduce
1. Using the above minimal config, have the ESLint server up and running in VSCode
```log
[Info  - 10:43:36 PM] ESLint server is starting.
[Info  - 10:43:37 PM] ESLint server running in node v20.9.0
[Info  - 10:43:37 PM] ESLint server is running.
[Info  - 10:43:37 PM] ESLint library loaded from: /Users/[...]/node_modules/eslint/lib/api.js
```
2. Break the config by misconfiguring it (eg. add `ignore` instead of `ignores`) **and hit save**
```log
[Error - 10:47:47 PM] An unexpected error occurred:
[Error - 10:47:47 PM] ConfigError: Config (unnamed): Unexpected key "ignore" found.
```
3. Remove / fix the incorrect **and hit save**
4. Open the Command Palette (cmd + shift + P) and select "ESLint: Restart ESLint Server"
5. Open the Command Palette (cmd + shift + P) and select "ESLint: Show Output Channel"

## Expected Behaviour

The ESLint plugin to restart and work correctly.


## Actual Behaviour

After the restart, the ESLint server exits.

```log
...
[Error - 10:49:40 PM] Server process exited with code 0.
```

The developer has to reload VSCode to get it working again.

## Additional Information

The same thing happens, when the config is provided via a `*.json` file and there's a trailing comma at the last line. Incorrectly formatted `*.json` breaks the server and the restart does not seem to work event after the issue is fixed.
