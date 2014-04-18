---
layout: tutorial
title: Photoscan
meta: Opis procesu rekonstrukcji modeli 3D ze zdjęć przy pomocy programu Photoscan
sources:
    - url: http://www.agisoft.ru/products/photoscan
      title: Oficjalna strona programu Photoscan
    - url: http://www.agisoft.ru/pdf/photoscan_1_0_en.pdf
      title: Manual do wersji Standard 1.0
    - url: http://ir-ltd.net/delivering-aligned-and-scaled-photoscan-outputs/
      title: Delivering Aligned and Scaled Photoscan Outputs
page-type: tutorial
folder: photoscan
---

## 1. Konfiguracja programu

Photoscan wspiera akcelerację GPU (openCL). Wykorzystanie GPU w procesie rekonstrukcji ma sens tylko na w miarę wydajnych, desktowpowych, kartach graficznych. Zintegrowane karty graficzne nie nadają się do tego celu, mogą wręcz spowolnić działanie programu.

Aby skonfigurować akcelerację GPU, wchodzimy do menu `Tools > Preferences`, do zakładki `OpenCL`

Zaznaczamy karty graficzne. Dla każdej zaznaczonej karty graficznej musimy zmniejszyć liczbę wykorzystywanych rdzeni procesora o 1 (Fig. 1.1).

![alt text](photoscan_01_01.jpg "Ustawienia OpenCL")
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

## 2. Kalibracja zdjęć

Celem tego kroku jest wyliczenie pozycji aparatu, dla każdego zdjęcia. Sama chmura punktów będąca rezultatem tego kroku nie będzie wykorzystana przy rekonstrukcji modelu, służy jedynie do kalibracji sceny.

Aby rozpocząć kalibrację sceny, dodajemy zdjęcia do przestrzeni roboczej `Workflow > Add Photos` lub 'Workflow > Add Folder'. Następnie Wchodzimy do menu `Workflow > Align Photos` (Fig. 2.1).

![alt text](photoscan_02_01.jpg "Opcje kalibracji")
<sup>Fig. 2.1 Opcje kalibracji</sup>

Tu ustawiamy parametry tego kroku - `Accuracy` i `Preselection`, jak na screenshocie. Następnie rozwijamy menu `Advanced` i ustawiamy wartość parametru `Point limit`. Im więcej punktów, tym dokładniej jest wyliczona pozycja aparatu dla każdego zdjęcia. W tym przypadku ustawiliśmy 180 000 punktów, im więcej punktów tym dokłądniejsza kalibracja, lecz dłuższe obliczenia. Nie znaczy to że program znajdzie tyle punktów, ile ustawilismy - wszystko zależy od ilości, jakości zdjęć oraz od rozmiaru sceny. Liczba odnalezionych punktów referencyjnych może być więc mniejsza niz zadana wartość (Fig. 2.2).

![alt text](photoscan_02_02.png "Liczba odnalezionych punktów referencyjnych")
<sup>Fig. 2.2 Liczba odnalezionych punktów referencyjnych</sup>

Górną granicą rozsądku jest ok 500 000 punktów dla większych scen / obiektów z dużą ilością zdjęć. Ustalenie wartości `Point limit` na poziomie ok 180 000 wydaje być się dobrym punktem startowym. Scenę zawsze można przeliczyć, jeżeli okaże się że program odnalazł liczbę punktów równą wartości tego parametru (czyli możliwa jest dokałdniejsza kalibracja przy zwiększeniu wartości tego parametru).

## 3. Wstępna orientacja sceny

Po zakonczeniu procesu kalibracji zdjęć otrzymujemy rzadką chmurę punktów - nasz obiekt. Powinniśmy teraz tak obrócić obiekt, aby zgadzała się z grubsza z systemem współrzędnych sceny (Fig. 3.1). Dokładną kalibrację orientacji będziemy przeprowadzać po wygenerowaniu modelu.

![alt text](photoscan_03_01.png "Rzadka chmura punktów")
<sup>Fig. 3.1 Orientacja obiektu jest rozbieżna z systemem współrzędnych sceny</sup>

Dla większej przejrzystości wyłączamy widoczność kamer (Fig 3.2).

![alt text](photoscan_03_02.png "Widoczność kamer")
<sup>Fig. 3.3 Widoczność kamer</sup>

Zmieniamy widok na `Ortographic` (_menu_ `View > Perspective/Ortographic`), następnie przełączając się pomiędzy rzutami sceny (_menu_ `View > Predefined Views`) obracamy obiekt przy pomocy narzędzia `Rotate Object` (Fig. 3.2), tak aby jego oriantacja była zgodna z systemem współrzędnych.

![alt text](photoscan_03_03.png "Rotate object")
<sup>Fig. 3.3 Narzędzie `Rotate Object`</sup>

### Skróty klawiszowe do przełączania pomiędzy widokami

Widok |  Skrót klawiaturowy |
--- | ---
Perspective / Ortographic | 5
Front | 1
Back | Ctrl + 1
Right | 3
Left | Ctrl + 3
Top | 7
Bottom | Ctrl + 7
Rotate Up | 8
Rotate Down | 2
Rotate Left | 4
Rotate Right | 6


## 4. Ustalenie zakresu rekonstrukcji

## 5. Zagęszczona chmura punktów

## 6. Usuwanie artefaktów

## 7. Budowa geometrii modelu

## 8. Budowa tekstury modelu

## 9. Kalibracja skali

## 10. Kalibracja koordynat

![alt text](photoscan_10_01.png "Standardowy system koordynat")
<sup>Fig. 10.1 Standardowy system koordynat</sup>

![alt text](photoscan_10_02.png "Koordynaty FBX")
<sup>Fig. 11.2 Koordynaty FBX (osie Z i X zamienione)</sup>

## 11. Eksport


