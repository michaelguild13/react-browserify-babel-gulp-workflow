# react-browserify-babel-gulp-workflow
React, brwoserify, babel, gulp, es6, jsx work flow build 
```
// helped from https://github.com/gulpjs/gulp/blob/master/docs/recipes/fast-browserify-builds-with-watchify.md

var gulp = require('gulp');
var gutil = require('gulp-util');
var sourcemaps = require('gulp-sourcemaps');
var watchify = require('watchify');
var browserify = require('browserify');
var babelify = require('babelify');
var source = require('vinyl-source-stream');
var buffer = require('vinyl-buffer');
var assign = require('lodash.assign');

// add custom browserify options here
var customOpts = {
  entries: './app/app.jsx',
  extensions: ['.jsx'],
  debug: true
};
var opts = assign({}, watchify.args, customOpts);
var bify = watchify(browserify(opts));
bify.transform(babelify);// add transformations here

function bundle() {
  return bify.bundle()
    // log errors if they happen
    .on('error', gutil.log.bind(gutil, 'Browserify Error'))
    .pipe(source('app.js'))
    // optional, remove if you don't need to buffer file contents
    .pipe(buffer())
    // optional, remove if you dont want sourcemaps
    .pipe(sourcemaps.init({loadMaps: true})) // loads map from browserify file
    // Add transformation tasks to the pipeline here.
    .pipe(sourcemaps.write('./')) // writes .map file
    .pipe(gulp.dest('./dist'));
}
gulp.task('default', bundle); // so you can run `gulp` to build the file
bify.on('update', bundle); // on any dep update, runs the bundler
bify.on('log', gutil.log); // output build logs to terminal
```
