---
layout: tutorial
title: Photoscan
meta: Opis procesu rekonstrukcji modeli 3D ze zdjęć przy pomocy programu Photoscan
sources:
    - url: http://www.agisoft.ru/products/photoscan
      title: Oficjalna strona programu Photoscan
    - url: http://www.agisoft.ru/pdf/photoscan_1_0_en.pdf
      title: Manual do wersji Standard 1.0
    - url: http://www.agisoft.ru/pdf/photoscan_pro_1_0_en.pdf
      title: Manual do wersji Pro 1.0
    - url: http://ir-ltd.net/delivering-aligned-and-scaled-photoscan-outputs/
      title: Delivering Aligned and Scaled Photoscan Outputs
    - url: http://ir-ltd.net/its-your-birthday/
      title: Agisoft Pro Scripts
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

Aby rozpocząć kalibrację sceny, dodajemy zdjęcia do przestrzeni roboczej   `Workflow > Add Photos` lub `Workflow > Add Folder`. Następnie Wchodzimy do menu `Workflow > Align Photos` (Fig. 2.1).

![alt text](photoscan_02_01.jpg "Opcje kalibracji")
<sup>Fig. 2.1 Opcje kalibracji</sup>

Tu ustawiamy parametry tego kroku - `Accuracy` i `Preselection`, jak na screenshocie. Następnie rozwijamy menu `Advanced` i ustawiamy wartość parametru `Point limit`. Im więcej punktów, tym dokładniej jest wyliczona pozycja aparatu dla każdego zdjęcia. W tym przypadku ustawiliśmy 180 000 punktów, im więcej punktów tym dokłądniejsza kalibracja, lecz dłuższe obliczenia. Nie znaczy to że program znajdzie tyle punktów, ile ustawilismy - wszystko zależy od ilości, jakości zdjęć oraz od rozmiaru sceny. Liczba odnalezionych punktów referencyjnych może być więc mniejsza niz zadana wartość (Fig. 2.2).

![alt text](photoscan_02_02.png "Liczba odnalezionych punktów referencyjnych")
<sup>Fig. 2.2 Liczba odnalezionych punktów referencyjnych</sup>

Górną granicą rozsądku jest ok 500 000 punktów dla większych scen / obiektów z dużą ilością zdjęć. Ustalenie wartości `Point limit` na poziomie ok 180 000 wydaje być się dobrym punktem startowym. Scenę zawsze można przeliczyć, jeżeli okaże się że program odnalazł liczbę punktów równą wartości tego parametru (czyli możliwa jest dokałdniejsza kalibracja przy zwiększeniu wartości tego parametru).

### Określanie jakości materiału zdjęciowego - krok opcjonalny

Jakośc obrazu (relatywna ostrość danego zdjęcia) może wpłynąć na jakość rekonstruowanego obiektu. W celu oszacowania jakości zdjęć, po dokonaniu kalibracji otwieramy panel `Photos` (menu `View > Panes > Photos`). Następnie zaznaczamy wszystkie zdjęcia, otwieramy menu kontekstowe PPM i Wybieramy opcje `Estimate Image Quality` (Fig 2.3).

![alt text](photoscan_02_03.png "Szacowanie jakości zdjęć")
<sup>Fig. 2.3 Szacowanie jakości zdjęć</sup>

Twórcy programu zalecają wykluczenie zdjęć o jakości poniżej `0.5` W tym celu zaznaczamy (CTRL + click) zdjęcia o jakości poniżej `0.5` i klikamy ikonkę `Disable Cameras` (Fig 2.4)

![alt text](photoscan_02_04.png "Oszacowana jakość zdjęć")
<sup>Fig. 2.4 Oszacowana jakość zdjęć</sup>

## 3. Wstępna orientacja sceny

Po zakonczeniu procesu kalibracji zdjęć otrzymujemy rzadką chmurę punktów - nasz obiekt. Powinniśmy teraz tak obrócić obiekt, aby zgadzała się z grubsza z systemem współrzędnych sceny (Fig. 3.1). Dokładną kalibrację orientacji będziemy przeprowadzać po wygenerowaniu modelu.

![alt text](photoscan_03_01.png "Rzadka chmura punktów")
<sup>Fig. 3.1 Orientacja obiektu rozbieżna z systemem współrzędnych sceny</sup>

