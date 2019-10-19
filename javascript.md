# Node.js

#### use ECM on node > 12

```
node --experimental-modules index.js
```

#### run code in line

```
node -p "os.cpus().length"
```

# npm

#### create a package module

Add `"type": "module",`
```
npm init -y
```

#### install from GitHub

```
npm i expressjs/express
```

#### install only on dev mode

```
npm i -D eslint
```

#### create and update with npx

This command download the last version of `reactful` and run the command to create a project
```
npx reactful new project
```

#### run commands on deve mode

```
./node_modules/.bin/eslint -h
```