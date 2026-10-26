---
title: Install Libreboot Open Source BIOS/UEFI boot firmware
x-toc-enable: true
...

Open source BIOS/UEFI boot firmware
------------------------------

**ALWAYS remember to make a backup of the current flash, when overwriting it,
regardless of what firmware you currently have and what firmware you're
re-flashing it with; this includes updates between Libreboot releases. Use
the `-r` option in flashprog instead `-w`, to read from the flash.**

These documents will teach you how to install Libreboot, on these devices/motherboards.
Libreboot is a [Free Software](https://writefreesoftware.org/learn) project
that replaces proprietary BIOS/UEFI firmware.

Trouble?
----------

Inquiries can be done via [Libreboot IRC](../../contact.md) and other channels.

If you want professional installation, Minifree Ltd sells [Libreboot
pre-installed](https://minifree.org/) on select hardware, and it also provides
a [Libreboot preinstall service](https://minifree.org/product/installation-service/)
if you want to send your machine in to have Libreboot installed for you.

Leah Rowe, the founder and lead developer of Libreboot, also owns and
operates Minifree Ltd; sales provide funding for the Libreboot project.


Chromebook install/update
----------------

Be careful as certain config in `RO_VPD` must be retained during installation
and updates. Check [chromebooks-x86.md](chromebooks-x86.md) for guidance,
cross-referenced with MrChromebook documentation.


PCI-E rebar
-----------

The EDK2-based images work with modern graphics cards, which often lack VGA
ROMs these days; EDK2 does GOP ROM
execution by default, in Libreboot.

However, we do not enable re-locatable BAR by default. This is required
for higher-end modern cards with large amounts of video RAM. [Please read
the documentation for this](../extras.md#reversible-pcie-rebar).

If in doubt, please just follow the link and enable rebar if you're using
a graphics card.


SMMSTORE settings
---------------

Modern Libreboot releases use EDK2 with SMMSTORE.

If you used an EDK2 build before Libreboot 26.10, note that SMMSTORE was set
to 256KB. It was upped back to 512KB as is default, on most platforms. For
safety, please flash your entire image when switching to a stable release
with EDK2.

If you're installing for first time, please also flash the whole image.

If updating from an existing stable Libreboot release with EDK2, note that you
can avoid overwriting your EFI setup config by doing this:

```
flashprog -p internal --fmap -i COREBOOT -w whatever.rom
```

This avoids flashing the SMMSTORE section, where your settings are. This is
because the EDK2 payload writes settings to that section in the flash.

Conversely, you may reset it thus:

```
flashprog -p internal --fmap -i SMMSTORE -w whatever.rom
```

Be very careful with this. Note that fmap exists inside the BIOS region on
IFD-based systems.


### Laptops (x86\_64 chromebooks)

See: [x86 chromebooks](chromebooks-x86.html) - also pertains to EDK2 more
generally, across all boards, not just Chromebooks.

Libreboot 26.10 and higher have full integration of all EDK2-based MrChromebox
Chromebook setups. That is to say: we support every Chromebook that MrChromebox
does, in Libreboot.

The MrChromebook coreboot tree contains support for the following Chromebook
boards, most of which Libreboot now also has direct support for:

**WARNING: Make/model don't always match; sometimes the same model can
  use different hardware. Please try only to use serial numbers corresponding
  to proper codenames when flashing, and cross reference that with what's
  included in the Libreboot build system.***

The list of models/makes for chromebooks may be incomplete. Please make sure
to use codenames and serial numbers when identifying your device.

#### Google Nyan Blaze (NVIDIA Tegra K1 (CD570M))

Codenames:

* Nyan Blaze (HP Chromebook 14 G3)

#### Google Mistral (Qualcomm Dragonwing QCS405)

Codenames:

* Mistral

#### Google Stout (Intel SandyBridge (2nd Gen))

Codenames:

* Stout (Lenovo Thinkpad X131e Chromebook)

#### Google Cyan (Intel Braswell)

Codenames:

* Banon (Acer Chromebook 15 (CB3-532))

Codenames:

* Celes (Samsung Chromebook 3)

Codenames:

* Cyan (Acer Chromebook R11 (C738T))

Codenames:

* Edgar (Acer Chromebook 14 (CB3-431))

Codenames:

* Kefka (Dell Chromebook 11 3180/3189)

Codenames:

* Reks (Lenovo N22/N42 Chromebook)

Codenames:

* Relm (Multiple devices - see list below):

The Relm codename covers these devices:

- Acer Chromebook 11 N7 (C731, C731T)
- CTL NL61 Chromebook
- EduGear Chromebook R
- Edxis Education Chromebook (NL6D)
- HP Chromebook 11 G5 EE
- Mecer V2 Chromebook
- Positivo Chromebook C216B
* Setzer (HP Chromebook 11 G5)

Codenames:

* Terra (ASUS Chromebook C202SA/C300SA/C301SA)

Codenames:

* Ultima (Lenovo Yoga 11e G3)

Codenames:

* Wizpig (Multiple devices - see list below):

The Wizpig codename covers these devices:

- CTL J5 Chromebook
- Edugear CMT Chromebook
- Haier Chromebook 11 C
- Multilaser Chromebook M11C
- PCMerge Chromebook PCM-116T-432B
- Prowise Chromebook Proline
- Viglen Chromebook 360

#### Google Daisy (Samsung Exynos 5250)

Codenames:

* Daisy (Samsung Chromebook (2012))

#### Google Skywalker (MediaTek Kompanio 540 (MT8189))

Codenames:

* Anakin
* Baze
* Dooku
* Grogu
* Jaina
* Mace
* Obiwan
* Padme
* R2d2
* Sheev
* Skywalker
* Tarkin
* Vader
* Yoda

#### Google Brox (Intel RaptorLake (13th Gen))

Codenames:

* Brox
* Brox RTK EC
* Brox EC ISH
* Brox TI PDC
* Caboc
* Greenbayupoc
* Jubilant (Multiple devices - see list below):

The Jubilant codename covers these devices:

- Acer Chromebook Plus 514
- Acer Chromebook Plus 516 (CB516-1H,CB516-1HT)

Codenames:

* Juchi
* Lotso (Lenovo Chromebook Plus 2-in-1)

#### Google Rambi (Intel Baytrail)

Codenames:

* Banjo (Acer Chromebook 15 (CB3-531))

Codenames:

* Candy (Dell Chromebook 11 3120)

Codenames:

* Clapper (Lenovo N20 Chromebook)

Codenames:

* Enguarde (Multiple devices - see list below):

The Enguarde codename covers these devices:

- ASI Chromebook
- CTL N6 Education Chromebook
- Crambo Chromebook
- Education Chromebook
- Edxis Education Chromebook (NL6)
- JP Sa Couto Chromebook
- Lenovo N21 Chromebook
- M&A Chromebook
- RGS Education Chromebook
- Senkatel C1101 Chromebook
- True IDC Chromebook
- Videonet Chromebook
- eduGear Chromebook R
* Expresso (Multiple devices - see list below):

The Expresso codename covers these devices:

- Bobicus Chromebook 11
- Consumer Chromebook
- Edxis Chromebook
- HEXA Chromebook Pi
* Glimmer (Lenovo ThinkPad 11e Chromebook)

Codenames:

* Gnawty (Acer Chromebook 11 (CB3-111/131,C730/C730E/C735))

Codenames:

* Heli (Haier Chromebook G2)

Codenames:

* Kip (HP Chromebook 11 G3 / G4 / G4 EE)

Codenames:

* Ninja (AOpen Chromebox Commercial)

Codenames:

* Orco (Lenovo 100S Chromebook)

Codenames:

* Quawks (ASUS Chromebook C300)

Codenames:

* Squawks (ASUS Chromebook C200)

Codenames:

* Rambi
* Sumo (AOpen Chromebase Commercial)

Codenames:

* Swanky (Toshiba Chromebook 2)

Codenames:

* Winky (Samsung Chromebook 2 (XE500C12))

#### Google Veyron Mickey (Rockchip RK3288)

Codenames:

* Veyron_Mickey (Asus Chromebit CS10)

#### Google Kahlee (AMD StoneyRidge)

Codenames:

* Aleena/Kasumi (Acer Chromebook  315 (CB315-2H), 311 (C721) / Spin 311 (R721T))

Codenames:

* Barla/Careena (HP Chromebook 11A G6/G8 EE, 14A G5/G6)

Codenames:

* Grunt
* Liara (Lenovo 14e Chromebook, Chromebook S345-14)

Codenames:

* Nuwani
* Treeya (Lenovo 100e/300e Gen2 AMD)

#### Google Jecht (Intel Broadwell (5th Gen))

Codenames:

* Guado (ASUS Chromebox CN62)

Codenames:

* Jecht
* Rikku (Acer Chromebox CXI2)

Codenames:

* Tidus (Lenovo ThinkCentre Chromebox)

#### Google Butterfly (Intel SandyBridge (2nd Gen))

Codenames:

* Butterfly (HP Pavilion Chromebook 14)

#### Google Herobrine (Qualcomm Snapdragon 7c+ Gen3 (SC7280))

Codenames:

* Herobrine
* Herobrine_Rev0
* Senor
* Piglin
* Hoglin
* Villager
* Evoker
* Zoglin
* Zombie

#### Google Smaug (Nvidia Tegra X1 (T210))

Codenames:

* Smaug (Google Pixel C)

#### Google Zork (AMD Ryzen Mobile 3000 (Picasso))

Codenames:

* Berknip (HP Pro c645 Chromebook Enterprise)

Codenames:

* Dalboz
* Dirinboz (HP Chromebook 14a-nd0097nr)

Codenames:

* Ezkinil (Acer Chromebook Spin 514)

Codenames:

* Gumboz (HP Chromebook x360 14a)

Codenames:

* Morphius (Lenovo ThinkPad C13 Yoga Chromebook)

Codenames:

* Shuboz
* Trembyle
* Vilboz (Lenovo 100e/300e Gen3 AMD)

Codenames:

* Woomax (ASUS Chromebook Flip CM5)

#### Google Parrot (Intel SandyBridge (2nd Gen))

Codenames:

* Parrot (Acer C7/C710 Chromebook)

#### Google Brya (Intel AlderLake/RaptorLake/TwinLake (12 - 13th Gen))

Codenames:

* Agah
* Anahera (HP Elite c640 14 inch G3 Chromebook)

Codenames:

* Anahera4ES
* Anraggar (ASUS Chromebook CR12 CR1204C)

Codenames:

* Aurash (AOpen Chromebox Commercial 3)

Codenames:

* Banshee (Framework Laptop Chromebook Edition)

Codenames:

* Brask
* Brya 0
* Craask (Acer Chromebook Spin 512)

Codenames:

* Craaskov (Acer Chromebook Spin 312)

Codenames:

* Constitution (Google Meet Series Two)

Codenames:

* Crota (Multiple devices - see list below):

The Crota codename covers these devices:

- Dell Latitude 5430 Chromebook
- Dell Latitude 5430 2-in-1 Chromebook
* Dirks (Acer Chromebox Mini CXM2 (TWL))

Codenames:

* Dirkson
* Dochi (Acer Chromebook Plus Spin 514)

Codenames:

* Domika (Multiple devices - see list below):

The Domika codename covers these devices:

- HP Fortis G1i 11 Chromebook
- HP Fortis Flip G1i 11 Chromebook
- HP Fortis G1i 14 Chromebook
* Felwinter (ASUS Chromebook Flip CX5(CX5601))

Codenames:

* Gaelin
* Gimble (HP Chromebook x360 14c-cd0)

Codenames:

* Gimble4ES
* Gladios (HP Chromebox Enterprise G4)

Codenames:

* Glassway (CTL Chromebook Plus PX141GX)

Codenames:

* Gothrax (Multiple OEM Chromebooks)

Codenames:

* Guren (CTL Chromebook PX121E)

Codenames:

* Hades
* Kano (Acer Chromebook Spin 714)

Codenames:

* Kaladin
* Kinox (Lenovo ThinkCentre M60q Chromebox)

Codenames:

* Kuldax (ASUS Chromebox 5/5a (CN67))

Codenames:

* Kulnex
* Joxer (HP Chromebook x360 14b)

Codenames:

* Lisbon (Multiple devices - see list below):

The Lisbon codename covers these devices:

- CTL Chromebox CBx3 / CTL Chromebox Enterprise CBx3
- Centerm Chromebox D661 / Centerm Chromebox Enterprise D661
- Promethean Chromebox 3
* Marasov (ASUS Chromebook Plus CX34 CX3402)

Codenames:

* Mithrax (ASUS Chromebook CX34 Flip (CX3401))

Codenames:

* Moli (Acer Chromebox CXI5)

Codenames:

* Moxie (Acer Chromebox CXI6)

Codenames:

* Moxoe
* Nivviks
* Nereid
* Nokris
* Omnigul (Multiple devices - see list below):

The Omnigul codename covers these devices:

- Acer Chromebook Plus 515 (CB515-2H/2HT)
- Acer Chromebook Plus Enterprise 515 (CBE595-2/2T)

Codenames:

* Osiris (Acer Chromebook 516 GE)

Codenames:

* Pirrha
* Primus (Lenovo ThinkPad C14 Gen 1 Chromebook)

Codenames:

* Pujjo (Lenovo 500e Yoga/Flex 3i Chromebook)

Codenames:

* Pujjoniru (Lenovo Chromebook Plus 2-in-1 14ITN10)

Codenames:

* Quandiso (CTL Chromebook NL73)

Codenames:

* Quandiso2 (CTL Chromebook NL73 Gen 2)

Codenames:

* Redrix (HP Elite Dragonfly Chromebook)

Codenames:

* Redrix4ES
* Riven (Acer Chromebook Spin 511)

Codenames:

* Rull (Acer Chromebook 514 C937)

Codenames:

* Skolas
* Skolas4ES
* Taeko (Lenovo IdeaPad/Flex 5i Chromebook 14)

Codenames:

* Taeko4ES
* Taniks (Lenovo IdeaPad Gaming Chromebook 16)

Codenames:

* Teliks (ASUS Chromebook CR11 (CR1104CTA))

Codenames:

* Tereid
* Tivviks
* Trulo
* Uldren (Dell Chromebook 3120)

Codenames:

* Uldrenite
* Vell (HP Dragonfly Pro Chromebook)

Codenames:

* Volmar (Acer Chromebook Vero 514)

Codenames:

* Xivu (Asus Chromebook CR11 [CR1102C])

Codenames:

* Yaviks (HP Chromebook 15.6)

Codenames:

* Yavilla (HP Fortis 11 G10 Chromebook)

Codenames:

* Zydron
* Xol (Galaxy Chromebook Plus)

Codenames:

* Nova
* Bujia (Multiple devices - see list below):

The Bujia codename covers these devices:

- AOPEN Chromebox OPS / AOPEN Chromebox Enterprise OPS
- IMAGO Chromebox OPS
- Satsuki Chromebox OPS / Satsuki Chromebox Enterprise OPS
- ViewSonic Bujia
* Yavista
* Sundance (NEC Chromebook Y4)

Codenames:

* Pujjoga (Lenovo 500e Chromebook Gen 4s)

Codenames:

* Pujjogatwin (Lenovo 500e Chromebook Gen 4s)

Codenames:

* Pujjolo
* Orisa
* Telith (ASUS Chromebook CX15 (CX1505CTA))

Codenames:

* Meliks
* Epic
* Pujjocento

#### Google Storm (Qualcomm IPQ8064)

Codenames:

* Storm (OnHub Router TGR1900)

#### Google Calypso (Qualcomm Calypso)

Codenames:

* Calypso
* Mensa
* C1nv

#### Google Bluey (Qualcomm Snapdragon X Plus (X1P-42-100))

Codenames:

* Bluey
* Quenbi
* BlueyH
* QuenbiH
* Quartz
* Mica
* Annite
* Pic
* Aneto
* Meru

#### Google Beltino (Intel Haswell (4th Gen))

Codenames:

* Mccloud (Acer Chromebox CXI)

Codenames:

* Monroe (LG Chromebase 22CV241 & 22CB25S)

Codenames:

* Panther (ASUS Chromebox CN60)

Codenames:

* Tricky (Dell Chromebox 3010)

Codenames:

* Zako (HP Chromebox G1)

#### Google Link (Intel IvyBridge (3rd Gen))

Codenames:

* Link (Google Chromebook Pixel (2013))

#### Google Dedede (Intel JasperLake)

Codenames:

* Beadrix (Multiple devices - see list below):

The Beadrix board supports the following devces:
- DINATECH Gen1 Chromebook SG20JL1C
- Multilaser Chromebook M11C-PC919
- Evolve III Chromebook 11 E1
- Positivo Chromebook N2320
- Allied Chromebook 11 N5100E
* Blipper (Lenovo 3i-15 Chromebook)

Codenames:

* Boten (Lenovo 500e Chromebook Gen 3)

Codenames:

* Boxy (Lenovo Chromebox Micro)

Codenames:

* Bugzzy (Samsung Galaxy Chromebook 2 360)

Codenames:

* Cappy2
* Corori
* Cret (Dell Chromebook 3110)

Codenames:

* Dedede
* Dexi (AOPEN Chromebox Mini 2)

Codenames:

* Dibbi
* Dita (Acer Chromebox Mini CXM1)

Codenames:

* Drawcia (Multiple devices - see list below):

The Drawcia codename covers these devices:

- HP Chromebook x360 11 G4 EE (Drawcia)
- HP Chromebook 11 G9 EE (Drawlat)
- HP Chromebook 14 G7 (Drawman)
- HP Fortis 14 G10 Chromebook (Drawper)

Codenames:

* Driblee
* Galtic (ASUS Chromebook CX1400CKA)

Codenames:

* Gooey
* Haboki
* Kracko/Kracko360 (Multiple devices - see list below):

The Kracko board is for clamshell form factor.
The Kracko360 board is for convertible form factor.
They support the following devices:

- ADVAN Chromebook 116J
- Centerm Chromebook M610
- CTL Chromebook NL72
- CTL Chromebook NL72T
- Edxis Chromebook 11
- LG Chromebook 11TC50Q/11TQ50Q
- Poin2 Chromebook 11B
- Poin2 Chromebook 11E
- Zyrex Chromebook M432-64
- Multilaser Chromebook M11HC-PC916
- Positivo Chromebook N2310
- Positivo Chromebook N2312
* Lalala
* Lantis (HP Chromebook 14a)

Codenames:

* Madoo (HP Chromebook x360 14b)

Codenames:

* Magolor (Acer Chromebook Spin 511)

Codenames:

* Metaknight (NEC Chromebook Y3)

Codenames:

* Pirika (Multiple devices - see list below):

The Drawcia codename covers these devices:

- Axioo Chromebook P14
- CTL Chromebook Enterprise
- Gateway Chromebook 14
- Arai Nimbus S1 Chromebook Enterprise
- Edxis E-Lite Chromebook 14
* Sasuke (Samsung Galaxy Chromebook Go)

Codenames:

* Sasukette (Samsung Galaxy Chromebook Go 11)

Codenames:

* Storo (ASUS Chromebook CR1100CKA)

Codenames:

* Shotzo
* Taranza
* Waddledee
* Waddledoo
* Awasuki (ASUS Chromebook CX1405CKA/CB1405CKA)

#### Google Ocelot (Intel WildcatLake (Ultra 3rd Gen))

Codenames:

* Hekla
* Kodkod
* Matsu
* Ocelot
* Ocelotite
* Ocelotmchp
* Ojal
* Ocelot4ES
* Ocelotite4ES
* Ocelotmchp4ES
* Ocicat

config VENDOR_GOOGLE
bool Google

#### Google Drallion (Intel CometLake (10th Gen) with ISH)

Codenames:

* Drallion/Drallion360 (Dell Latitude 7410 Chromebook Enterprise)

#### Google Fizz (Intel KabyLake/Kabylake-R (7th/8th Gen))

Codenames:

* Fizz (Multiple devices - see list below):

The Fizz codename covers these devices:

- AOpen Chromebox Commercial 2 (Jax)
- HP Chromebox G2 (Kench)
- Acer Chromebox CXI3 (SION)
- ASUS Chromebox 3 (CN65) (TEEMO)
- CTL Chromebox CBx1 (WUKONG)

Codenames:

* Karma (Acer Chromebase 24I2)

Codenames:

* Endeavour (Google Meet Series One (Lenovo))

#### Google Asurada (MediaTek Kompanio 820 (MT8192))

Codenames:

* Asurada
* Hayato (ASUS Chromebook Flip CM3 (CM3200))

Codenames:

* Spherion (Acer Chromebook 514 (CB514-2H, CB514-2HT))

#### Google Kingler (MediaTek Kompanio 520 (MT8186))

Codenames:

* Kingler
* Kyogre
* Ponyta
* Squirtle (Acer Chromebook Spin 311 (R724T))

Codenames:

* Steelix/Magneton (see list below)

The Steelix codename covers these devices:

- Lenovo 300e Yoga Chromebook Gen 4 (Steelix, convertible)
- Lenovo IdeaPad Slim 3 Chromebook (14M868) (Magneton, clamshell)

Codenames:

* Voltorb (Acer Chromebook 311 (C723/C732T))

#### Google Krabby (MediaTek Kompanio 520 (MT8186))

Codenames:

* Chinchou/Chinochou360 (see list below)

The Chinochou codename covers these devices:

- Asus Chromebook CZ1104CM2A/CZ1204CM2A
- Asus Chromebook CZ1104FM2A/CZ1204FM2A/CZ1104CM2A/CZ1204CM2A
- Asus Chromebook CZ1104FM2A/CZ1204FM2A Flip
* Krabby
* Magikarp
* Skitty
* Tentacruel/Tentacool (see list below)

The Tentacruel codename covers these devices:

- ASUS Chromebook CM14 Flip (CM1402F) (Tentacruel, convertible)
- ASUS Chromebook CM14 (CM1402C) (Tentacool, clamshell)

Codenames:

* Veluza

#### Google Staryu (MediaTek Kompanio 528 (MT8186T))

Codenames:

* Starmie (ASUS Chromebook Enterprise CM30 (CM3001))

Codenames:

* Wugtrio
* Wyrdeer

#### Google Volteer (Intel TigerLake (11th Gen))

Codenames:

* Chronicler (FMV Chromebook 14F)

Codenames:

* Collis (Asus Chromebook Flip CX3)

Codenames:

* Copano (ASUS Chromebook Flip CX5400)

Codenames:

* Delbin (ASUS Chromebook Flip CX5)

Codenames:

* Drobit (ASUS Chromebook CX9400)

Codenames:

* Eldrid (HP Chromebook x360 14c)

Codenames:

* Elemi (HP Pro c640 G2 Chromebook)

Codenames:

* Halvor
* Lindar (Lenovo 5i-14/Slim 5 Chromebook)

Codenames:

* Malefor
* Terrador
* Todor
* Trondo
* Voema (Acer Chromebook Spin 514)

Codenames:

* Volet (Acer Chromebook 515)

Codenames:

* Volteer
* Volteer2

* Voxel (Acer Chromebook Spin 713 (CP713-3W))

#### Google Gru (Rockchip RK3399)

Codenames:

* Kevin (Samsung Chromebook Plus)

Codenames:

* Gru
* Bob (Asus Chromebook Flip C101PA)

Codenames:

* Scarlet (Acer Chromebook Tab 10)

Codenames:

* Nefario
* Rainier

#### Google Hatch (Intel CometLake (10th Gen))

Codenames:

* Akemi (IdeaPad Flex 5/5i Chromebook)

Codenames:

* Dratini (HP Pro c640 Chromebook)

Codenames:

* Hatch
* Helios (ASUS Chromebook Flip C436FA)

Codenames:

* Helios_Diskswap
* Jinlon (HP Elite c1030 Chromebook)

Codenames:

* Kindred (Acer Chromebook 712)

Codenames:

* Kohaku (Samsung Galaxy Chromebook)

Codenames:

* Mushu
* Nightfury (Samsung Galaxy Chromebook 2)

Codenames:

* Palkia

#### Google Trogdor (Qualcomm Snapdragon 7c (SC7180))

Codenames:

* Bubs
* Coachz (HP Chromebook x2 11-da0)

Codenames:

* Gelarshie
* Homestar (Lenovo IdeaPad Duet 5 Chromebook)

Codenames:

* Kingoftown (HP Fortis 11 G9 Q Chromebook)

Codenames:

* Lazor (Acer Chromebook Spin 513, CP513-1H/1HL, R841T/LT)

Codenames:

* Marzipan
* Mrbland
* Pazquel (see list below)

The Pazquel codename covers these devices:

- Orbic Chromebook 4G
- Libera-Merdeka Chromebook C100/C110/C120/C150
* Pompom (Dynabook Chromebook C1)

Codenames:

* Quackingstick (Acer Chromebook Tab 510 / Enterprise Tab 510)

Codenames:

* Wormdingler (Lenovo IdeaPad Duet 3 Chromebook)

Codenames:

* Trogdor

#### Google Glados (Intel Skylake (6th Gen))

Codenames:

* Asuka (Dell Chromebook 13 3380)

Codenames:

* Caroline (Samsung Chromebook Pro)

Codenames:

* Cave (Asus Chromebook Flip C302SA)

Codenames:

* Chell (HP Chromebook 13 G1)

Codenames:

* Glados Skylake Reference Board
* Lars (Acer Chromebook 14 for Work (CP5-471))

Codenames:

* Sentry (Lenovo Thinkpad 13 Chromebook)

#### Google Foster (Nvidia Tegra X1 (T210))

Codenames:

* Foster

#### Google Poppy (Intel KabyLake/KabyLake-R (7th/8th Gen))

Codenames:

* Atlas (Google Pixelbook Go)

Codenames:

* Poppy
* Nami

The Nami codename covers these devices:

- Acer Chromebook 13 / Spin 13 (Akali)
- Lenovo Yoga Chromebook C630 (Pantheon)
- HP Chromebook x360 14 (Sona)
- HP Chromebook 15 G1 (Syndra)
- Dell Inspiron Chromebook 14 (7460) (Vayne)

Codenames:

* Nautilus (Samsung Chromebook Plus V2, V2 LTE)

Codenames:

* Nocturne (Google Pixel Slate)

Codenames:

* Rammus

The Rammus codename covers these devices:

- Asus Chromebook C425 (Leona)
- Asus Chromebook Flip C433/C434 (Shyvana)

Codenames:

* Soraka (HP Chromebook x2)

#### Google Cherry (MediaTek Kompanio 1200 (MT8195))

Codenames:

* Cherry
* Dojo (HP Chromebook x360 (13b-ca0002sa))

Codenames:

* Tomato (Acer Chromebook Spin 513 (CP513-2H))

#### Google Nyan Big (Nvidia Tegra K1 (T124))

Codenames:

* Nyan Big (Acer Chromebook 13 (CB5-311))

#### Google Veyron Rialto (Rockchip RK3288)

Codenames:

* Veyron_Rialto

#### Google Sarien (Intel WhiskeyLake (8th Gen))

Codenames:

* Arcada (Latitude 5300 2-in-1 Chromebook Enterprise)

Codenames:

* Sarien (Dell Latitude 5400 Chromebook Enterprise)

#### Google Guybrush (AMD Ryzen Mobile 5000 (Cezanne))

Codenames:

* Dewatt (Acer Chromebook Spin 514)

Codenames:

* Guybrush
* Nipperkin (HP Elite c645 G2 Chromebook)

#### Google Geralt (MediaTek Kompanio 838 (MT8188))

Codenames:

* Geralt
* Ciri (Lenovo Chromebook Duet Gen9 (11M889))

#### Google Gale (Qualcomm IPQ4019)

Codenames:

* Gale (Google WiFi)

#### Google Reef (Intel ApolloLake)

Codenames:

* Reef/Electro (Acer Chromebook Spin 11 R751T)

Codenames:

* Pyro (Lenovo Thinkpad (Yoga) 11e Chromebook)

Codenames:

* Sand (Acer Chromebook 15 CB515-1HT/1H)

Codenames:

* Snappy (HP Chromebook x360 11 G1 EE)

Codenames:

* Coral

#### Google Atria (Intel NovaLake)

Codenames:

* Atria
* Penghu

#### Google Octopus (Intel GeminiLake)

Codenames:

* Ampton (ASUS Chromebook Flip C214)

Codenames:

* Bloog (HP Chromebook x360 12b, 14a, x360 14b)

Codenames:

* Bobba (Acer Chromebook 311 / Spin 511)

Codenames:

* Bluebird/Casta (Samsung Chromebook 4, 4+)

Codenames:

* Dood (NEC Chromebook Y2)

Codenames:

* Fleex (Dell Chromebook 3100)

Codenames:

* Foob (CTL Chromebook VX11/VT11T)

Codenames:

* Garg (CTL Chromebook NL71/CT/LTE)

Codenames:

* Lick (Ideapad 3 Chromebook)

Codenames:

* Meep (HP Chromebook x360 11 G2 EE)

Codenames:

* Octopus
* Phaser (Lenovo 100e/300e/500e Chromebook 2nd Gen (Intel))

Codenames:

* Yorp

#### Google Veyron (Rockchip RK3288)

Codenames:

* Veyron_Jaq (Haier Chromebook 11)

Codenames:

* Veyron_Jerry (Hisense Chromebook 11)

Codenames:

* Veyron_Mighty (Haier Chromebook 11(edu))

Codenames:

* Veyron_Minnie (ASUS Chromebook Flip C100)

Codenames:

* Veyron_Speedy (ASUS C201 Chromebook)

#### Google Rauru (MediaTek Kompanio Ultra 910 (MT8196))

Codenames:

* Hylia (Acer Chromebook Plus Spin 514 (CP514-5HN/CPE594-2N))

Codenames:

* Navi (Lenovo Chromebook Plus (14M9610))

Codenames:

* Rauru
* Sapphire

#### Google Slippy (Intel Haswell (4th Gen))

Codenames:

* Falco (HP Chromebook 14)

Codenames:

* Leon (Toshiba Chromebook)

Codenames:

* Peppy (Acer C720/C720P Chromebook)

Codenames:

* Wolf (Dell Chromebook 11)

#### Google Kukui (MediaTek Kompanio 500 (MT8183))

Codenames:

* Kukui
* Krane (Lenovo Chromebook Duet/Lenovo IdeaPad Duet Chromebook)

Codenames:

* Kodama (Lenovo 10e Chromebook Tablet)

Codenames:

* Kakadu (ASUS Chromebook Detachable CM3)

Codenames:

* Flapjack
* Katsu (ASUS Chromebook Detachable CZ1)

#### Google Jacuzzi (MediaTek Kompanio 500 (MT8183))

Codenames:

* Jacuzzi
* Juniper (Acer Chromebook Spin 311 (CP311-3H))

Codenames:

* Kappa (HP Chromebook 11a)

Codenames:

* Damu (ASUS Chromebook Flip CM3 (CM3200))

Codenames:

* Cerise
* Stern
* Willow (Acer Chromebook 311 (C722/C722T))

Codenames:

* Esche (HP Chromebook 11MK G9 EE)

Codenames:

* Burnet (HP Chromebook x360 11MK G3 EE)

Codenames:

* Fennel (Lenovo IdeaPad 3 Chromebook)

Codenames:

* Cozmo (Acer Chromebook 314 (CB314-2H/CB314-2HT))

Codenames:

* Makomo (Lenovo 100e Chromebook (2nd Gen))

Codenames:

* Munna
* Pico (Acer Chromebook Spin 311)

#### Google Nyan (NVIDIA Tegra K1 (CD570M))

Codenames:

* Nyan

#### Google Rex (Intel MeteorLake (Ultra 1st Gen))

Codenames:

* Deku
* Deku4ES
* Karis (Acer Chromebook Plus Spin 714)

Codenames:

* Karis4ES
* Ovis
* Ovis4ES
* Rex 0
* Rex EC ISH
* Rex4ES
* Rex4ES EC ISH
* Rex 64
* Screebo (ASUS ExpertBook CX54 Chromebook Plus (CX5403))

Codenames:

* Screebo4ES
* Kanix (Acer Chromebook Plus 714)

#### Google Auron (Intel Broadwell (5th Gen))

Codenames:

* Auron_Paine (Acer C740 Chromebook)

Codenames:

* Auron_Yuna (Acer Chromebook 15 (C910/CB5-531))

Codenames:

* Buddy (Acer Chromebase 24)

Codenames:

* Gandof (Toshiba Chromebook 2 (2015))

Codenames:

* Lulu (Dell Chromebook 13 7310)

Codenames:

* Samus (Google Chromebook Pixel (2015))

#### Google Fatcat (Intel PantherLake (Ultra 3rd Gen))

Codenames:

* Fatcat
* Fatcat4ES
* Fatcat_ish
* Fatcatite
* Fatcatite4ES
* Fatcatnuvo
* Fatcatnuvo4ES
* Felino
* Felino4ES
* Francka
* Kinmen4ES
* Kinmen
* Lapis
* Moonstone
* Ruby

#### Google Oak (MediaTek Kompanio 500 (MT8173))

Codenames:

* Oak
* Elm (Acer Chromebook R13)

Codenames:

* Hana (Lenovo N23 Yoga Chromebook)

#### Google Eve (Intel KabyLake (7th Gen))

Codenames:

* Eve (Google Pixelbook)

#### Google Peach Pit (Samsung Exynos 5420)

Codenames:

* Peach Pit (Samsung Chromebook 2 11\)

#### Google Puff (Intel CometLake (10th Gen))

Codenames:

* Ambassador (Meet Compute System [Intel 10th Gen])

Codenames:

* Dooly (HP Chromebase 21.5)

Codenames:

* Duffy Legacy (32MB)

Codenames:

* Duffy (ASUS Chromebox 4)

Codenames:

* Faffy (ASUS Fanless Chromebox)

Codenames:

* Genesis (Meet Compute System - Series One [Intel 10th Gen])

Codenames:

* Kaisa Legacy (32MB)

Codenames:

* Kaisa (Acer Chromebox CXI4)

Codenames:

* Moonbuggy (Series One Board 65)

Codenames:

* Noibat (HP Chromebox G3)

Codenames:

* Puff
* Scout (Series One Desk 27)

Codenames:

* Wyvern (Multiple devices - see list below):

The Wyvern codename covers these devices:

- CTL Chromebox CBx2/Enterprise CBx2
- Promethean Chromebox 2
- ViewSonic NMP760 Chromebox
- Wyvern MobLab

#### Google Skyrim (AMD Ryzen Mobile 7000 (Mendocino))

Codenames:

* Crystaldrift (SPC Chromebook V1)

Codenames:

* Frostflow (ASUS Chromebook CM34 Flip)

Codenames:

* Markarth (Acer Chromebook Plus 514)

Codenames:

* Skyrim
* Winterhold (Dell Latitude 3445 Chromebook)

### Laptops

Intel

**8th Gen.**

-   [Lenovo ThinkPad X270/X280](t480.html) (Complex, DDR4) - Also **[available to buy with Libreboot pre-installed](https://minifree.org/product/libreboot-x280.html)**
-   [Lenovo ThinkPad T480 and ThinkPad T480s](t480.html) (Complex, DDR4) - Also **[available to buy with Libreboot pre-installed](https://minifree.org/product/libreboot-t480.html)**
-   [Lenovo ThinkPad T580](t480.html) (Complex, DDR4) - Also **[available to buy with Libreboot pre-installed](https://minifree.org/product/libreboot-t580.html)**

**5th Gen.**

-   [HP EliteBook 720/740/750/820/840/850 G2, HP ZBook 14/15u G2](hp820g2.md) (Complex, DDR3L)

**4th Gen.** 

-   [Lenovo ThinkPad W541](w541_external.md) (Complex, DDR3L)
-   [Lenovo ThinkPad T440p](t440p_external.md) (Complex, DDR3L)

 **3rd -> 2nd Gen.**
 
(Socketed processors are fully compatible with motherboards from different generations)

-   [HP EliteBook 2170p](hp2170p.md) (Moderate, DDR3L)
-   [HP EliteBook 2570p/2560p](hp2560p.md) (Moderate, DDR3L)
-   [HP EliteBook 8470p/8460p](hp8460p.md) (Moderate, DDR3L)
-   [HP EliteBook Folio 9470m](hp9470m.md) (Complex, DDR3L)
-   Lenovo ThinkPad T430 (**but not the T430S**) (Complex, DDR3L)
-   Lenovo ThinkPad T530 (Complex, DDR3L)
-   [Lenovo Thinkpad X230 / X230 Tablet](x230_external.md) (Complex, DDR3L)
-   [HP EliteBook 8560w](hp8560w.md) (Moderate, DDR3L)
-   [Dell Latitude E5420, E5520, E5530, E6420, E6430, E6520, E6530, E6230, E6330, E6220, E7240](latitude.md) (Simple, DDR3L)
    (Intel GPU variants only)
-   [Lenovo Thinkpad T420/T520](t420_external.md) (Complex, DDR3L)
-   Lenovo ThinkPad T420s (Complex, DDR3L)
-   [Lenovo ThinkPad X220/X220](x230_external.md) (Complex, DDR3L)

**Core 2**

-   [Dell Latitude E4300, E6400, E6400 XFR and E6400 ATG, all with Nvidia or Intel GPU](latitude.md) (Simple, DDR2L + DDR3L for E4300)
-   [Lenovo ThinkPad R400](r400.md) (Complex, DDR3L)
-   Lenovo ThinkPad R500 (Complex, DDR3L)
-   [Lenovo ThinkPad T400 / T400S](t400.md) (Complex, DDR3L)
-   [Lenovo ThinkPad T500 / W500](t500.md) (Complex, DDR3L)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](x200.md) (Complex, DDR3L)
-   [Apple MacBook1,1 and MacBook2,1](macbook21.md) (Complex for 1.1/Simple for 2.1 - 2007, DDR2L)
-   [Lenovo ThinkPad T60](t60_unbrick.md) (with Intel GPU) (Complex, DDR2L)
-   [Lenovo ThinkPad X60](x60_unbrick.md) (with Intel GPU) (Complex, DDR2L)
-   Lenovo ThinkPad X60S (with Intel GPU) (Complex, DDR2L)
-   [Lenovo ThinkPad X60 Tablet](x60tablet_unbrick.md) Tablet (with Intel GPU) (Complex, DDR2L)

Arm

**Chromebooks with U-Boot payload**

-   [ASUS Chromebook Flip C101 (gru-bob)](chromebooks.md) (Complex)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](chromebooks.md) (Complex)


### Games consoles

-   [Sony Playstation](playstation.md) (Complex) (PS1/PSX)


### Servers (x86_64 with ECC support)

Intel

**9th -> 6th gen** (Xeon v5-6 and Core up to the 9th gen ones)

(Different gen. CPUs compatible with older and newer motherboards too, on older ones 8th-9th Core with B0 stepping; i3, Pentium Gold, Celeron. 6/8 core processors might work with pin-mod, untested.)

-   [Supermicro X11SSH-(LN4)F](supermicro_x11ssh_f_ln4f.md) (Moderate, DDR4)

AMD

**Opteron 6300**

-   [ASUS KGPE-D16 motherboard](kgpe-d16.md) (Moderate, DDR3)

**Opteron 4300/4200**

-   [ASUS KCMA-D8 motherboard](kcma-d8.md) (Moderate, DDR3)

**Opteron 2000/8000**

-   [ASUS KFSN4-DRE motherboard](kfsn4-dre.md) (Complex, DDR2)


### Desktops/WorkStations (x86/_64)

**12th Gen**

-   [Topton X2E N150 aka BKHD H30W-N150](x2e-n150.md) (Simple, DDR5)

**9th -> 6th gen** (Xeon v5-6 and Core up to the 9th gen ones)

(Different gen. CPUs compatible with older and newer motherboards too, on older ones 8th-9th Core with B0 stepping;
i3, Pentium Gold, Celeron. 6/8 core processors might work with pin-mod, untested.)

-   [Dell OptiPlex 3040/3050 Micro](dell3050.html) (Simple, DDR3L/DDR4) - **Also [available to buy with Libreboot pre-installed](https://minifree.org/product/libreboot-3050-micro/)**
-   [Dell OptiPlex 5050 SFF](dell5050.html) (Simple, DDR4)

**5th -> 4th Gen.** (Core + Xeon v3-4)

(5th gen cpus are compatible with some the 4gen boards; C222/4/6 and H/Z97 chipsets 
https://en.wikipedia.org/wiki/LGA_1150#Haswell_chipsets )

-   [Dell OptiPlex 7020/9020/XE2/T1700 MT and SFF](dell9020.md) (Simple, DDR3 + ECC for T1700, Broadwell processors does not works with this motherboard yet)

**3rd -> 2nd Gen.** (Core + Xeon v1-2)

(Processors are fully compatible with motherboards from different generations)

-   [Dell OptiPlex 7010/9010 SFF/MT +T1650](dell7010.md) (Simple, DDR3 + ECC for T1650)
-   [HP Compaq Elite 8300 USDT](hp8300usdt.md) (Simple, DDR3)
-   [HP Compaq Elite 8200/Compaq 6200 Pro SFF/MT](hp8200sff.md) (Simple, DDR3)

**Core 2**

-   [Acer G43T-AM3](acer_g43t-am3.md) (Simple, DDR3)
-   [Dell OptiPlex 780 variants e.g. MT, USFF](dell780.md) (Simple, DDR3)
-   [Gigabyte GA-G41M-ES2L motherboard](ga-g41m-es2l.md) (Simple, DDR2)
-   [Apple Apple iMac 5,2](macbook21.md) (Simple, DDR2) (Same as Macbook 2.1 to flash)

**Bonnel (Atom)**
-   [Intel D945GCLF](d945gclf.md) (Moderate, DDR2)
-   Intel D510MO and D410PT motherboards (Moderate, DDR2)


### Emulation

-   [Qemu x86 and arm64](../misc/emulation.md)

More information about the Libreboot
build system can be found in the [lbmk maintenance manual](../maintain/),
and information about porting new systems in
the [porting manual](../maintain/porting.md).


ROM image file names
--------------------

**Modern Libreboot standardises on EDK2 these days, not SeaBIOS/GRUB which are
provided inside EDK2 for backward compatibility but recommended only for
legacy use. Please use the `edk2_` images provided by modern Libreboot, where
possible as this is much more compatible with modern operating systems.**

Libreboot ROM images are named like
this: `payload_board_inittype_displaytype_keymap.rom`

The `payload` option can be EDK2, SeaBIOS, GRUB or U-Boot. U-Boot on ARM64
and EDK2 on x86; GRUB and SeaBIOS EFI builds provided for backward compatibility
with older releases, inside of the EDK2 build.

Inittype can be `libgfxinit`, `vgarom` or `normal`. The `libgfxinit` option
means coreboot provides native video initialisation, for onboard graphics.
The `vgarom` option means coreboot executes a VGA option ROM for video
initialisation. The `normal` option means coreboot provides no video
initialisation, via VGA ROM or native code.

Displaytype can be `txtmode` or `corebootfb` - if inittype is `normal`, this
is ignored because `txtmode` is assumed.

NOTE: modern releases also support `fspgop` which, semantics aside, works
essentially just like `libgfxinit_corebootfb`.

If `payload` is `seabios` instead of `seagrub`, no keymaps are inserted into
flash and only US QWERTY is assumed, otherwise the keymap refers to what is used
in GRUB on `seagrub` payload setups.

If you use a libgfxinit image on a desktop machine, you can still insert a
graphics card and it'll work just fine; its own VGA option ROM will be
executed instead, if the primary payload is SeaBIOS, whether that be pure
SeaBIOS or a SeaGRUB setup.


EC firmware updates
-------------------

Obviously, free EC firmware would be preferable, but it is not the case on
all machine. We would like to have free EC firmware on more machines, but for
now, we must rely on the vendor in a lot of cases. The EC is usually on a
separate flash, so you wouldn't think about it unless you knew it was there;
this is exactly why it's mentioned, so that you think about it,
[because proprietary software is bad](../../news/policy.md).

In many cases, the EC firmware must be updated on a separate IC to the main
boot flash, and this can usually only be done with the vendor's own tool,
running from the vendor boot firmware, and usually only on Windows, because
they provide EC and BIOS/UEFI updates in the same utility. Find out what you
need to do for your machine before installing Libreboot.

It is recommended that you update to the latest EC firmware version. The
[EC firmware](../../faq.md#ec-embedded-controller-firmware)

Updating the EC can sometimes provide benefit depending on the vendor. For
example, they might fix power issues that could then enhance battery life.


### ThinkPads

See: <http://www.thinkwiki.org/wiki/BIOS_update_without_optical_disk>

Otherwise, check the Lenovo website to find the update utility for your
motherboard.


### Flash lockdown / boot security

This is referred to informally as *Secure libreBoot*.

Full flash lockdown is possible, with cryptographic verification of your
Linux kernel and other files, using special features in the GRUB payload.

See: [GRUB hardening / Secure libreBoot](../linux/grub_hardening.md)

If you already did this, it's possible that you may no longer be able to
flash internally. If that is the case, you must [flash externally](spi.md).


### ThinkPad X200/T400/T500/W500/R400/R500

If you're running one of these with Lenovo BIOS, you must externally flash
Libreboot, because the original firmware restricts writes to the flash.

There machines all use SOIC8/SOIC16 flash ICs. Refer to pages specifically for
each machine:

* [ThinkPad X200](x200.md)
* [ThinkPad T400](t400.md)
* [ThinkPad R400](r400.md)
* [ThinkPad T500/W500](t500.md) (R500 is similar)

NOTE: T400S, X200S and X200 Tablet require different steps, because these have
WSON8 flash ICs on them, which will require some soldering. Please read
the [external flashing guide](spi.md) in the section pertaining to WSON.

You can find WSON8 probes online, that are similar to a SOIC8/SOIC16 clip. Your
mileage may very, but WSON8 has the same pinout as SOIC8 so you might have some
luck with that.

Hot-swap the flash IC with another one while it's running, and flash it
internally.


### ThinkPad X220/X220T/T420/T420s/T520

T420/T520: [T420 flash instructions](t420_external.md)

T420s/T520 are similar, in terms of assembly/disassembly.

Now, as for X220/X220 Tablet:

X220/X220 Tablet is essentially the same as the X230, but cannot be flashed
internally; you must use [an external flasher](spi.md).

Otherwise, look at [X230 disassembly](x230_external.md). Note that the X220
has a single 8MB flash, instead of 8MB and 4MB.


### ThinkPad X230/T430/T530/W530/X230T

Internal flashing from vendor BIOS to Libreboot is possible, but not
recommended. See: [IvyBridge ThinkPad internal flashing](ivy_internal.md).

External flashing recommended, but only documented for X230.
See: [ThinkPad X230 install guide](x230_external.md).

Otherwise, refer to [external SPI flashing](spi.md).


### ThinkPad T60/X60/X60Tablet/X60S

Only the Intel GPU is compatible. Do not flash the ATI GPU models.

External flashing guides:

* [ThinkPad X60](x60_unbrick.md)
* [ThinkPad X60 Tablet](x60tablet_unbrick.md)
* [ThinkPad T60](t60_unbrick.md)

These machines can also be flashed internally, by exploiting a bug
in the original Lenovo BIOS. If there's a BIOS password at boot, you should
just flash externally.

Internal flashing instructions:

First, please ensure that your CR2032/CMOS battery is working. This is what
powers the SRAM containing BIOS settings, and it powers the real-time clock.
It also holds the BUC.TS value - this is what we need.

BUC (Backup Control) register contains a bit called Top Swap (TS). The 64KB
bootblock at the top of flash is complemented by a backup Top Swap just above
it. The one at the end can't be flashed internally while Lenovo BIOS is running,
but the rest of it can be flashed (everything above the main bootblock).

By setting the TS bit, you can make the machine boot from the backup bootblock.

Download the Libreboot 20160907 utils archive, and in there you will find
these binaries:

* `flashrom`
* `flashrom_i945_sst`
* `flashrom_i945_mx`

You'll also find the bucts tool. Run it as root:

	./bucts 1

Now run both of these as root:
	
	./flashrom_i945_sst -p internal -w coreboot.rom
	./flashrom_i945_mx -p internal -w coreboot.rom

You'll see a lot of errors. This is normal. You should see something like:

```
Reading old flash chip contents... done.
Erasing and writing flash chip... spi_block_erase_20 failed during command execution at address 0x0
Reading current flash chip contents... done. Looking for another erase function.
spi_block_erase_52 failed during command execution at address 0x0
Reading current flash chip contents... done. Looking for another erase function.
Transaction error!
spi_block_erase_d8 failed during command execution at address 0x1f0000
Reading current flash chip contents... done. Looking for another erase function.
spi_chip_erase_60 failed during command execution
Reading current flash chip contents... done. Looking for another erase function.
spi_chip_erase_c7 failed during command execution
Looking for another erase function.
No usable erase functions left.
FAILED!
Uh oh. Erase/write failed. Checking if anything has changed.
Reading current flash chip contents... done.
Apparently at least some data has changed.
Your flash chip is in an unknown state.
```

If you see this, rejoice! It means that the flash was successful. Please do not
panic. Shut down now, and wait a few seconds, then turn back on again.

The main bootblock still isn't flashed, but you can shut down, wait a few
seconds and boot up again. When you do, you'll have Libreboot. Please make
sure to flash a second time, like so:
	
	flashprog -p internal -w coreboot.rom

Libreboot recommends `flashprog` now, which is a fork of flashrom, but we used
flashrom in the 2016 release. The macronix/ssh flashrom binaries there are
specifically patched; check the Libreboot 20160907 source code for the actual
patches. The patches modify some flash chip definitions in flashrom, to exploit
the bug in Lenovo BIOS enabling internal flashing.

You must ensure that the second flash is performed, upon reboot, because
otherwise if the CR2032 battery dies, bucts will be reset and it will no
longer boot.

When you've done the second flash, which includes overwriting the main
bootblock, set bucts back to zero:

	./bucts 0

The second flash can be done by simply following the general internal flashing
guide further down on this page.


### ARM-based Chromebooks

See: [Chromebook flashing instructions](chromebooks.md)

NOTE: The generic flashing instructions (later on this page) apply only to
the x86 machines, because the Chromebooks still use flashrom with
the `-p host` argument instead of `-p internal` when flashing, and you typically
need to flash externally, due to Google's security model.


### QEMU (arm64 and x86)

Libreboot can be used on QEMU (virtual machine), which is useful for debugging
payloads and generally trying out Libreboot, without requiring real hardware.

See: [Libreboot QEMU guide](../misc/emulation.md)



Removed boards
--------------

These boards were in Libreboot, but have been removed with the intention of
re-adding them at a later date. They were removed due to issues. List:

-   [Acer Chromebook 13 (CB5-311, C810) (nyan-big)](chromebooks.md)
-   [ASUS Chromebit CS10 (veyron-mickey)](chromebooks.md)
-   [ASUS Chromebook C201PA (veyron-speedy)](c201.md)
-   [ASUS Chromebook Flip C100PA (veyron-minnie)](chromebooks.md)
-   [Hisense Chromebook C11 and more (veyron-jerry)](chromebooks.md)
-   [HP Chromebook 11 G1 (daisy-spring)](chromebooks.md)
-   [HP Chromebook 14 G3 (nyan-blaze)](chromebooks.md)
-   [Samsung Chromebook 2 11" (peach-pit)](chromebooks.md)
-   [Samsung Chromebook 2 13" (peach-pi)](chromebooks.md)
-   [Samsung Chromebook XE303 (daisy-snow)](chromebooks.md)
-   Lenovo ThinkPad X301 (still in lbmk, but with `release="n"`)


### NOTES about removed boards

**WARNING: veyron speedy boards (e.g. C201) have non-functional video init as
of 19 February 2023, and no fix is yet available on that date. See:
<https://notabug.org/libreboot/lbmk/issues/136> - the last tested revision
from 2021.01 is known to work, for u-boot on this board. See:\
<https://wiki.postmarketos.org/wiki/ASUS_Chromebook_C201_(google-veyron-speedy)>
(alpernebbi on IRC is looking into this, to bisect uboot and update the latest
revisions) - for now, ROM images deleted from the Libreboot 20221214
and 20230319 releases.**

**WARNING: daisy- and peach- boards require a BL1 bootloader firmware, but the
one from coreboot 3rdparty is a fake/placeholder file. We need logic in the
Libreboot build system for properly fetching/extracting these, plus docs to
cover it. For now, assume that these are broken - ROM images are excluded,
for now, and have been deleted from the Libreboot 20221214 and 20230319
releases. - see: <https://review.coreboot.org/plugins/gitiles/blobs/+/4c0dcf96ae73ba31bf9aa689768a5ecd47bac19e>
and <https://review.coreboot.org/plugins/gitiles/blobs/+/b36cc7e08f7337f76997b25ee7344ab8824e268d>**

D945GCLF: Doesn't boot at all, according to last report. D510MO is still in
lbmk but still was reported problematic; other boards should be fine (see list
above).

WARNING: Support for ARM chromebooks is at a proof-of-concept stage. Refer
to [docs/uboot/](../uboot/) for more info about the U-Boot payload.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/>
