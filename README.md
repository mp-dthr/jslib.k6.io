# jslib.k6.io

[jslib.k6.io](http://jslib.k6.io), hosted utility libs for k6 scripts.


To make life a little easier during development make sure that the following are installed:
* node >= 11.2.0
* yarn >= 1.13.0

## How to add a new JS package
1. Create a new branch from master.
2. Create a new lib dir `/lib/{lib_name}`
3. Create a new version dir `/lib/{lib_name}/{desired_version}/`
4. Add a entry file `/lib/{lib_name}/{desired_version}/{desired_name}.js`
5. Add the lib to `supported.json` (see example below):
```javascript
{
  "{lib_name}": {
    "{desired_version}": {}
  }
}

// Example result
{
  "awesome-lib": {
    "2.0.3": {}
  }
}
```
6. Add test case in `/tests/basic.js` to make sure that the added lib is importable and runnable by k6.
7. Update the homepage by running `yarn run generate-homepage`.
8. Verify that new homepage `/lib/index.html` is legit (Quickly done by running `yarn run verify-homepage`).
9. Create a PR, get approved etc..
10. Merge master. (This will push the updates to S3).
11. Browse to https://jslib.k6.io/{lib_name}/{desired_version}/{desired_name}.js


## How to add a new version of a and existing package.
1. Create a new branch from master.
2. Create a new "version dir" in `/lib/{lib_name}/{desired_version}/`
3. Add entry file for version `/lib/{lib_name}/{desired_version}/{desired_name}.js`
4. Add the version to `supported.json` like:
```javascript
{
  "my-lib": {
    "1.0.2": {},
    // Use semantic versioning
    "{desired_version}": {}
  }
}

// Example result
{
  "my-lib": {
    "1.0.2": {},
    "2.0.3": {}
  }
}
```
5. Add test case in `/tests/basic.js` to make sure that the ported lib is importable and runnable by k6.
6. Update the homepage by running `yarn run generate-homepage`.
7. Verify that new homepage `/lib/index.html` is legit (Quickly done by running `yarn run verify-homepage`).
8. Create a PR, get approved etc..
9. Merge master. (This will push the updates to S3).
10. Browse to https://jslib.k6.io/{lib_name}/{desired_version}/{desired_name}.js


## Updating styling and layout of the "Homepage"

The homepage of https://jslib.k6.io live in the `/static` dir.\
The final `index.html` page gets generated by running `yarn run generate-homepage`.

Useful dev commands:

```bash
# Listen to file changes, restart devserver and serve index.html
yarn run develop-homepage

# Quickly serve index.html
yarn run verify-homepage

# Generate index.html
yarn run generate-homepage
```


## Installing new package via npm and automagically adding it to libs
The outcome of this might vary depeding on the quality of the desired node_module you want to add.

The following command will try to:
1. Install the desired {package_name}
2. Copy the module to `/lib`
3. Update the homepage `/lib/index.html`

```bash
yarn run add-pkg {package_name}
```

Once run successfully, make sure that you do the following:
- Add test case in `/tests/basic.js` to make sure that the ported lib is importable and runnable by k6.
- Verify that new homepage `/lib/index.html` is legit.


## Updating a version of a JS package listed in _package.json dependencies_
Some of the JS packages in `/lib/` can be libraries that exist on NPM today.
To add a specific or the latest version of a package that exists on npm the following steps will help you with that.

1. `yarn upgrade {my_package}@version` (for specific version) `yarn upgrade {my_package} --latest` (for the latest).
2. `yarn run copy-libs`

If all went well the following should have happened:

* New version dir and entry file created `/lib/{my_package}/{new_version}/index.js`.
* New version entry added to `support.json` (see example below)
```javascript
{
  "my_package": {
    "1.0.2": {}, // Previous version
    "2.0.0": {} // New version added by copy-libs command.
  }
}
```
If not then you will have to manually create the new version like described [here](how-to-add-a-new-version-of-a-and-existing-package).

Once a new version dir has been created and added to `supported.json` it is time to:

3. Add test case in `/tests/basic.js` to make sure that the added version is importable and runnable by k6.
4. Update the homepage by running `yarn run generate-homepage`.
5. Verify that new homepage `/lib/index.html` is legit (Quickly done by running `yarn run verify-homepage`).
6. Create a PR, get approved etc..
7. Merge master. (This will push the updates to S3).
8. Browse to https://jslib.k6.io/{lib_name}/{desired_version}/{desired_name}.js
