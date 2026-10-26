---
title: Installer Libreboot, firmware de démarrage BIOS/UEFI open source
x-toc-enable: true
...

Firmware de démarrage BIOS/UEFI open source
------------------------------

**N'oubliez TOUJOURS de faire une sauvegarde de la flash actuelle avant de l'écraser,
quel que soit le firmware que vous avez actuellement et celui avec lequel vous
la reflashez ; cela inclut les mises à jour entre versions de Libreboot. Utilisez
l'option `-r` de flashprog au lieu de `-w`, pour lire la flash.**

AVERTISSEMENT DE SÉCURITÉ !
---------------------------

**[VEUILLEZ LIRE CES INSTRUCTIONS AVANT D'INSTALLER](ivy_has_common.md), SINON
VOUS RISQUEZ DE BRIQUER VOTRE MACHINE !!

Si vous [compilez à partir des sources](../build/), vous pouvez ignorer les conseils ci-dessus.
Veillez tout de même à sauvegarder le firmware actuel avant de flasher !

Appareils achetables directement chez [Minifree](https://minifree.org) ;
-------------------------------------------------------------------

### Portables et ordinateurs de bureau de 8e génération (Kabylake Refresh)

-   **[Lenovo ThinkPad X280](https://minifree.org/product/libreboot-x280.html)**

-   **[Lenovo ThinkPad T480 et ThinkPad T480s](https://minifree.org/product/libreboot-t480.html)**

-   **[Lenovo ThinkPad T580](https://minifree.org/product/libreboot-t580.html)**

-   **[Dell OptiPlex 3050 Micro](https://minifree.org/product/libreboot-3050-micro/)**

Pris en charge par Libreboot ;
-----------------------

**Si vous souhaitez une installation professionnelle de Libreboot,
un [service d'installation](https://minifree.org/product/installation-service.html)
est disponible ; il propose aussi d'autres firmwares coreboot en plus de Libreboot. Par
exemple, l'installation sur Chromebook via la distribution UEFI coreboot MrChromebox est
également disponible.**

### Chromebooks (Intel/AMD)


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

# Reworked Volteer2 prototype, Haven chip replaced with Dauntless demo board* Volteer2_Ti50
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

### Portables (Intel, x86)

#### 8e génération (Kabylake Refresh).

-   [Lenovo ThinkPad X280](t480.html) (Complexe, DDR4) - Aussi **[disponible à l'achat avec Libreboot préinstallé](https://minifree.org/product/libreboot-x280.html)**
-   [Lenovo ThinkPad T480 et ThinkPad T480s](t480.html) (Complexe, DDR4) - Aussi **[disponible à l'achat avec Libreboot préinstallé](https://minifree.org/product/libreboot-t480.html)**
-   [Lenovo ThinkPad T580](t480.html) (Complexe, DDR4) - Aussi **[disponible à l'achat avec Libreboot préinstallé](https://minifree.org/product/libreboot-t580.html)**

#### 5e génération.

-   [HP EliteBook 820 G2](hp820g2.md) (Complexe, DDR3L)

#### 4e génération.

-   [Lenovo ThinkPad W541](w541_external.md) (Complexe, DDR3L)
-   [Lenovo ThinkPad T440p](t440p_external.md) (Complexe, DDR3L)

#### 3e génération.

-   [HP EliteBook 2170p](hp2170p.md) (Modéré, DDR3L)
-   [HP EliteBook 2570p](hp2570p.md) (Modéré, DDR3L)
-   [HP EliteBook 8470p](hp8470p.md) (Modéré, DDR3L)
-   [HP EliteBook Folio 9470m](hp9470m.md) (Complexe, DDR3L)
-   Lenovo ThinkPad T430 (**mais pas le T430S**) (Complexe, DDR3L)
-   Lenovo ThinkPad T530 (Complexe, DDR3L)
-   [Lenovo Thinkpad X230 / X230 Tablet](x230_external.md) (Complexe, DDR3L)
-   [HP EliteBook 2560p](hp2560p.md) (Modéré, DDR3L)
-   [HP EliteBook 8460p](hp8460p.md) (Modéré, DDR3L)
-   [HP EliteBook 8560w](hp8560w.md) (Modéré, DDR3L)
-   [Dell Latitude E5420, E5520, E5530, E6420, E6430, E6520, E6530, E6230, E6330, E6220, E7240](latitude.md) (Simple, DDR3L)
    (variantes à GPU Intel uniquement)
-   [Lenovo Thinkpad T420/T520](t420_external.md) (Complexe, DDR3L)
-   Lenovo ThinkPad T420s (Complexe, DDR3L)
-   [Lenovo ThinkPad X220/X220](x230_external.md) (Complexe, DDR3L)

#### Core 2

-   [Dell Latitude E4300, E6400, E6400 XFR et E6400 ATG, tous avec GPU Nvidia ou Intel](latitude.md) (Simple, DDR2L + DDR3L pour l'E4300)
-   [Lenovo ThinkPad R400](r400.md) (Complexe, DDR3L)
-   Lenovo ThinkPad R500 (Complexe, DDR3L)
-   [Lenovo ThinkPad T400 / T400S](t400.md) (Complexe, DDR3L)
-   [Lenovo ThinkPad T500 / W500](t500.md) (Complexe, DDR3L)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](x200.md) (Complexe, DDR3L)
-   [Apple MacBook1,1 et MacBook2,1](macbook21.md) (Complexe pour le 1.1 / Simple pour le 2.1 - 2007, DDR2L)
-   [Lenovo ThinkPad T60](t60_unbrick.md) (avec GPU Intel) (Complexe, DDR2L)
-   [Lenovo ThinkPad X60](x60_unbrick.md) (avec GPU Intel) (Complexe, DDR2L)
-   Lenovo ThinkPad X60S (avec GPU Intel) (Complexe, DDR2L)
-   [Lenovo ThinkPad X60 Tablet](x60tablet_unbrick.md) Tablet (avec GPU Intel) (Complexe, DDR2L)

### Consoles de jeu

-   [Sony Playstation](playstation.md) (Complexe) (PS1/PSX)

### Serveurs (AMD, x86 avec prise en charge ECC)

#### Opteron 6300

-   [Carte mère ASUS KGPE-D16](kgpe-d16.md) (Modéré, DDR3)

#### Opteron 4300/4200

-   [Carte mère ASUS KCMA-D8](kcma-d8.md) (Modéré, DDR3)

#### Opteron 2000/8000

-   [Carte mère ASUS KFSN4-DRE](kfsn4-dre.md) (Complexe, DDR2)

### Ordinateurs de bureau/stations de travail (Intel, x86)

#### 12e génération

-   [Topton X2E N150 alias BKHD H30W-N150](x2e-n150.md) (Simple, DDR5)

#### 8e génération (Kabylake Refresh).

(Les processeurs d'autres générations sont aussi compatibles avec les cartes mères plus anciennes et plus récentes ; les 4 cœurs sont confirmés, les processeurs 6/8 cœurs pourraient fonctionner, non testé.)

-   [Dell OptiPlex 3050 Micro](dell3050.html) (Simple, DDR4) - **Aussi [disponible à l'achat avec Libreboot préinstallé](https://minifree.org/product/libreboot-3050-micro/)**
-   [Dell OptiPlex 5050 SFF](dell5050.html) (Simple, DDR4)

#### 5e -> 4e génération (Core + Xeon v3-4)

(Les processeurs de 5e génération sont compatibles avec certaines cartes de 4e génération ; C222/4/6 avec initialisation native de la RAM uniquement, et chipsets H/Z97
                  https://en.wikipedia.org/wiki/LGA_1150#Haswell_chipsets )

-   [Dell OptiPlex 7020/9020/XE2/T1700 MT et SFF](dell9020.md) (Simple, DDR3 + ECC pour le T1700)

#### 3e -> 2e génération (Core + Xeon v1-2)

(Les processeurs sont entièrement compatibles avec les cartes mères d'autres générations)

-   [Dell OptiPlex 7010/9010 SFF/MT +T1650](dell7010.md) (Simple, DDR3 + ECC pour le T1650)
-   [HP Elite 8300 USDT](hp8300usdt.md) (Simple, DDR3)
-   [HP Elite 8200 SFF/MT](hp8200sff.md) (Simple, DDR3) (le HP 6200 Pro Business fonctionne probablement aussi)

#### Core 2

-   [Acer G43T-AM3](acer_g43t-am3.md) (Simple, DDR3)
-   [Dell OptiPlex 780 variantes, par ex. MT, USFF](dell780.md) (Simple, DDR3)
-   [Carte mère Gigabyte GA-G41M-ES2L](ga-g41m-es2l.md) (Simple, DDR2)
-   [Apple Apple iMac 5,2](macbook21.md) (Simple, DDR2) (Même procédure de flash que le Macbook 2.1)

**Bonnel (Atom)**
-   [Intel D945GCLF](d945gclf.md) (Modéré, DDR2)
-   Cartes mères Intel D510MO et D410PT (Modéré, DDR2)

### Portables (ARM, avec charge utile U-Boot)

-   [ASUS Chromebook Flip C101 (gru-bob)](chromebooks.md) (Complexe)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](chromebooks.md) (Complexe)

### Émulation

-   [Qemu x86 et arm64](../misc/emulation.md)

Plus d'informations sur le système de compilation de Libreboot
se trouvent dans le [manuel de maintenance de lbmk](../maintain/),
et des informations sur le portage de nouveaux systèmes dans
le [manuel de portage](../maintain/porting.md).

Installer Libreboot par flashage externe
---------------------------------------

Consultez l'article suivant :\
[Réécrire en externe une flash NOR 25xx via le protocole SPI](spi.md)

Il est vivement conseillé d'*avoir* une configuration de flashage externe, et de vérifier
qu'elle fonctionne, avant de tenter un flashage interne. Cela s'ajoute à la
sauvegarde du contenu actuel de la flash avant de flasher, que vous la lisiez
en externe ou en interne - si seul le flashage externe est disponible, alors
en général seule la lecture externe l'est aussi.

Un problème ?
----------

Vous pouvez poser vos questions sur l'[IRC de Libreboot](../../contact.md) et d'autres canaux.

Si vous souhaitez une installation professionnelle, Minifree Ltd vend
[Libreboot préinstallé](https://minifree.org/) sur du matériel sélectionné, et propose aussi
un [service de préinstallation de Libreboot](https://minifree.org/product/installation-service/)
si vous voulez envoyer votre machine pour qu'on y installe Libreboot à votre place.

Leah Rowe, la fondatrice et développeuse principale de Libreboot, est aussi propriétaire
et gérante de Minifree Ltd ; les ventes financent le projet Libreboot.

Désactiver la sécurité avant de flasher
--------------------------------

**Avant un flashage interne, vous devez d'abord désactiver les protections de `/dev/mem`.
Veillez à les réactiver une fois terminé.**

**Voir : [Désactiver la protection de /dev/mem](devmem.md)**

Noms des fichiers d'image ROM
--------------------

Les images ROM de Libreboot sont nommées ainsi :
`payload_board_inittype_displaytype_keymap.rom`

L'option `payload` peut être SeaBIOS, SeaGRUB ou U-Boot. Si GRUB est disponible
sur une carte donnée, dans la flash, SeaBIOS et SeaGRUB sont tous deux fournis ; les images SeaBIOS
donnent quand même accès à GRUB via le menu de SeaBIOS, et SeaGRUB signifie que
SeaBIOS charge automatiquement GRUB depuis la flash en premier (mais vous pouvez toujours choisir
autre chose en appuyant sur ÉCHAP dans SeaBIOS lorsque c'est proposé).

Inittype peut être `libgfxinit`, `vgarom` ou `normal`. L'option `libgfxinit`
signifie que coreboot fournit l'initialisation vidéo native, pour les graphismes intégrés.
L'option `vgarom` signifie que coreboot exécute une option ROM VGA pour l'initialisation
vidéo. L'option `normal` signifie que coreboot ne fournit aucune initialisation
vidéo, ni par ROM VGA ni par code natif.

Displaytype peut être `txtmode` ou `corebootfb` - si inittype est `normal`, cette
option est ignorée car `txtmode` est supposé.

Si `payload` vaut `seabios` au lieu de `seagrub`, aucune disposition de clavier n'est insérée dans la
flash et seul le QWERTY US est supposé ; sinon, la disposition de clavier correspond à celle utilisée
dans GRUB avec les configurations de charge utile `seagrub`.

Si vous utilisez une image libgfxinit sur un ordinateur de bureau, vous pouvez quand même insérer une
carte graphique et cela fonctionnera très bien ; sa propre option ROM VGA sera
exécutée à la place, si la charge utile principale est SeaBIOS, qu'il s'agisse de
SeaBIOS pur ou d'une configuration SeaGRUB.

Mises à jour du firmware EC
-------------------

Évidemment, un firmware EC libre serait préférable, mais ce n'est pas le cas sur
toutes les machines. Nous aimerions avoir un firmware EC libre sur davantage de machines, mais
pour l'instant, nous devons nous fier au fabricant dans beaucoup de cas. L'EC se trouve généralement sur une
flash séparée, donc on n'y penserait pas si l'on ne savait pas qu'il existe ;
c'est précisément pour cela qu'on le mentionne, pour que vous y pensiez,
[parce que les logiciels propriétaires sont mauvais](../../news/policy.md).

Dans de nombreux cas, le firmware EC doit être mis à jour sur un circuit intégré distinct de la
flash de démarrage principale, ce qui ne peut généralement se faire qu'avec l'outil du fabricant,
exécuté depuis le firmware de démarrage du fabricant, et généralement uniquement sous Windows, car
les mises à jour EC et BIOS/UEFI sont fournies dans le même utilitaire. Renseignez-vous sur ce que vous devez
faire pour votre machine avant d'installer Libreboot.

Il est recommandé de mettre à jour vers la dernière version du firmware EC. Le
[firmware EC](../../faq.md#ec-embedded-controller-firmware)

Mettre à jour l'EC peut parfois apporter un bénéfice selon le fabricant. Par
exemple, il peut corriger des problèmes d'alimentation, ce qui peut ensuite améliorer l'autonomie de la batterie.

### ThinkPad

Voir : <http://www.thinkwiki.org/wiki/BIOS_update_without_optical_disk>

Sinon, consultez le site web de Lenovo pour trouver l'utilitaire de mise à jour de votre
carte mère.

### HP EliteBook

Le firmware EC est requis dans la flash de démarrage principale, mais le système de compilation de Libreboot
le télécharge automatiquement depuis HP pour chaque machine, et l'insère, donc
vous n'avez pas à vous en soucier. Assurez-vous simplement que les [fichiers propriétaires sont
insérés](ivy_has_common.md) si vous utilisez des images de version.

### Autres

Le même principe s'applique aux autres fabricants de portables.

Les machines qui ne sont pas des portables n'ont généralement pas de contrôleur embarqué.

Instructions d'installation de Libreboot
-----------------------------------

En général, si Libreboot fonctionne déjà, vous pouvez passer directement
à la dernière section de cette page, qui fournit des instructions générales de flashage
interne. Le flashage interne consiste à flasher la machine cible depuis la machine cible elle-même,
depuis un système d'exploitation qui y tourne.

Certaines cartes nécessitent des étapes particulières, même si Libreboot fonctionne déjà,
par exemple si vous avez [verrouillé la flash](../linux/grub_hardening.md), ou
autre exemple, [SureStart sur le HP EliteBook 820 G2](hp820g2.md)

Par conséquent, avant de suivre les guides génériques, vérifiez d'abord si
votre carte a des instructions particulières ; sinon, utilisez le guide générique à la
fin de cet article.

### Adresse MAC Intel GbE (systèmes basés sur IFD)

Sur toutes les plateformes Intel sauf X4X (par ex. Gigabyte GA-G41M-ES2L) et i945
(par ex. ThinkPad X60, ThinkPad T60, MacBook2,1), un Intel Flash Descriptor est
utilisé. Si la carte dispose d'un Ethernet gigabit Intel, l'adresse MAC est incluse dans
la flash, et peut (doit) être modifiée avant l'installation.

Vous pouvez utiliser [nvmutil](nvmutil.md) pour changer l'adresse MAC. Vous effectuerez
cette modification sur l'image ROM, avant de la flasher.

### Verrouillage de la flash / sécurité du démarrage

On l'appelle de façon informelle *Secure libreBoot*.

Un verrouillage complet de la flash est possible, avec vérification cryptographique de votre
noyau Linux et d'autres fichiers, grâce à des fonctionnalités spéciales de la charge utile GRUB.

Voir : [Durcissement de GRUB / Secure libreBoot](../linux/grub_hardening.md)

Si vous l'avez déjà fait, il est possible que vous ne puissiez plus
flasher en interne. Dans ce cas, vous devez [flasher en externe](spi.md).

### Mettre à jour une installation existante

Sauf indication contraire, dans les sections propres à chaque carte mère ci-dessous,
une installation Libreboot existante peut être mise à jour par flashage interne,
sans étape particulière ; suivez simplement le guide général de flashage interne,
dans la dernière section plus bas dans cette page.

Si vous avez une installation Libreboot existante mais que vous avez *verrouillé la flash*,
la mettre à jour nécessitera un flashage externe.

Si vous avez actuellement le firmware d'origine, vous devrez probablement flasher
en externe ; sur *certaines* machines, le flashage interne est possible, avec en général
des étapes particulières différentes de celles d'une mise à jour d'une installation existante.

Les sections suivantes concernent des cartes mères précises, là où c'est indiqué,
suivies d'instructions générales de flashage interne le cas échéant.

### ThinkPad X200/T400/T500/W500/R400/R500

Si vous utilisez l'une de ces machines avec le BIOS Lenovo, vous devez flasher Libreboot
en externe, car le firmware d'origine restreint les écritures dans la flash.

Ces machines utilisent toutes des circuits de flash SOIC8/SOIC16. Consultez les pages propres à
chaque machine :

* [ThinkPad X200](x200.md)
* [ThinkPad T400](t400.md)
* [ThinkPad R400](r400.md)
* [ThinkPad T500/W500](t500.md) (le R500 est similaire)

NOTE : les T400S, X200S et X200 Tablet nécessitent des étapes différentes, car ils ont
des circuits de flash WSON8, ce qui nécessitera de la soudure. Veuillez lire
le [guide de flashage externe](spi.md) dans la section consacrée au WSON.

Vous pouvez trouver en ligne des sondes WSON8, similaires à une pince SOIC8/SOIC16. Votre
expérience peut varier, mais le WSON8 a le même brochage que le SOIC8, donc vous aurez peut-être
un peu de chance avec ça.

Échangez à chaud le circuit de flash avec un autre pendant qu'il fonctionne, et flashez-le
en interne.

### ThinkPad X220/X220T/T420/T420s/T520

T420/T520 : [instructions de flashage du T420](t420_external.md)

Les T420s/T520 sont similaires, en termes de montage/démontage.

Quant aux X220/X220 Tablet :

Le X220/X220 Tablet est essentiellement identique au X230, mais ne peut pas être flashé
en interne ; vous devez utiliser [un flasheur externe](spi.md).

Sinon, consultez le [démontage du X230](x230_external.md). Notez que le X220
a une seule flash de 8 Mo, au lieu de 8 Mo et 4 Mo.

### ThinkPad X230/T430/T530/W530/X230T

Le flashage interne depuis le BIOS d'origine vers Libreboot est possible, mais déconseillé.
Voir : [Flashage interne des ThinkPad IvyBridge](ivy_internal.md).

Le flashage externe est recommandé, mais documenté uniquement pour le X230.
Voir : [guide d'installation du ThinkPad X230](x230_external.md).

Sinon, consultez le [flashage SPI externe](spi.md).

### ThinkPad T60/X60/X60Tablet/X60S

Seul le GPU Intel est compatible. Ne flashez pas les modèles à GPU ATI.

Guides de flashage externe :

* [ThinkPad X60](x60_unbrick.md)
* [ThinkPad X60 Tablet](x60tablet_unbrick.md)
* [ThinkPad T60](t60_unbrick.md)

Ces machines peuvent aussi être flashées en interne, en exploitant un bogue
du BIOS Lenovo d'origine. S'il y a un mot de passe BIOS au démarrage, vous devriez
simplement flasher en externe.

Instructions de flashage interne :

D'abord, assurez-vous que votre pile CR2032/CMOS fonctionne. C'est elle qui
alimente la SRAM contenant les réglages du BIOS, ainsi que l'horloge temps réel.
Elle conserve aussi la valeur BUC.TS - c'est ce dont nous avons besoin.

Le registre BUC (Backup Control) contient un bit appelé Top Swap (TS). Le bootblock de 64 Ko
situé en haut de la flash est complété par un Top Swap de secours juste au-dessus.
Celui de la fin ne peut pas être flashé en interne pendant que le BIOS Lenovo tourne,
mais le reste peut l'être (tout ce qui se trouve au-dessus du bootblock principal).

En activant le bit TS, vous pouvez faire démarrer la machine depuis le bootblock de secours.

Téléchargez l'archive d'utilitaires de Libreboot 20160907, et vous y trouverez
ces binaires :

* `flashrom`
* `flashrom_i945_sst`
* `flashrom_i945_mx`

Vous y trouverez aussi l'outil bucts. Exécutez-le en root :

	./bucts 1

Exécutez maintenant ces deux commandes en root :
	
	./flashrom_i945_sst -p internal -w coreboot.rom
	./flashrom_i945_mx -p internal -w coreboot.rom

Vous verrez beaucoup d'erreurs. C'est normal. Vous devriez voir quelque chose comme :

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

Si vous voyez cela, réjouissez-vous ! Cela signifie que le flashage a réussi. Ne
paniquez pas. Éteignez maintenant, attendez quelques secondes, puis rallumez.

Le bootblock principal n'est toujours pas flashé, mais vous pouvez éteindre, attendre quelques
secondes et redémarrer. Quand vous le ferez, vous aurez Libreboot. Veillez
à flasher une seconde fois, comme ceci :
	
	flashprog -p internal -w coreboot.rom

Libreboot recommande désormais `flashprog`, un fork de flashrom, mais nous avons utilisé
flashrom dans la version de 2016. Les binaires flashrom macronix/sst qui s'y trouvent sont
spécialement modifiés ; consultez le code source de Libreboot 20160907 pour les patchs
exacts. Les patchs modifient certaines définitions de puces flash dans flashrom, afin d'exploiter
le bogue du BIOS Lenovo qui permet le flashage interne.

Vous devez vous assurer que le second flashage est effectué, au redémarrage, car
sinon, si la pile CR2032 meurt, bucts sera réinitialisé et la machine ne
démarrera plus.

Quand vous avez fait le second flashage, qui inclut l'écrasement du bootblock
principal, remettez bucts à zéro :

	./bucts 0

Le second flashage peut se faire simplement en suivant le guide général de flashage interne
plus bas dans cette page.

### Chromebooks ARM

Voir : [Instructions de flashage des Chromebooks](chromebooks.md)

NOTE : les instructions générales de flashage (plus loin dans cette page) ne s'appliquent qu'aux
machines x86, car les Chromebooks utilisent toujours flashrom avec
l'argument `-p host` au lieu de `-p internal` lors du flashage, et vous devez généralement
flasher en externe, à cause du modèle de sécurité de Google.

### QEMU (arm64 et x86)

Libreboot peut être utilisé sur QEMU (machine virtuelle), ce qui est utile pour déboguer
des charges utiles et pour essayer Libreboot en général, sans matériel réel.

Voir : [Guide Libreboot QEMU](../misc/emulation.md)

Installer via le CPU hôte (flashage interne)
----------------------------------------

NOTE : cela s'applique principalement aux machines x86.

Veuillez consulter les autres sections listées ci-dessus, pour voir s'il y a quelque chose
concernant votre carte mère. Le flashage interne signifie que vous démarrez Linux ou
BSD sur la machine cible, et que vous y exécutez `flashprog`, flashant ainsi la machine
directement.

**Si vous ne pouvez pas flasher en interne, vous devez [flasher en externe](spi.md).**

Le flashage interne est souvent indisponible avec le firmware d'origine, mais il est
généralement possible lorsque Libreboot fonctionne (sauf circonstances particulières).

### Exécuter flashprog sur le CPU hôte

**Lorsque vous utilisez des images de version, n'oubliez pas de toujours
[insérer les fichiers propriétaires](ivy_has_common.md). Lorsque vous compilez à partir des sources,
les fichiers propriétaires sont ajoutés automatiquement à la compilation (ils ne sont pas
présents dans les images de version).**

#### Taille de la puce flash

Utilisez ceci pour la connaître :

	flashprog -p internal

La sortie contiendra des informations sur votre flash de démarrage.

#### Mode d'emploi : lire/écrire/effacer la flash de démarrage

Comment lire le contenu actuel de la puce :

	sudo flashprog -p internal:laptop=force_I_want_a_brick,boardmismatch=force -r dump.bin

Vous devriez quand même faire plusieurs dumps, même si vous flashez en interne, afin de
vous assurer d'obtenir les mêmes sommes de contrôle. Vérifiez chaque dump avec `sha1sum`

Comment effacer et réécrire le contenu de la puce :

	sudo flashprog -p internal:laptop=force_I_want_a_brick,boardmismatch=force -w libreboot.rom

NOTE : `force_I_want_a_brick` n'est pas effrayant. N'ayez pas peur ! Cela désactive simplement
les contrôles de sécurité de flashprog. Flashprog et coreboot évoluent beaucoup au fil des années,
et il est parfois nécessaire d'utiliser cette option. Si cela vous fait peur, suivez simplement
les instructions ci-dessus, mais retirez cette option. Utilisez donc simplement `-p internal`.
Si cela ne fonctionne pas, essayez ensuite `-p internal:boardmismatch=force`. Si cela ne
fonctionne pas, essayez `-p internal:boardmismatch=force,laptop=force_I_want_a_brick`. Tant
que vous *vous assurez* d'utiliser la bonne ROM pour votre machine, exécuter flashprog
sera sans danger. Ces options supplémentaires désactivent simplement les contrôles de sécurité de flashprog.
Il n'y a rien à craindre.

En cas de succès, il affichera soit `VERIFIED`, soit que le contenu de la puce
est identique à l'image demandée.

NOTE : il existe des exceptions où ce qui précède n'est pas possible. Lisez à leur sujet dans
les sections ci-dessous :

Cartes retirées
--------------

Ces cartes étaient dans Libreboot, mais ont été retirées avec l'intention de
les rajouter ultérieurement. Elles ont été retirées en raison de problèmes. Liste :

-   [Acer Chromebook 13 (CB5-311, C810) (nyan-big)](chromebooks.md)
-   [ASUS Chromebit CS10 (veyron-mickey)](chromebooks.md)
-   [ASUS Chromebook C201PA (veyron-speedy)](c201.md)
-   [ASUS Chromebook Flip C100PA (veyron-minnie)](chromebooks.md)
-   [Hisense Chromebook C11 et autres (veyron-jerry)](chromebooks.md)
-   [HP Chromebook 11 G1 (daisy-spring)](chromebooks.md)
-   [HP Chromebook 14 G3 (nyan-blaze)](chromebooks.md)
-   [Samsung Chromebook 2 11" (peach-pit)](chromebooks.md)
-   [Samsung Chromebook 2 13" (peach-pi)](chromebooks.md)
-   [Samsung Chromebook XE303 (daisy-snow)](chromebooks.md)
-   Lenovo ThinkPad X301 (toujours dans lbmk, mais avec `release="n"`)

### NOTES sur les cartes retirées

**AVERTISSEMENT : les cartes veyron speedy (par ex. C201) ont une initialisation vidéo non fonctionnelle
au 19 février 2023, et aucun correctif n'est encore disponible à cette date. Voir :
<https://notabug.org/libreboot/lbmk/issues/136> - la dernière révision testée
de 2021.01 est connue pour fonctionner, pour u-boot sur cette carte. Voir :\
<https://wiki.postmarketos.org/wiki/ASUS_Chromebook_C201_(google-veyron-speedy)>
(alpernebbi sur IRC s'y intéresse, pour bissecter uboot et mettre à jour les dernières
révisions) - pour l'instant, les images ROM ont été supprimées des versions Libreboot 20221214
et 20230319.**

**AVERTISSEMENT : les cartes daisy- et peach- nécessitent un firmware de chargeur d'amorçage BL1, mais
celui de coreboot 3rdparty est un faux fichier/fichier de substitution. Il nous faut une logique dans le
système de compilation de Libreboot pour les récupérer/extraire correctement, ainsi que de la documentation pour
l'expliquer. Pour l'instant, considérez-les comme non fonctionnelles - les images ROM sont exclues,
pour l'instant, et ont été supprimées des versions Libreboot 20221214 et 20230319.
- voir : <https://review.coreboot.org/plugins/gitiles/blobs/+/4c0dcf96ae73ba31bf9aa689768a5ecd47bac19e>
et <https://review.coreboot.org/plugins/gitiles/blobs/+/b36cc7e08f7337f76997b25ee7344ab8824e268d>**

D945GCLF : ne démarre pas du tout, d'après le dernier rapport. Le D510MO est toujours dans
lbmk mais a aussi été signalé comme problématique ; les autres cartes devraient fonctionner (voir la liste
ci-dessus).

AVERTISSEMENT : la prise en charge des Chromebooks ARM est au stade de preuve de concept. Consultez
[docs/uboot/](../uboot/) pour plus d'informations sur la charge utile U-Boot.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/>
