# Halo: Combat Evolved pro Linux, Windows a Android
NEJSEM MAJITEL A TENTO FORK JE POUZE PŘEKLAD!!!! 
Odkaz na originální repo: https://github.com/cybersecurity/halo-ce-universal
[![Přpojte se na náš Discord](https://invidget.switchblade.xyz/9gqcHyr5km)](https://discord.gg/9gqcHyr5km)

Tento projekt je port dekompilace Halo: Combat Evolved pro Linux,
Windows and Android. Je to dekompilace Xbox build 2342
(`cachebeta.exe`, SHA-256
`4cc87b45f721270392a96f1674ed2b5cd4a7bb4355faeab4531d1cf1884d9520`).

<img width="1289" height="995" alt="The game on Linux" src="https://github.com/user-attachments/assets/0d3ad50f-f8b8-46cf-aef8-e3661da2a7d7" />

Port začíná z dekompilace [bnunu/halo-1](https://github.com/bnunu/halo-1).
Ten projekt je fork tohoto projektu [punpckhdq/halo](https://github.com/punpckhdq/halo).

## Stažení

GitHub Actions sestavuje hru pro každý commit.
Tyto odkazy slouží ke stažení nejnovější verze:


| Platforma | Vydání | Debug |
| --- | --- | --- |
| Linux | [halo-linux-release.zip](https://github.com/cybersecurity/halo-ce-universal/releases/latest/download/halo-linux-release.zip) | [halo-linux-debug.zip](https://github.com/cybersecurity/halo-ce-universal/releases/latest/download/halo-linux-debug.zip) |
| Windows | [halo-windows-release.zip](https://github.com/cybersecurity/halo-ce-universal/releases/latest/download/halo-windows-release.zip) | [halo-windows-debug.zip](https://github.com/cybersecurity/halo-ce-universal/releases/latest/download/halo-windows-debug.zip) |
| Android | [halo-android-release.zip](https://github.com/cybersecurity/halo-ce-universal/releases/latest/download/halo-android-release.zip) | [halo-android-debug.zip](https://github.com/cybersecurity/halo-ce-universal/releases/latest/download/halo-android-debug.zip) |

Pro hraní hry použijte "Release". The debug build stops at the first failed
assertion and writes it to the log. Debug sestavení používejte k hledání
a nahlašování chyb.

Hra se sama aktualizuje. Jakmile se spustí, začne hledat aktualizaci, a zeptá se
jestli ji chcete nainstalovat. Viz
[port/linux/README.md](port/linux/README.md#updates).

Každé sestavení `main`který je na všech třech platformách je nové vydání.
Tato stránka: [Releases](https://github.com/cybersecurity/halo-ce-universal/releases)
vždy má posledních pět vydání. Pokud nejnovějsí sestavení má problem, stáhněte si starší
sestavení z této stránky.

## Herní data

Tento port neobsahuje data hry. Stáhněte si diskový obraz Xbox
(`.xiso` or `.iso`) hry Halo: Combat Evolved. Všechny verze hry fungují.
Mapy evropské verze (PAL) byly vytvořeny pro pomalejší konzoli.
Tento port je změní, aby fungovaly jako severo Americké (NTSC) mapy,
aby hráči těchto dvou verzí mohli hrát spolu.

1. Zapněte hru
2. Při prvním zapnutí, hra se zeptá na váš diskový obraz. Vyberte ho.
3. Hra extrahuje složku `maps/`. Pak se hra nastartuje.

Na Linuxu a Windowsu, hra dá `maps/` vedle spustitelného souboru. Na
Androidu, nejdřív zkopírujte váš obraz do telefonu. Aplikace dá složku maps `maps/`
do své složky "data".
Viz [port/android/README.md](port/android/README.md).

## Platformy

Každá platforma má své vlastní instrukce:

| Platforma | Instrukce |
| --- | --- |
| Linux (32-bitový x86 spustelný soubor, OpenGL 4.5, SDL3) | [port/linux/README.md](port/linux/README.md) |
| Windows (32-bitový x86 spustelný soubor, OpenGL 4.5, SDL3) | [port/windows/README.md](port/windows/README.md) |
| Android (Aplikace arm64 OpenGL ES 3, SDL3) | [port/android/README.md](port/android/README.md) |

Soubor "přečti mě" pro Linux vám taky dá ovládání, nastavení, a funkce pro více hráčů.
Ty jsou skoro stejné na všech platformách.

## Hra pro více hráčů

Hra může spojit relace na lokální síti a internetu.

- "System link" hra může mít až 128 hráčů až na 128mi zařízeních.
- Zařízení s Linuxem, Androidem, a Windowsem mohou hrát spolu.
- Zvací odkaz nechá zařízení se připojit na internetovou hru. Není potřeba žádný server.
- "Netcode" je nový. Každé zařízení hýbe vlastními hráči zároveň
  a hostitel rozhoduje o nastavení. Referujte k
  [port/linux/NETCODE.md](port/linux/NETCODE.md).

## Sestavení hry

Xbox SDK není potřeba.
Port poskytuje deklarace SDK, které hra používá. Viz [port/include/xdk](port/include/xdk/README.md).

Pro sestavení hry:

1. Nainstalujte Python a [ninja](https://ninja-build.org/).
2. Nainstalujte nástroje pro vaši platformu. viz "README" (soubor přečti mě)
pro vaši platformu
4. V kořenové složce repozitáře, zadejte `python configure.py`.
5. zadejte `ninja` s cílem pro platformu:

| Cíl | Výsledek |
| --- | --- |
| `ninja linux` | `build/linux/halo` |
| `ninja windows` (na Windowsu) | `build/windows/halo.exe` a `SDL3.dll` |
| `ninja android_apk` | `port/android/app/build/outputs/apk/debug/app-debug.apk` |

Pokud zadáte `ninja` bez cíle, ninja sestaví hru pro váš počítač.

`tools/ci_build.py` sestaví stejnou hru jako GitHub Actions. Pro příklad,
zadejte `python tools/ci_build.py linux release`.

### Možnosti sestavení

Tyto možnosti dejte `configure.py`:

| Možnost | Výsledek |
| --- | --- |
| (nic) | Debug sestavení. Neúspěšné tvrzení zastaví hru. |
| `--release` | Sestavení k vydání. Hra nekontroluje tvrzení, jako prodávaná hra. |
| `--portable` | Verze pro Linux a Windows fungují na všech x86-64 procesorech. Tuto možnost používejte pro sestavení, které dáváte ostatním lidem. |
| `--lto=thin`, `--lto=off` | Less link-time optimization. The link is faster. |
| `--pgo=off` | No profile-guided optimization. |
| `--pgo=train` | Records a new optimization profile. Refer to "Optimization profiles". |

Without `--portable`, the Linux and Windows builds use all the instructions
of the processor that builds them (`-march=native`). Such a build does not
always start on a different computer.

### Profily optimizace

Sestavení používají profily hry k optimizaci kódu:

- `pgo/halo_linux.profdata` pro Linux a Android.
- `pgo/halo_windows.profdata` pro Windows.

Profily vyžadují clang 22 nebo novější. Se starším clangem, sestavení
nebudou používat profily.

Pokud chcete nahrát nový profil:

1. Smažte profil.
2. Zadejte `python configure.py --pgo=train`.
3. Zadejte `ninja linux` or `ninja windows`.
Sestavení potom spustí hlavní menu a první minutu
každého levelu v kampaňi. Tato procedura zabere kolem patnácti minut.
Data hry musí být v `assets/`.
