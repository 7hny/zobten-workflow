---
layout: tutorial
title: Photoscan
meta: Przekrojowy tutorial - instrukcja obsługi Photoscana, czyli jak używać Photoscana tak aby osiągnąć najlepsze rezultaty dla naszych potrzeb. Tutorial W trakcie budowy.
sources:
    - url: http://www.agisoft.ru/products/photoscan
      title: Oficjalna strona Photoscana
    - url: http://www.agisoft.ru/pdf/photoscan_1_0_en.pdf
      title: Manual do wersji Standard 1.0
page-type: tutorial
folder: photoscan
---

## 1. Konfiguracja programu

Photoscan wspiera akcelerację GPU (openCL). Wykorzystanie GPU w procesie rekonstrukcji ma sens tylko na w miarę wydajnych, desktowpowych, kartach graficznych. Zintegrowane karty graficzne nie nadają się do tego celu, mogą wręcz spowolnić działąnie programu.

Aby skonfigurować akcelerację GPU, wchodzimy do menu `Tools > Preferences`, do zakładki `OpenCL`

![alt text](photoscan_005.jpg "Ustawienia OpenCL")
<sup>Fig. 1.1 Ustawienia OpenCL</sup>

Zaznaczamy karty graficzne. Dla każdej zaznaczonej karty graficznej musimy zmniejszyć liczbę wykorzystywanych rdzeni procesora o 1.

### Lista obsługiwanych kart graficznych

NVIDIA |  AMD |
--- | ---
Seria Maxwell | Seria R9
GeForce GTX Titan | Radeon HD 7970
GeForce GTX 780 | Radeon HD 6970
GeForce GTX 680 | Radeon HD 6950
GeForce GTX 580 | Radeon HD 6870
GeForce GTX 570 | Radeon HD 5870
GeForce GTX 560 | Radeon HD 5850
GeForce GTX 480 | Radeon HD 5830
GeForce GTX 470 |
GeForce GTX 465 |
GeForce GTX 285 |
GeForce GTX 280 |


