# steps to reproduce issue
- run `npm install`
- run `npm run dev`
- observe that there is a plugin startup message `starting example sandbox plugin`
- press `ctl + c` to quit the process
- observe that there is no plugin stopped message
- go into your node_modules to `node_modules/@remix-run/dev/dist/devServer_unstable/index.js`
- change line 139 so that  instead of having `stdio: "pipe"` you have it broken into two lines, one for `stdout: "pipe"` and one for `stdin:"inherit"` like so:

```
let newAppServer = execa__default["default"].command(cmd, {
   stdout: "pipe",
   stdin: "inherit",
   env: {
      NODE_ENV: "development",
      PATH: bin + (process.platform === "win32" ? ";" : ":") + process.env.PATH,
      REMIX_DEV_ORIGIN: options.REMIX_DEV_ORIGIN.href,
      FORCE_COLOR: process.env.NO_COLOR === undefined ? "1" : "0"
   },
   // https://github.com/sindresorhus/execa/issues/433
   windowsHide: false
   })
```

- re-run `npm run dev`
- again you should see the plugin start message `starting example sandbox plugin`
- again press `ctl + c` to quit the process
- observe that now you see the plugin stop with `stopping example sandbox plugin`
- observe that the process hangs until you press `ctl + c` again.