# vapoursynth-git AUR PKGBUILD

Personal patched PKGBUILD for building latest VapourSynth from upstream git master on Arch Linux.

This fixes the old AUR `vapoursynth-git` PKGBUILD, which still expected the removed autotools files such as `Makefile.am`.

Current upstream VapourSynth uses:

- `pyproject.toml`
- `meson-python`
- `meson`
- `ninja`

## Build

```bash
makepkg -si
```
