# aur-konqix

`PKGBUILD` for [konqix](https://github.com/radek-bucek/konqix), a Qt 6
instant messenger built on libpurple. Follows the AUR's required repo
layout (`PKGBUILD` + `.SRCINFO`), so this same content can be pushed
straight to the [AUR](https://aur.archlinux.org/) if published there.

## Install

`libpurple` itself isn't in the official Arch repos — it's only on the
AUR (as a split of the `pidgin` package), so plain `pacman`-based
dependency resolution can't pull it in automatically. Install it first,
either with an AUR helper that resolves AUR-to-AUR dependencies:

```sh
yay -S libpurple   # or: paru -S libpurple
```

or manually:

```sh
git clone https://aur.archlinux.org/pidgin.git
cd pidgin
makepkg -si   # prompts to build/install the libpurple split package
```

Then build konqix itself:

```sh
git clone https://github.com/asmbk/aur-konqix.git
cd aur-konqix
makepkg -si
```

`makepkg -si` builds the package and installs it (plus any *officially
packaged* missing dependencies — `qt6-base`, `kwindowsystem`,
`hunspell`) via `pacman`.

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
