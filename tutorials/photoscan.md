---
layout: tutorial
title: Photoscan
meta: Opis procesu rekonstrukcji modeli 3D ze zdjęć przy pomocy programu Photoscan
sources:
    - url: http://www.agisoft.ru/products/photoscan
      title: Oficjalna strona programu Photoscan
    - url: http://www.agisoft.ru/pdf/photoscan_1_0_en.pdf
      title: Manual do wersji Standard 1.0
page-type: tutorial
folder: photoscan
---

## 1. Konfiguracja programu

Photoscan wspiera akcelerację GPU (openCL). Wykorzystanie GPU w procesie rekonstrukcji ma sens tylko na w miarę wydajnych, desktowpowych, kartach graficznych. Zintegrowane karty graficzne nie nadają się do tego celu, mogą wręcz spowolnić działąnie programu.

Aby skonfigurować akcelerację GPU, wchodzimy do menu `Tools > Preferences`, do zakładki `OpenCL`

Zaznaczamy karty graficzne. Dla każdej zaznaczonej karty graficznej musimy zmniejszyć liczbę wykorzystywanych rdzeni procesora o 1 (Fig. 1.1).

![alt text](photoscan_001.jpg "Ustawienia OpenCL")
<sup>Fig. 1.1 Ustawienia OpenCL</sup>



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

## 2. Kalibracja

Celem tego kroku jest wyliczenie pozycji aparatu, dla każdego zdjęcia. Sama chmura punktów będąca rezultatem tego kroku nie będzie wykorzystana przy rekonstrukcji modelu, służy jedynie do kalibracji sceny.

Aby rozpocząć kalibrację sceny, dodajemy zdjęcia do przestrzeni roboczej `Workflow > Add Photos` lub 'Workflow > Add Folder'. Następnie Wchodzimy do menu `Workflow > Align Photos` (Fig. 2.1).

![alt text](photoscan_002.jpg "Opcje kalibracji")
<sup>Fig. 2.1 Opcje kalibracji</sup>

Tu ustawiamy parametry tego kroku - `Accuracy` i `Preselection`, jak na screenshocie. Następnie rozwijamy menu `Advanced` i ustawiamy wartość parametru `Point limit`. Im więcej punktów, tym dokładniej jest wyliczona pozycja aparatu dla każdego zdjęcia. W tym przypadku ustawiliśmy 180 000 punktów, im więcej punktów tym dokłądniejsza kalibracja, lecz dłuższe obliczenia. Nie znaczy to że program znajdzie tyle punktów, ile ustawilismy - wszystko zależy od ilości, jakości zdjęć oraz od rozmiaru sceny. Liczba odnalezionych punktów referencyjnych może być więc mniejsza niz zadana wartość (Fig. 2.2).

![alt text](photoscan_003.png "Liczba odnalezionych punktów referencyjnych")
<sup>Fig. 2.2 Liczba odnalezionych punktów referencyjnych</sup>

Górną granicą rozsądku jest ok 500 000 punktów dla większych scen / obiektów z dużą ilością zdjęć. Ustalenie wartości `Point limit` na poziomie ok 180 000 wydaje być się dobrym punktem startowym. Scenę zawsze można przeliczyć, jeżeli okaże się że program odnalazł liczbę punktów równą wartości tego parametru (czyli że możliwa jest dokąłdniejsza kalibracja przy zwiększeniu wartości tego parametru).

## 3. Kalibracja skali

## 4. Kalibracja koordynat

![alt text](photoscan_003.png "Standardowy system koordynat")
<sup>Fig. 4.1 Standardowy system koordynat</sup>

![alt text](photoscan_003.png "Liczba odnalezionych punktów referencyjnych")
<sup>Fig. 4.2 Koordynaty FBX (osie Z i X zamienione)</sup>




