## Ng-Bootstrap
1. Create worksapce
```bash
ng new ng-bootstrap
```

2. Add ng-bootstrap
```bash
yarn add @ng-bootstrap/ng-bootstrap
# or
# npm install --save @ng-bootstrap/ng-bootstrap
```

3. Add ng-alert to app.component.html
```html
<ngb-alert [dismissible]="false">
  <strong>Warning!</strong> Better check yourself, you're not looking too good.
</ngb-alert>
```

4. Add NgbModule to app.module
```ts
import {NgbAlertModule} from '@ng-bootstrap/ng-bootstrap';
// ...
imports: [
  //...
  NgbAlertModule,
  //...
]
```

5. Note if you want Bootstrap stylings
Option 1: Add boostrap.css as stylesheet to index.html
```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" />
```

Option 2: Add vid npm and add bootstrap.scss to styles.scss
```bash
yarn add bootstrap
```
```scss
@import '~bootstrap/scss/bootstrap';
```

6. [Add Bazel](https://angular.io/guide/bazel)
```bash
ng add @angular/bazel
```

Create the initial Bazel configuration files by running the following command:
```bash
ng build --leaveBazelFilesOnDisk
```
ERROR:
```
16:18 $ ng build --leaveBazelFilesOnDisk
Starting local Bazel server and connecting to it...
INFO: Call stack for the definition of repository 'npm' which is a yarn_install (rule definition at /private/
var/tmp/_bazel_cryan/6001c764db77e9e4437b171720c1971d/external/build_bazel_rules_nodejs/internal/npm_install/
npm_install.bzl:381:16):
 - /private/var/tmp/_bazel_cryan/6001c764db77e9e4437b171720c1971d/external/build_bazel_rules_nodejs/defs.bzl:82:5
 - /Users/cryan/code/p/bazel/ng-bootstrap/WORKSPACE:64:1
ERROR: An error occurred during the fetch of repository 'npm':
   Traceback (most recent call last):
        File "/private/var/tmp/_bazel_cryan/6001c764db77e9e4437b171720c1971d/external/build_bazel_rules_nodejs/internal/npm_install/npm_install.bzl", line 379
                _create_build_files(repository_ctx, "yarn_install", node, ...)
        File "/private/var/tmp/_bazel_cryan/6001c764db77e9e4437b171720c1971d/external/build_bazel_rules_nodejs/internal/npm_install/npm_install.bzl", line 146, in _create_build_files
                fail(("generate_build_file.js failed:...)))
generate_build_file.js failed: 
STDOUT:

STDERR:
Could not find peer dependency 'jquery' of 'bootstrap'
ERROR: no such package '@npm//': Traceback (most recent call last):
        File "/private/var/tmp/_bazel_cryan/6001c764db77e9e4437b171720c1971d/external/build_bazel_rules_nodejs/internal/npm_install/npm_install.bzl", line 379
                _create_build_files(repository_ctx, "yarn_install", node, ...)
        File "/private/var/tmp/_bazel_cryan/6001c764db77e9e4437b171720c1971d/external/build_bazel_rules_nodejs/internal/npm_install/npm_install.bzl", line 146, in _create_build_files
                fail(("generate_build_file.js failed:...)))
generate_build_file.js failed: 
STDOUT:

STDERR:
Could not find peer dependency 'jquery' of 'bootstrap'
ERROR: no such package '@npm//': Traceback (most recent call last):
        File "/private/var/tmp/_bazel_cryan/6001c764db77e9e4437b171720c1971d/external/build_bazel_rules_nodejs/internal/npm_install/npm_install.bzl", line 379
                _create_build_files(repository_ctx, "yarn_install", node, ...)
        File "/private/var/tmp/_bazel_cryan/6001c764db77e9e4437b171720c1971d/external/build_bazel_rules_nodejs/internal/npm_install/npm_install.bzl", line 146, in _create_build_files
                fail(("generate_build_file.js failed:...)))
generate_build_file.js failed: 
STDOUT:

STDERR:
Could not find peer dependency 'jquery' of 'bootstrap'
INFO: Elapsed time: 71.210s
INFO: 0 processes.
FAILED: Build did NOT complete successfully (0 packages loaded)
/Users/cryan/code/p/bazel/ng-bootstrap/node_modules/@bazel/bazel/node_modules/@bazel/bazel-darwin_x64/bazel-0.28.1-darwin-x86_64 failed with code 1.
```

Issue: https://github.com/angular/angular/issues/31617

Workaround: Install peers
```bash
yarn add jquery
yarn add popper.js
```

ERROR:
```
6:28 $ ng build
ERROR: /Users/cryan/code/p/bazel/ng-bootstrap/src/BUILD.bazel:12:9: Traceback (most recent call last):
        File "/Users/cryan/code/p/bazel/ng-bootstrap/src/BUILD.bazel", line 10
                sass_binary(name = "global_stylesheet", src = ...], ...")
        File "/Users/cryan/code/p/bazel/ng-bootstrap/src/BUILD.bazel", line 12, in sass_binary
                glob(["styles.css", "styles.scss"])[0]
index out of range (index is 0, but sequence has 0 elements)
ERROR: error loading package 'src': Package 'src' contains errors
INFO: Elapsed time: 22.169s
INFO: 0 processes.
FAILED: Build did NOT complete successfully (1 packages loaded)
/Users/cryan/code/p/bazel/ng-bootstrap/node_modules/@bazel/bazel-darwin_x64/bazel-0.28.1-darwin-x86_64 failed with code 1.
```










## [Bazel Query](https://docs.bazel.build/versions/master/query-how-to.html)
Query build graph
```bash
$ bazel query --output=graph ... | dot -Tpng > graph.png
```
## [Angular Bazel Example](https://github.com/angular/angular-bazel-example}

## [Bazel CLI](https://docs.bazel.build/versions/master/command-line-reference.html)

## [Customizing BUILD.bazel files](https://angular.io/guide/bazel#customizing-buildbazel-files)

### BUILD.bazel
- Declares a package (is a marker in your workspace)
- Many packages can be bundled together in a package ("uber-package") to publish to npm.
- Calls **Rules** with some attributes to derive some outputs given some inputs and dependencies.
- Bazel loads all the rules you've declared to determine an absolute ordering of what needs to be run. 
- Note that only the rules needed to produce the requested output will actually be executed.

### Rules
- **"Rules"** are like plugins for Bazel. 
- Many rule sets are available. 
- This guide documents the ones maintained by the Angular team at Google.
- Rules are used in `BUILD.bazel` files.
- In the BUILD.bazel file, each rule must first be imported, using the load statement. 

## [Front-end Rules rules_nodejs](https://github.com/bazelbuild/rules_nodejs/)
