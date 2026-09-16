# Third party software shipped with Papertrail

Papertrail bundles SANE and its dependencies so that scanning works without asking anyone to
install Homebrew. Those components carry their own licences, and shipping them brings
obligations. This file records what is included and what those obligations are.

Everything listed here is copied into the app bundle by `Scripts/vendor-sane.sh`, which takes
them from a Homebrew installation on the build machine. The exact version of sane-backends
used for a build is recorded in `Vendor/sane/VERSION`.

## sane-backends — GNU General Public License, version 2

`scanimage`, `libsane`, and the backend modules under `Contents/Resources/sane`.

SANE separates its licensing, and the distinction matters here. From the project's own
`LICENSE` file:

- **The backend libraries** are GPL, "but as an exception, it is permissible to link against
  such a library without affecting the licensing status of the program that uses the
  libraries." So linking against `libsane` does not by itself make Papertrail GPL. Note that
  not every backend applies the exception.
- **The frontend programs** — which includes `scanimage`, the tool Papertrail runs — carry no
  such exception and are plain GPL.

Because the app ships `scanimage`, any distribution of Papertrail is a distribution of GPL
software, and the corresponding source has to be offered to whoever receives it. What has to be
offered is the source for the GPL parts: `scanimage` and the backends, unmodified, at
<https://gitlab.com/sane-project/backends>, with the released tarballs at
<https://gitlab.com/sane-project/backends/-/releases>. This notice ships inside the disk image
and is published alongside the download, which is how that offer is made.

Papertrail's own source is not covered by that obligation and is not published. It runs
`scanimage` as a separate process rather than linking it, so the two stay separate works and
the GPL does not reach across.

**This rules out the Mac App Store and TestFlight.** Their terms impose usage restrictions
that the GPL does not permit anyone to add, which is why GPL software has been pulled from the
store before. Direct distribution, signed with a Developer ID and notarised, has no such
conflict.

## libusb — GNU Lesser General Public License, version 2.1 or later

Used by the backends to talk to USB scanners. Shipped as a separate dynamic library and
loaded at run time, which is what the LGPL asks for: anyone receiving the app can replace it
with their own build. Source: <https://github.com/libusb/libusb>.

## Everything else

Pulled in as dependencies of the above, all under permissive licences:

| Library | Licence | Source |
| --- | --- | --- |
| libjpeg-turbo | IJG / BSD style | <https://github.com/libjpeg-turbo/libjpeg-turbo> |
| libpng | PNG Reference Library License | <https://github.com/pnggroup/libpng> |
| libtiff | libtiff (BSD style) | <https://gitlab.com/libtiff/libtiff> |
| xz / liblzma | 0BSD (public domain in older releases) | <https://github.com/tukaani-project/xz> |
| zstd | BSD 3 clause or GPLv2, dual licensed | <https://github.com/facebook/zstd> |
| net-snmp | BSD style | <https://github.com/net-snmp/net-snmp> |

## A caveat

This is a plain reading of the licences involved, not legal advice. The GPL position is worth
confirming with someone qualified before charging for the app or distributing it at any scale.
