#### How to use this boilerplate

1. Clone or download the repository
2. Run `npm i` to install all dependencies
3. Run `npm run "watch:project"` to start working
4. Use Visual Studio Code live server plugin to host compiled files, or use Webpack

#### A little bit about how this boilerplate was created and what is being used

1. Create a folder structure for the project, add `./src` and `./compiled` folders
2. Run `npm init -y` to create a `package.json` configuration file for the project
3. Install TypeScript `npm install typescript --save-dev`
4. Create a `tsconfig.json` configuration file for Typescript. It can be created manually or using `npx tsc --init`
5. Add options to `tsconfig.json`

```json
{
  "target": "ES2015",
  "module": "es2015",
  "lib": ["dom", "ES2015"],
  "sourceMap": true,
  "outDir": "./compiled/",
  "rootDir": "./src/",

  "strict": true,
  "noImplicitAny": true,
  "strictNullChecks": true,
  "esModuleInterop": true,
  "forceConsistentCasingInFileNames": true 
  }
```

6. Add a script to watch and compile files. Open `package.json` file and add new script

```json
  "scripts": {
    "tsc:compile": "tsc -w -p tsconfig.json"
  }
```

7. Add `index.html` file next to `package.json` file, and add to the `head` tag this line

```html
<script type="module" src="./compiled/index.js"></script>
```

8. Create main `index.ts` files and run `npm run "tsc:compile"`
9. Using export/import statements, live server plugin in Visual Studio Code fetches files wich can cause a problem. The problem is that after compiling we have the import statements without `*.js` extension being specified.
We can solve it by manually adding `.js` at the end of each statement each time the files have been, OR(!) add a file, I called it `toolForExtension.js` and use a script I found on [GitHub](https://github.com/microsoft/TypeScript/issues/16577) to fix that. Also, add a new command: 
   ```json
        "scripts": {
            "fix:extension": "node toolForExtension.js"
        }
   ```
Running scripts one by one - first to compile, then to fix extensions. Is should work fine. 
But we want to watch the files and run `toolForExtension.js` script each time `*.ts` files are changed!

10. Install nodemon dependency `npm install nodemon --save-dev`

11. Refactor one script, remove `-w` option from script we added before

    ```json
        "scripts": {
            "tsc:compile": "tsc -p tsconfig.json",
            "fix:extension": "node toolForExtension.js"
        }
    ```

    - add `nodemon.json` configuration file next to `package.json` file

    ```json
    {
      "watch": ["src/**/*.ts"],
      "ext": "ts",
      "ignore": ["src/**/*.spec.ts"],
      "exec": "npm run tsc:compile && npm run fix:extension"
    }
    ```

    - add a script for nodemon in `package.json` file

    ```json
        "scripts": {
            "watch:project": "nodemon",
            "tsc:compile": "tsc -p tsconfig.json",
            "fix:extension": "node toolForExtension.js"
        }
    ```

12. It will watch all `*.ts` files in `src/` and as soon as something is changed,
    it will run tsc compiler and run majic function to add `.js` extensions to all import statements

P.S. If you know how to work with webpack - use it instead to work and host compiled files