Dla większej przejrzystości wyłączamy widoczność pozycji zdjęć (kamer) (Fig 3.2).

![alt text](photoscan_03_02.png "Widoczność kamer")
<sup>Fig. 3.2 Widoczność kamer</sup>

Zmieniamy widok na `Ortographic` ( *menu* `View > Perspective/Ortographic`), następnie przełączając się pomiędzy rzutami sceny ( *menu* `View > Predefined Views`) obracamy obiekt przy pomocy narzędzia `Rotate Object` (Fig. 3.2), tak aby jego oriantacja była zgodna z systemem współrzędnych (Fig. 3.4).

![alt text](photoscan_03_03.png "Rotate object")
<sup>Fig. 3.3 Narzędzie `Rotate Object`</sup>

![alt text](photoscan_03_04.png "Rzadka chmura punktów")
<sup>Fig. 3.4 Orientacja obiektu zgodna z systemem współrzędnych sceny</sup>

### Skróty klawiszowe do zmiany widoków

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

Interesujący nasz zakres rekonstrukcji ustalamy przy pomocy narzędzi `Resize Region` oraz `Rotate Region` (Fig. 4.1).

![alt text](photoscan_04_01.png "Region tools")
<sup>Fig. 4.1 Narzędzia do zmiany zakresy rekonstrukcji</sup>

Przy ustalaniu zakresu rekonstrukcji, posługujemy się ortogonalnymi (Ortographic) widokami, podobnie jak w poprzednim kroku. Zawężenie akrsu rekonstrukcji zminiejsza liczbę artefaktów oraz przyśpiesza obliczenia (Fig. 4.3)

![alt text](photoscan_04_02.png "Domyślny zakres rekonstrukcji")
<sup>Fig. 4.2 Domyślny zakres rekonstrukcji</sup>

![alt text](photoscan_04_03.png "Zakres rekonstrukcji")
<sup>Fig. 4.3 Zakres rekonstrukcji</sup>

## 5. Generacja zagęszczonej chmury punktów

Jest to najważniejszy, i najbardziej intensywny obliczeniowo krok, podczas którego uzyskujemy góęstą chmurę punktów, na podstawie której będziemy potem rekonstruować siatkę geometryczną.

Wybieramy z głównego menu komendę `Workflow > Build Dense Cloud`.

![alt text](photoscan_05_01.png "Build Dense Cloud")
<sup>Fig. 5.1 Build Dense Cloud</sup>

Opcja `Quality` (Fig. 5.1) odpowiada za szczegółowość rekonstrukcji, im wyższa jakość tym dłuższe obliczenia (Fig. 5.2). Każdy krok zminiejsza rozmiar zdjęcia 4x. `Ultra` to romiar oryginalny zdjęcia.

Opcja `Depth filtering` (Fig. 5.1) odpowiada za szczegółowość powierzchni rekonstruowanego obiektu obiektu, Mild - duża, Aggressive - niska (Fig. 5.3). Opcja `Aggressive` przelicza się szybciej.

![alt text](photoscan_05_02.jpg "Jakość High i Medium")
<sup>Fig. 5.2 Kolejno, jakość Medium i High</sup>

![alt text](photoscan_05_03.jpg "Build Dense Cloud")
<sup>Fig. 5.3 Kolejno, Aggressive i Mild</sup>

Po zakończeniu obliczeń przełączamy się na widok zagęszczonej chmury punktów (Fig.5.4).

![alt text](photoscan_05_04.png "Zagęszczona chmura punktów")
<sup>Fig. 5.4 Zagęszczona chmura punktów</sup>

## 6. Usuwanie artefaktów

## 7. Rekonstrukcja geometrii modelu

## 8. Rekonstrukcja tekstury modelu

## 9. Kalibracja skali

## 10. Kalibracja koordynat

![alt text](photoscan_10_01.png "Standardowy system koordynat")
<sup>Fig. 10.1 Standardowy system koordynat</sup>

![alt text](photoscan_10_02.png "Koordynaty FBX")
<sup>Fig. 11.2 Koordynaty FBX (osie Z i X zamienione)</sup>

## 11. Eksport

## Apendiks

### Jak fotografować

### Maskowanie zdjęć


