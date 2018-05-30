# About The Package
This (very small) package is designed to assist with the development of cross-platform libraries with the Qt Framework while targeting Android.

When targeting android any file that is not under the directory set via `ANDROID_PACKAGE_SOURCE_DIR` will be omitted by the build process. While this makes obvious sense, unfortunately it is not possible to specify multiple paths via `ANDROID_PACKAGE_SOURCE_DIR` so library developers cannot easily include their source files without pre-compiling their code and linking.

To circumvent this limitation the `copyAndroidSources` [replace function](http://doc.qt.io/qt-5/qmake-function-reference.html) is provided with this package It's usage is simple:

## Usage Example
```
include(qmakeAndroidSourcesHelper/functions.pri)

android {
  QMAKE_EXTRA_TARGETS += $$copyAndroidSources("MyJavaSources", "src/com/package", $$shell_path($$PWD/platforms/android/MyJavaSource.java))
}
```

The above block (within a .pro or .pri file) will ensure that `MyJavaSource.java` is copied to `ANDROID_PACKAGE_SOURCE_DIR/src/com/package` before the gradle build commences.

The function will also create and/or update a .gitignore file in the destination path to ensure that library files are not commited.

## Method Parameters
```
copyAndroidSources(commandAlias, targetDirectory, sourceFiles)
```

* `commandAlias` is a unique name assigned to the specific copy operation
* `targetDirectory` is the location under the android build directory that the files should be copied
* `sourceFiles` a individual or set of files to copy

## Under The Hood
This package works by injecting make steps that are run prior to the gradle build commencing. `commandAlias` is required as each make step requires a unique identifier within the Makefile.

## Real World Example
To see an example of `copyAndroidSources` in use please check out my [Facebook QML](https://github.com/lukevear/FacebookQml) helper package.