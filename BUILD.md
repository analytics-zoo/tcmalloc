# How to build libtcmalloc.so

## Step 1: install bazel

Download the latest release of bazelisk from <https://github.com/bazelbuild/bazelisk/releases>, then rename the bazelisk executable file to `bazel` and add it to `PATH`

## Step 2: install git and c++ compiler (supporting c++17)

When using Ubuntu 20.04, you can install them by:

```bash
sudo apt-get install git
sudo apt-get install g++
```

## Step 3: get source code

Get the source code of tcmalloc

```bash
git clone https://github.com/google/tcmalloc.git
```

Then enter the `tcmalloc` directory, **during the following steps, assume you are in the root directory of this repo**

```bash
cd tcmalloc
```

## Step 4: edit the `tcmalloc/BUILD` file

The tcmalloc repo doesn't provide building .so support, so we should edit the `tcmalloc/BUILD` file to build a .so file

Find this build target in `tcmalloc/BUILD` file:

```
cc_library(
    name = "tcmalloc",
    ...
    alwayslink = 1,
)
```

Copy it and modify it:

```
cc_binary(
    name = "libtcmalloc.so",
    ...
    linkshared = True,
)
```

Note that you only need to change these three lines.

Add this new target in the `tcmalloc/BUILD` file.

## Step 5: edit the `tcmalloc/copts.bzl` file

**I'm not sure whether this step is necessary**

Add `"-O3"` in `TCMALLOC_GCC_FLAGS` in the `tcmalloc/copts.bzl` file

## Step 6: build .so file

```bash
bazel build //tcmalloc:libtcmalloc.so
```

After building, the .so file will be located in `bazel-bin/tcmalloc/libtcmalloc.so`



