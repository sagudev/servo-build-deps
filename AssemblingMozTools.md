# Assembling MozTools

Every moztools bundle is based upon [MozillaBuild](https://wiki.mozilla.org/MozillaBuild). It's versions are also taken from MozillaBuild that was used for creating bundle.

## Creating archive

1. Download MozillaBuild installer from <https://ftp.mozilla.org/pub/mozilla/libraries/win32/>.
2. Extract installer using 7zip into `moztools-${mozilla-build-version}` folder and move to this folder
3. Remove all files/folders except `msys2`, `bin` and `VERSION`
4. Remove big unused stuff for smaller size:
    - vim (found in `msys2/usr/bin` and `msys2/usr/share`)
    - emacs (`msys2/usr/bin` and `msys2/usr/share`)
5. Download `mozmake.tar.ztd` from <https://firefox-ci-tc.services.mozilla.com/tasks/index/gecko.cache.level-1.toolchains.v3.win64-mozmake/latest>
6. Extract `mozmake.exe` from `mozmake.tar.ztd` into `bin`
7. Zip `moztools-${mozilla-build-version}` folder into `moztools-${mozilla-build-version}.zip`

final structure should look like this:

```console
moztools-${mozilla-build-version}.zip
└───moztools-${mozilla-build-version}
    ├───msys2 (dir)
    ├───bin (dir)
    │   └─ mozmake.exe
    └─VERSION (file)
```

You have created new moztools bundle!
