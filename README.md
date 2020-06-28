**English** [简体中文](README-Hans.md) [繁體中文](README-Hant.md)

# Nowar DIN · Nowar DIN Cursive

Nowar DIN and Nowar DIN Cursive are DIN-inspired font packs for _World of Warcraft_ and _WoW Classic_ that support all client languages.

> Make Love, Not Warcraft.

![Nowar DIN](poster/poster.png)

## Download the Fonts

[Latest release at GitHub](https://github.com/nowar-fonts/Nowar-DIN/releases)

Mirrors: [Gitee (Release Repo)](https://gitee.com/nowar-fonts/Nowar-DIN)

Nowar DIN and Nowar DIN Cursive are shipped in 4 weights and 5 regional variants with a prebuilt feature variant, respectively.

### Weights

* 300: Light
* 400: Regular
* 500: Medium
* 700: Bold

### Regional Variants

Bliz and Neut are “standard variants” with regional Chinese character orthographies.

|      | European and 한국어 | 简体中文       | 繁體中文 | Note                                       |
| ---- | ------------------- | -------------- | -------- | ------------------------------------------ |
| Bliz | Mainland China      | Mainland China | Taiwan   | Acts like WoW’s default fallback setting.  |
| Neut | Classical           | Mainland China | Taiwan   | Prefers classical orthography on fallback. |

CL is the “classical variant” with classical Chinese character orthography (aka Kāngxī Dictionary forms).

|    | All languages |
| -- | ------------- |
| CL | Classical     |

PSimp and PSimpChat are special variants for 繁體中文 that remap traditional Chinese character to simplified ones.

|           | 繁體中文-related non-chat fonts | 繁體中文 chat fonts       | European, 简体中文 and 한국어 |
| --------- | ------------------------------- | ------------------------- | ----------------------------- |
| PSimp     | Mainland China (Remapped)       | Mainland China            | N/A                           |
| PSimpChat | Mainland China (Remapped)       | Mainland China (Remapped) | N/A                           |

* European: English, Español (AL), Português, Deutsch, Español (EU), Français, Italiano, and Русский.
* 繁體中文-related fonts include `FRIZQT__` and `ARIALN`, which are hard-coded in some addons.

### Features

| Tag | Name        | Description                                                            |
| --- | ----------- | ---------------------------------------------------------------------- |
| OSF | Oldstyle    | Oldstyle (non-lining), monospaced figure.                              |
| RP  | Roleplaying | `丶` (U+4E36) is mapped to the same glyph as `·` (U+00B7, MIDDLE DOT). |

Prebuilt feature variant: `Bliz,OSF`.

## How to Build

### Dependencies

+ basic Unix utils,
+ [Python](https://www.python.org/),
+ [otfcc](https://github.com/caryll/otfcc) and
+ [7-Zip](https://www.7-zip.org/) (add to `PATH`).

Note:
+ Choose 64-bit version if possible. 32-bit version may lead to out-of-memory issue.

### Build Feature Variant

Prepare submodules:
```bash
git submodule update --init --recursive
```

Run `configure.py` to generate Makefile:
```bash
python configure.py
```

Put Source Han Sans OTF files (all families but HW) to `source/shs/`.

Then make a specific variant:
```bash
make <family>-<region>,<features>-<weight> -j<threads>
```
where family is `Sans` (Nowar DIN) or `Cursive` (Nowar DIN Cursive).
Note: Features must be sorted alphabetically. (`OSF`, `RP`).

e.g.
```bash
make Cursive-CN,OSF,RP-400 -j4
```

The output is `out/NowarDIN-<region>,<features>-<weight>-<version>.7z` or `out/NowarDINCursive-<region>,<features>-<weight>-<version>.7z`.

### Create Regional Variant

To build exactly what you need, modify `configure.py`:
```python
class Config:
    # put your variant here
    fontPackRegion = [ <your_region> ]

# define the variant here.
regionalVariant = { ... }
```

For example, the “CNmulti” multi-orthography variant,

|         | European       | 简体中文       | 繁體中文 | 한국어   |
| ------- | -------------- | -------------- | -------- | -------- |
| CNmulti | Mainland China | Mainland China | Taiwan   | S. Korea |

```python
class Config:
    fontPackRegion = [ "CNmulti" ]

regionalVariant = {
    "CNmulti": {
        "base": "CN",
        "enUS": True,
        "ruRU": True,
        "zhCN": "CN",
        "zhTW": "TW",
        "koKR": "KR",
    }
}
```

Then, run `python configure.py` to generate `Makefile`. The new regional variant (with optional feature) can be built by:
```bash
make <family>-<region>,<features>-<weight> -j<threads>
```
e.g.
```bash
make Cursive-CNmulti-400 -j4
make Cursive-CNmulti,OSF-400 -j4
```

## Credit

Latin, Greek and Cyrillic characters are from [Iosevka](https://github.com/be5invis/Iosevka) by Belleve Invis, with some [modifications](https://github.com/nowar-fonts/Iosevka-CFF).

CJK Ideographs, Kana and Hangul are from [Source Han Sans](https://github.com/adobe-fonts/source-han-sans) by Adobe.

The traditional Chinese to simplified Chinese conversion table is from [Open Chinese Convert project](https://github.com/BYVoid/OpenCC).
