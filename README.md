### package.json

```
"devDependencies": {
    "autoprefixer": "^10.4.16",
    "gulp": "^4.0.2",
    "gulp-plumber": "^1.2.1",
    "gulp-postcss": "^9.0.1",
    "gulp-sass": "^5.1.0",
    "gulp-sourcemaps": "^3.0.0",
    "postcss": "^8.4.33",
    "sass": "^1.69.7"
  }
```

### gulpfile.js
```
const { series, parallel, src, dest, watch } = require('gulp');
const sass = require('gulp-sass')(require('sass'));
const autoprefixer = require('autoprefixer');
const postcss = require('gulp-postcss');
const sourcemaps = require('gulp-sourcemaps');

function styles() {
  return src('src/scss/**/*.scss')
    .pipe(sourcemaps.init())
    .pipe(sass().on('error', sass.logError))
    // .pipe(sass({ outputStyle: 'expanded' })) // Компиляция SASS
    .pipe(sass({ outputStyle: 'expanded' }).on('error', sass.logError))
    .pipe(postcss([autoprefixer()]))
    .pipe(sourcemaps.write('.'))
    .pipe(dest('dist/css'));
}

function watchFiles() {
  watch('src/scss/**/*.scss', styles);
}
```
exports.default = series(styles, watchFiles);
