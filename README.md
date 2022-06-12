
# git.js

Publish files to a branch on GitHub.

## Getting Started

```shell
npm install git-push.js --save-dev
```

## Basic Usage

```js
var Git = require('git-push.js');

Git.publish('dist', function(err) {});
```


## `publish`

```js
Git.publish(dir, callback);
// or...
Git.publish(dir, options, callback);
```


### Options

The default options work for simple cases.  The options described below let you push to alternate branches, customize your commit messages, and more.


#### <a id="optionssrc">options.src</a>
 * type: `string|Array<string>`
 * default: `'**/*'`

The [minimatch](https://github.com/isaacs/minimatch) pattern or array of patterns used to select which files should be published.


#### <a id="optionsbranch">options.branch</a>
 * type: `string`
 * default: `'master'`
 * `-b | --branch <branch name>`

The name of the branch you'll be pushing to.  The default uses GitHub's `master` branch, but this can be configured to push to any branch on any remote.

Example use of the `branch` option:

```js
/**
 * This task pushes to the `main` branch of the configured `repo`.
 */
Git.publish('dist', {
  branch: 'main',
  repo: 'https://example.com/other/repo.git'
}, callback);
```


#### <a id="optionsadd">options.add</a>
 * type: `boolean`
 * default: `false`

Only add, and never remove existing files.  By default, existing files in the target branch are removed before adding the ones from your `src` config.  If you want the task to add new `src` files but leave existing ones untouched, set `add: true` in your options.

Example use of the `add` option:

```js
/**
 * The usage below will only add files to the `master` branch, never removing
 * any existing files (even if they don't exist in the `src` config).
 */
Git.publish('dist', {add: true}, callback);
```


#### <a id="optionsrepo">options.repo</a>
 * type: `string`
 * default: url for the origin remote of the current dir (assumes a git repository)
 * `-r | --repo <repo url>`

By default, `master` assumes that the current working directory is a git repository, and that you want to push changes to the `origin` remote.

If instead your script is not in a git repository, or if you want to push to another repository, you can provide the repository URL in the `repo` option.

Example use of the `repo` option:

```js
/**
 * If the current directory is not a clone of the repository you want to work
 * with, set the URL for the repository in the `repo` option.  This usage will
 * push all files in the `src` config to the `master` branch of the `repo`.
 */
Git.publish('dist', {
  repo: 'https://example.com/other/repo.git'
}, callback);
```


#### <a id="optionsremote">options.remote</a>
 * type: `string`
 * default: `'origin'`

The name of the remote you'll be pushing to.  The default is your `'origin'` remote, but this can be configured to push to any remote.

Example use of the `remote` option:

```js
/**
 * This task pushes to the `master` branch of of your `upstream` remote.
 */
Git.publish('dist', {
  remote: 'upstream'
}, callback);
```


#### <a id="optionstag">options.tag</a>
 * type: `string`
 * default: `''`

Create a tag after committing changes on the target branch.  By default, no tag is created.  To create a tag, provide the tag name as the option value.


#### <a id="optionsmessage">options.message</a>
 * type: `string`
 * default: `'👨‍💻CODE BY 🕊️★⃝AJAY O S©️🧚‍♂️'`

The commit message for all commits.

Example use of the `message` option:

```js
/**
 * This adds commits with a custom message.
 */
Git.publish('dist', {
  message: 'Auto-generated commit'
}, callback);
```


#### <a id="optionsuser">options.user</a>
 * type: `Object`
 * default: `null`

If you are running the `master` task in a repository without a `user.name` or `user.email` git config properties (or on a machine without these global config properties), you must provide user info before git allows you to commit.  The `options.user` object accepts `name` and `email` string values to identify the committer.

Example use of the `user` option:

```js
Git.publish('dist', {
  user: {
    name: 'Ajay o s',
    email: 'blacksudo@example.com'
  }
}, callback);
```

#### <a id="optionsuser">options.remove</a>
 * type: `string`
 * default: `'.'`

Removes files that match the given pattern (Ignored if used together with
`--add`). By default, `master` removes everything inside the target branch
auto-generated directory before copying the new files from `dir`.

Example use of the `remove` option:

```js
Git.publish('dist', {
  remove: "*.json"
}, callback);
```


#### <a id="optionspush">options.push</a>
 * type: `boolean`
 * default: `true`

Push branch to remote.  To commit only (with no push) set to `false`.

Example use of the `push` option:

```js
Git.publish('dist', {push: false}, callback);
```


#### <a id="optionshistory">options.history</a>
 * type: `boolean`
 * default: `true`

Push force new commit without parent history.

Example use of the `history` option:

```js
Git.publish('dist', {history: false}, callback);
```


#### <a id="optionssilent">options.silent</a>
 * type: `boolean`
 * default: `false`

Avoid showing repository URLs or other information in errors.

Example use of the `silent` option:

```js
/**
 * This configuration will avoid logging the GH_TOKEN if there is an error.
 */
Git.publish('dist', {
  repo: 'https://' + process.env.GH_TOKEN + '@github.com/user/private-repo.git',
  silent: true
}, callback);
```


#### <a id="optionsbeforeadd">options.beforeAdd</a>
 * type: `function`
 * default: `null`

Custom callback that is executed right before `git add`.

The CLI expects a file exporting the beforeAdd function

```bash
@ajay-o-s/git.js --before-add ./cleanup.js
```

Example use of the `beforeAdd` option:

```js
/**
 * beforeAdd makes most sense when `add` option is active
 * Assuming we want to keep everything on the master branch
 * but remove just `some-outdated-file.txt`
 */
Git.publish('dist', {
  add: true,
  async beforeAdd(git) {
    return git.rm('./some-outdated-file.txt');
  }
}, callback);
```


#### <a id="optionsgit">options.git</a>
 * type: `string`
 * default: `'git'`

Your `git` executable.

Example use of the `git` option:

```js
/**
 * If `git` is not on your path, provide the path as shown below.
 */
Git.publish('dist', {
  git: '/path/to/git'
}, callback);
```
