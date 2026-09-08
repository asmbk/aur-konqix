# aur-konqix

`PKGBUILD` for [konqix](https://github.com/radek-bucek/konqix), a Qt 6
instant messenger built on libpurple. Follows the AUR's required repo
layout (`PKGBUILD` + `.SRCINFO`), so this same content can be pushed
straight to the [AUR](https://aur.archlinux.org/) if published there.

Want the live `main` branch instead of the latest tagged release? See
[aur-konqix-git](https://github.com/asmbk/aur-konqix-git) — the two
`provides`/`conflicts` each other and can't both be installed at once.

## Install

`libpurple` itself isn't in the official Arch repos — it's only on the
AUR (as a split of the `pidgin` package) — so plain `pacman`-based
dependency resolution can't pull it in automatically.

**With an AUR helper (recommended)** — `yay`/`paru` resolve AUR-to-AUR
dependencies recursively, so this one command builds/installs
`libpurple` from the AUR first, then konqix itself:

```sh
git clone https://github.com/asmbk/aur-konqix.git
cd aur-konqix
yay -Bi .    # or: paru -Bi .
```

**Without an AUR helper**, the same three AUR packages `yay -Bi` would
resolve on its own have to be built and installed manually, in
dependency order (plain `makepkg` can't reach across AUR dependencies
the way `yay`/`paru` do):

```sh
# 1. libgnt: makedepend needed to build finch alongside libpurple in
#    the shared pidgin source tree (AUR-only, PackageBase: libgnt)
git clone https://aur.archlinux.org/libgnt.git && cd libgnt && makepkg -si && cd ..

# 2. libgadu: runtime dependency of libpurple for the Gadu-Gadu
#    protocol (AUR-only, PackageBase: libgadu)
git clone https://aur.archlinux.org/libgadu.git && cd libgadu && makepkg -si && cd ..

# 3. libpurple: --pkg restricts the shared pidgin/finch/libpurple
#    build to just the split package konqix actually needs
git clone https://aur.archlinux.org/pidgin.git && cd pidgin && makepkg --pkg libpurple -si && cd ..

# 4. konqix: remaining deps (qt6-base, kwindowsystem, hunspell) are
#    all official, so plain makepkg installs them via pacman
git clone https://github.com/asmbk/aur-konqix.git && cd aur-konqix && makepkg -si
```

## Updating for a new konqix release

1. Bump `pkgver` (and reset `pkgrel=1`) in `PKGBUILD`.
2. Update `sha256sums` for the new release tarball, e.g.:
   ```sh
   curl -sL -o /tmp/konqix-<version>.tar.gz \
       https://github.com/radek-bucek/konqix/archive/refs/tags/v<version>.tar.gz
   sha256sum /tmp/konqix-<version>.tar.gz
   ```
3. Regenerate `.SRCINFO`:
   ```sh
   makepkg --printsrcinfo > .SRCINFO
   ```
4. Test the build with `makepkg -si` before committing.
