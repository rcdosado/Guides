CSS & JS Processing
=======

### How to remove unused CSS code to your project
Requirements:
* npm
* terminal
* node js

Steps:
1. install purgecss `npm i --save-dev purgecss`
2. you have your html e.g:
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>PurgeCSS sample</title>
	<link rel="stylesheet" href="./dist/bootstrap.min.css">
</head>
<body>
<div class="container">
<form>
  <div class="form-group">
    <label for="exampleInputEmail1">Email address</label>
    <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter email">
    <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
  </div>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="exampleCheck1">
    <label class="form-check-label" for="exampleCheck1">Check me out</label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>	
</div>

</body>
</html>

```
3.notice in line 20 you have link pointing to your lean css file. this will be the output css of purgecss.
4.Syntax of CLI command
```cmd
purgecss --css <css file> --content <content file to parse css> --out <output-directory>
```
5. execute it like this (assuming your've installed purgecss):
```cmd
node_modules\.bin\purgecss --css bootstrap.min.css --content index.html --output dist/
```
6. notice bootstrap.min.css is same directory with index.html, this command extracts all bootstrap css directives that is been used from index.html then outputs it to dist/ folder 
7. you can also use NPM script for this e.g
```javascript
{
  "name": "PurgeCSS",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "devDependencies": {
    "webpack": "^4.29.0"
  },
  "scripts": {
    "build": "purgecss --css bootstrap.min.css --content index.html --output dist/"
  }
}
```