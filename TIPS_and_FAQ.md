# Wild Web Developer FAQ

### As a user, how can I...

#### Use Embedded Node.js?

When a developer's system suits the supported OSs (Linux, MacOS and Win32) and architectures (x86_64) the embedded Node.js from Wild Web Developer will be used by WildWebDeveloper to run language Servers as well as for Node Debugger unless the `"org.eclipse.wildwebdeveloper.nodeJSLocation"` system property is set.

#### Automatically compile TypeScript to JavaScript ?

(From https://github.com/eclipse/wildwebdeveloper/issues/331#issuecomment-577240880 )

1. Right-click on project > Properties > Builders
2. Click "New", select "Program"
3. location => path to tsc (can be `${system_path:tsc}`; working directory => project directory (can be `${project_loc}`); arguments...
4. In Build Options
  a. select "Launch in background"
  b. select "During auto-build"
  c. select the interesting typescript source folder in "Select working set of relevant resources" to include the source folder.
5. in Refresh, select what needs to be refreshed upon build.

#### Debug a TypeScript program?

1. Ensure the `sourceMap` property of your `tsconfig.json` is set to `true`.
Example `tsconfig.json`:
```JSON
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "outDir": "out",
    "sourceMap": true
  }
}
```
2. Run `tsc` to transpile your TypeScript source code to JavaScript.
3. Right click on TypeScript you want to debug (eg. `index.ts`) => Debug as => Node program
4. If a breakpoint is set in your `.ts` source file, it will be hit when the equivalent JavaScript code is run.

#### Debug a website developed in TypeScript?

1. Ensure the `sourceMap` property of your `tsconfig.json` is set to `true`.
Example `tsconfig.json`:
```JSON
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "outDir": "out",
    "sourceMap": true
  }
}
```
2. Run `tsc` to transpile your TypeScript source code to JavaScript.
3. Right click on the entry point of your website (eg. `index.html`) => Debug as => Chrome Debug
4. If a breakpoint is set in your `.ts` source file, it will be hit when the equivalent JavaScript code is run.

#### Debug a client-side code of a web app?

1. Start your web app's Node.JS web server (usually done through a NPM script in your package.json, eg. `npm start` ). This can be done through the commandline or by right-clicking on a package.json in the Project Explorer => Run as => NPM...
2. Once your web app is running, take note of the local URL it's running on. For React project's, it's by default `http://localhost:3000/`
3. Right click on your project's root directory in the Project Explorer => Debug as => Debug configurations...
4. Create a launch or attach debug configuration for Chrome or Firefox, depending on your preference.
5. For launch debug configurations, select the URL radio button and enter the URL from step 2. For attach debug configurations, simply enter the URL from step 2.
6. Ensure the working directory is correctly set to your project's root folder.
7. Click `apply` followed by `debug` in the bottom right corner of the debug configuration

#### Get instant HTML preview on save ?

1. Open the HTML file with the Generic Editor for edition.
2. From the Edition context menu, the Project Explorer or other explorer, open this same HTML file with the Internal Web Browser.
3. Drag the editor/browser to get them side by side or stacked one on top of the other in the IDE.
4. In the Web Browser, click the arrow besides the refresh button, and select "Auto-refersh local changes"

### As an Eclipse plugin developer, how can I...

#### Reuse Embedded Node.js?

Developers may [reuse the Node.js Embedder](https://github.com/eclipse/wildwebdeveloper/blob/master/org.eclipse.wildwebdeveloper.embedder.node/README.md) in their products.

#### Attach a debugger to the XML Language Server process?

Run Eclipse with the following JVM property, e.g. set in `eclipse.ini`:

```text
-vmargs
...
-Dorg.eclipse.wildwebdeveloper.xml.internal.XMLLanguageServer.debugPort=8001
```

Note this is a JVM property you set in the parent JVM (the Eclipse IDE) so that the (LemMinX) XML Language Server child process gets started, suspended, in debug mode, waiting for the debugger attachment.  You can set the property's value to a different port if you'd like to select a different port value (than 8001 in the example).

#### Enable java.util.logging in the XML Language Server process?

Run Eclipse with the following JVM property, e.g. set in `eclipse.ini`:

```text
-vmargs
...
-Dorg.eclipse.wildwebdeveloper.xml.internal.XMLLanguageServer.log.level=all
```

Note this is a JVM property you set in the parent JVM (the Eclipse IDE) to take effect within the (LemMinX) XML Language Server child process.

The LemMinX process will use this property value as the "level" for its "root" Logger.   

This workflow doesn't support the full set of **java.util.logging** function: handlers/formatters/etc.  The normal handler registration (e.g. **ConsoleHandler**) is disabled and in its place a handler is created which writes the log messages to the file:  `<workspace-root>/.metadata/lemminx.log`.  Currently this handler is coded to only log messages at Level.INFO or higher.

 

