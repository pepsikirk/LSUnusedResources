# LSUnusedResources
A Mac App to find unused images and resources in an Xcode project. It is heavily influenced by jeffhodnett‘s [Unused](http://jeffhodnett.github.io/Unused/), but Unused is very slow, and the results are not entirely correct. So I made some performance optimization, the search speed is more faster than Unused.

## Example

![LSMessageHUD Example1](https://github.com/tinymind/LSUnusedResources/raw/master/LSUnusedResourcesExample.gif)  

## Usage

It's an useful utility tool to check what resources are not being used in your Xcode projects. Very easy to use: 

1. Click `Browse..` to select a project folder.
2. Click `Search` to start searching.
3. Wait a few seconds, the results will be shown in the tableview.

## Feature

Check `Ignore similar name` to ignore the resources which referenced by string concatenation, `regex: ([-_]?\d+)`.

For example:

You import some resources like:

```
icon_tag_0.png
icon_tag_1.png
icon_tag_2.png
icon_tag_3.png

icon_title-0.png
icon_title-1.png
icon_title-2.png

icon_test0.png
icon_test1.png
icon_test2.png
```

And using in this way:

``` objc
NSInteger index = random() % 4;
UIImage *img0 = [UIImage imageNamed:[NSString stringWithFormat:@"icon_tag_%d", index]];
	
// Or
UIImage *img1 = [self createImageWithPrefix:@"icon_title" concat:@"-" andIndex:index];

// Or
UIImage *img2 = [self createImageWithPrefix:@"icon_test" andIndex:index];
```

`icon_tag_x.png`, `icon_title-x` and `icon_testx` will be considered to be used, should not be shown as unused resource.

## Installation

* Download `LSUnusedResources.app.zip` from the [latest release](https://github.com/tinymind/LSUnusedResources/releases/latest), or from the copy committed in this repo: [LSUnusedResources.app.zip](https://github.com/tinymind/LSUnusedResources/raw/master/Release/LSUnusedResources.app.zip)
* Or build and run the project using Xcode.

The app is a universal binary (`arm64` + `x86_64`). It is ad-hoc signed rather than notarized, so on Apple Silicon you may have to clear the quarantine flag before opening it: `xattr -dr com.apple.quarantine LSUnusedResources.app`

## How it works

1. Get resource files (default: `[imageset, jpg, png, gif]`) in these folders `[imageset, launchimage, appiconset, bundle, png]`.
2. Use regex to search all string names in code files (default: `[h, m, mm, swift, xib, storyboard, strings, c, cpp, html, js, json, plist, css]`).
3. Exclude all used string names from resources files, we get all unused resources files.

## Requirements

Requires macOS 10.13 and above, ARC. Building from source needs a macOS SDK that still accepts the `10.13` deployment target (Xcode 26 or earlier).
