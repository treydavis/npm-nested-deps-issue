# npm-nested-deps-issue
A reproducible example of an npm version 6.14.6 install issue

In the situation where local (`file:`) deps form a chain and two share a common registry-derived dep, `npm i` will fail with an `ENOENT` error.
```
A
|
B -> some-module
|
C -> some-module
```
☝️ `A` depends on `B`. `B` depends on `C` and `some-module`. `C` depends on `some-module`.


Running:
```
cd a
npm i
```

will produce the following error:
```
npm WARN rollback Rolling back some-module@x.y.z failed (this is probably harmless): /npm-nested-deps-issue/c/node_modules/some-module is not a child of /npm-nested-deps-issue/a
npm WARN a@1.0.0 No description
npm WARN a@1.0.0 No repository field.

npm ERR! code ENOENT
npm ERR! syscall rename
npm ERR! path /npm-nested-deps-issue/a/node_modules/.staging/some-module-de6f41e0
npm ERR! dest /npm-nested-deps-issue/c/node_modules/some-module
npm ERR! errno -2
npm ERR! enoent ENOENT: no such file or directory, rename '/npm-nested-deps-issue/a/node_modules/.staging/some-module-de6f41e0' -> '/npm-nested-deps-issue/c/node_modules/some-module'
npm ERR! enoent This is related to npm not being able to find a file.
npm ERR! enoent
```
