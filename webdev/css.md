CSS & JS Processing
=======



### How to remove unused CSS code to your project

Requirements:
* npm
* terminal
* node js

Steps:
1. install purgecss `npm i --save-dev purgecss`
2. you have your html that uses bootstrap or any other bloated css e.g:
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
3. modify your html files to point to a build location example ./dist/ so it points to the output folder of purgecss.

4. take not of the syntax of purgeCSS CLI command, and you can add other option if you want
```cmd
purgecss --css <css file> --content <content file to parse css> --output <output-directory>
```
from the docs
```cmd
Usage: purgecss --css <css> --content <content> [options]

Options:
  -con, --content <files>  glob of content files
  -css, --css <files>      glob of css files
  -c, --config <path>      path to the configuration file
  -o, --output <path>      file path directory to write purged css files to
  -font, --font-face       option to remove unused font-faces
  -keyframes, --keyframes  option to remove unused keyframes
  -rejected, --rejected    option to output rejected selectors
  -s, --safelist <list>   list of classes that should not be removed
  -h, --help               display help for command
```
5. execute it like this (assuming you've installed purgecss):
```cmd
node_modules\.bin\purgecss --css bootstrap.min.css --content index.html --output dist/
```
6. this command executes purgecss to extract bootstrap.min.css css directives that index.html uses and output it to dist/ folder.
7. after that make sure your lean CSS is pointed by your HTMLs, take not that you can also use NPM script for this e.g
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

to use this simply paste to a file named package.json then execute `npm build` in your terminal (assuming you have npm in your path)

See the [documentation](https://purgecss.com/CLI.html) for more