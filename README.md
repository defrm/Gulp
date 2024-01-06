# Настройка Gulp с browser-sync

### package.json

```
  "devDependencies": {
    "autoprefixer": "^10.4.16",
    "browser-sync": "^3.0.2",
    "gulp": "^4.0.2",
    "gulp-cli": "^2.3.0",
    "gulp-plumber": "^1.2.1",
    "gulp-postcss": "^9.0.1",
    "gulp-sass": "^5.1.0",
    "gulp-sourcemaps": "^3.0.0",
    "laravel-mix": "^6.0.49",
    "postcss": "^8.4.33",
    "sass": "^1.69.7",
    "swiper": "^11.0.5"
  }
```

### gulpfile.js
```
'use strict';
const { series, src, dest, watch } = require('gulp');
const sass = require('gulp-sass')(require('sass'));
const autoprefixer = require('autoprefixer');
const postcss = require('gulp-postcss');
const sourcemaps = require('gulp-sourcemaps');
const browserSync = require('browser-sync').create();

function styles() {
  return src('sass/style.scss')
    .pipe(sourcemaps.init())
    .pipe(sass().on('error', sass.logError))
    .pipe(sass({ outputStyle: 'expanded' }).on('error', sass.logError))
    .pipe(postcss([autoprefixer()]))
    .pipe(sourcemaps.write('.'))
    .pipe(dest('./'))
    .pipe(browserSync.stream());
}

function browserSyncInit(done) {
  browserSync.init({
    proxy: '127.0.0.1:8080',
    port: 3000
  });
  done();
}

function watchFiles() {
  watch('sass/**/*.scss', styles);
  watch('*.html').on('change', browserSync.reload);
}

exports.default = series(
  styles,
  browserSyncInit,
  watchFiles
);
