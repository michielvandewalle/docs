---
id: frontend-gulp
title: Frontend workflow
---

this is another document

```shell
node --version
```
```$xslt
output
v8.11.1
```
```shell
npm --version
```
```$xslt
output
5.6.0
```

If they are not installed, follow the instructions [here](https://www.dyclassroom.com/howto-mac/how-to-install-nodejs-and-npm-on-mac-using-homebrew).


## Install Gulp

```$xslt
npm install --global gulp-cli
```
```$xslt
gulp --version
```

## Frontend framework

Frontend is a framework in its truest sense: it does not provide you with UI and design out of the box, instead, it provides you with a solid architectural baseline upon which to complete your own work.

```$xslt
git clone git@bitbucket.org:michielvandewalle/frontend.git
cd frontend
```

Install the node packages
```$xslt
npm install
```

Link global packages browser-sync and imagemin
```
npm run prepublish
```

Set the `dest` folder in de gulpfile.js
```javascript
var config = {
  dest: './assets/', 
  ...  
```

Wordpress setup, install **_frontend** in the root. <br>
Set the `dest` in the gulpfile.js to point to your theme
```javascript
// Wordpress
var config = {
  dest: '../wp-content/themes/{THEME}/assets/', 
  ...    
```