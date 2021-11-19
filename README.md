# iohook-build

This repository contains an augmented copy of the npm release of `iohook`(<https://www.npmjs.com/package/iohook>) in version 0.9.3, downloaded on 19th November 2021. This repository is not intended for general use but serves me as medium to build another project. The need for this repository is caused by the approach for deployment of the `iohook` module. The core of that module is written in native code, which must be compiled for each combination of operating system, cpu architecture, Node.js, and Electron version. In my project, I require support for at least both Windows x86_64 bit and macOS ARM64 and a specific Electron version. As there exist no prebuilt binaries for these combinations and the unavailable prebuilt binaries cause `npm install iohook` even to fails on macOS, I have decided to create this repository that mocks a release of the iohook that can be easily integrated into the standard npm / Node.js workflow. See an issue I commented on in the `iohook` repository on GitHub: <https://github.com/wilix-team/iohook/issues/315#issuecomment-972921484>

## Changes to the original release of `iohook`

I have made the following changes to the original release as downloaded from npm:

1. I put prebuilt binaries for my target systems under the `builds` directory.
1. I removed the `install.js` execution from the `npm install` command, as the script attempts to download other prebuilt binaries than what I need. Moreover, it even causes the installation to crash on macOS ARM64.
1. I removed ***all*** runtime dependencies as it appears that they are exclusively used in the `install.js` to gather prebuilt binaries from the Web. This even removes all known rumtime vulnerabilies, according to npm :)

## How to use this release in a project

Disclaimer: I strongly discourage anyone to actually integrate this release into their project.

Simply add this repository as depedency of your `package.json`:

```json
"dependencies": {
  "iohook": "raphaelmenges/iohook-build"
}
```

Afterwards, execute `npm install`.

## Add further prebuilt binaries

More prebuilt binaries can be added like the following:

1. Add this module to your target project as described above.
1. Run your target project and inspect an error stating that the prebuilt binary is not found, e.g.: `UnhandledPromiseRejectionWarning: Error: Cannot find module 'app\node_modules\iohook\builds\electron-v98-win32-x64\build\Release\iohook.node'`
1. Clone the iohook repository in version 0.9.3 onto your system: <https://github.com/wilix-team/iohook/tree/v0.9.3>
1. Execute "node build.js --runtime electron --version 15.3.1 --abi 98". I took the Electron version from the `package.lock` of my target project and the ABI version from the earlier error statement. See <https://wilix-team.github.io/iohook/manual-build.html#building-for-specific-versions-of-node> for details.
1. Put `iohook.node` and `uihook.dll` (or `.so` or `.dylib`, depending on your operating system) into the `builds` directory as expected by iohook. See again the error message for details about the directory structure.

I assume one needs a proper development environment to build the binaries. For me it worked out of the box, so I assume on needs at least Visual Studio 2017 on Windows and Xcode command line tools on macOS.

## License

All changes that I made are released under the MIT license.

> The MIT License (MIT)
>
> Copyright (c) 2021 Raphael Menges
>
> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

The license of the iohook module itself can be found in the `LICENSE` file in this repository.
