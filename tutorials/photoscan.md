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

![Opcje kalibracji](photoscan_02_01.jpg "Opcje kalibracji")
<sup>Fig. 2.1 Opcje kalibracji</sup>

Tu ustawiamy parametry tego kroku - `Accuracy` i `Preselection`, jak na screenshocie. Następnie rozwijamy menu `Advanced` i ustawiamy wartość parametru `Point limit`. Im więcej punktów, tym dokładniej jest wyliczona pozycja aparatu dla każdego zdjęcia. W tym przypadku ustawiliśmy 180 000 punktów, im więcej punktów tym dokłądniejsza kalibracja, lecz dłuższe obliczenia. Nie znaczy to że program znajdzie tyle punktów, ile ustawilismy - wszystko zależy od ilości, jakości zdjęć oraz od rozmiaru sceny. Liczba odnalezionych punktów referencyjnych może być więc mniejsza niz zadana wartość (Fig. 2.2).

![Liczba odnalezionych punktów referencyjnych](photoscan_02_02.png "Liczba odnalezionych punktów referencyjnych")
<sup>Fig. 2.2 Liczba odnalezionych punktów referencyjnych</sup>

Górną granicą rozsądku jest ok 500 000 punktów dla większych scen / obiektów z dużą ilością zdjęć. Ustalenie wartości `Point limit` na poziomie ok 180 000 wydaje być się dobrym punktem startowym. Scenę zawsze można przeliczyć, jeżeli okaże się że program odnalazł liczbę punktów równą wartości tego parametru (czyli możliwa jest dokałdniejsza kalibracja przy zwiększeniu wartości tego parametru).

### Wykluczanie zdjęć o niskiej jakości ( _krok opcjonalny_ )

Jakośc obrazu (relatywna ostrość danego zdjęcia) może wpłynąć na jakość rekonstruowanego obiektu. W celu oszacowania jakości zdjęć, po dokonaniu kalibracji otwieramy panel `Photos` (menu `View > Panes > Photos`). Następnie zaznaczamy wszystkie zdjęcia, otwieramy menu kontekstowe PPM i Wybieramy opcje `Estimate Image Quality` (Fig 2.3).

![Szacowanie jakości zdjęć](photoscan_02_03.png "Szacowanie jakości zdjęć")
<sup>Fig. 2.3 Szacowanie jakości zdjęć</sup>

Twórcy programu zalecają wykluczenie zdjęć o jakości poniżej `0.5` W tym celu zaznaczamy (CTRL + click) zdjęcia o jakości poniżej `0.5` i klikamy ikonkę `Disable Cameras` (Fig 2.4)

![Oszacowana jakość zdjęć](photoscan_02_04.png "Oszacowana jakość zdjęć")
<sup>Fig. 2.4 Oszacowana jakość zdjęć</sup>

## 3. Wstępna orientacja sceny

Po zakonczeniu procesu kalibracji zdjęć otrzymujemy rzadką chmurę punktów - nasz obiekt. Powinniśmy teraz tak obrócić obiekt, aby zgadzała się z grubsza z systemem współrzędnych sceny (Fig. 3.1). Dokładną kalibrację orientacji będziemy przeprowadzać po wygenerowaniu modelu.

![Rzadka chmura punktów](photoscan_03_01.png "Rzadka chmura punktów")
<sup>Fig. 3.1 Orientacja obiektu rozbieżna z systemem współrzędnych sceny</sup>

Dla większej przejrzystości wyłączamy widoczność pozycji zdjęć (kamer) (Fig 3.2).

![Widoczność kamer](photoscan_03_02.png "Widoczność kamer")
<sup>Fig. 3.2 Widoczność kamer</sup>

Zmieniamy widok na `Ortographic` ( *menu* `View > Perspective/Ortographic`), następnie przełączając się pomiędzy rzutami sceny ( *menu* `View > Predefined Views`) obracamy obiekt przy pomocy narzędzia `Rotate Object` (Fig. 3.2), tak aby jego oriantacja była zgodna z systemem współrzędnych (Fig. 3.4).

![Rotate object](photoscan_03_03.png "Rotate object")
<sup>Fig. 3.3 Narzędzie `Rotate Object`</sup>

![Rzadka chmura punktów](photoscan_03_04.png "Rzadka chmura punktów")
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

![Region tools](photoscan_04_01.png "Region tools")
<sup>Fig. 4.1 Narzędzia do zmiany zakresy rekonstrukcji</sup>

Przy ustalaniu zakresu rekonstrukcji, posługujemy się ortogonalnymi (Ortographic) widokami, podobnie jak w poprzednim kroku. Zawężenie akrsu rekonstrukcji zminiejsza liczbę artefaktów oraz przyśpiesza obliczenia (Fig. 4.3)

![Domyślny zakres rekonstrukcji](photoscan_04_02.png "Domyślny zakres rekonstrukcji")
<sup>Fig. 4.2 Domyślny zakres rekonstrukcji</sup>

![Zakres rekonstrukcji](photoscan_04_03.png "Zakres rekonstrukcji")
<sup>Fig. 4.3 Zakres rekonstrukcji</sup>

## 5. Generacja zagęszczonej chmury punktów

Jest to najważniejszy, i najbardziej intensywny obliczeniowo krok, podczas którego uzyskujemy góęstą chmurę punktów, na podstawie której będziemy potem rekonstruować siatkę geometryczną.

> **Opcjonalne czynności**  
> Możemy stworzyć maski dla każdego zdjęcia określające zarys obiektu. Przyśpieszy to obliczenia oraz wyeliminuje zbędne punkty, które mogły by przyczynić się do powstawania artefaktów. Tworzenie masek opisane jaest w **apendiksie**.

Wybieramy z głównego menu komendę `Workflow > Build Dense Cloud`.

![Build Dense Cloud](photoscan_05_01.png "Build Dense Cloud")
<sup>Fig. 5.1 Build Dense Cloud</sup>

Opcja `Quality` (Fig. 5.1) odpowiada za szczegółowość rekonstrukcji, im wyższa jakość tym dłuższe obliczenia (Fig. 5.2). Każdy krok zminiejsza rozmiar zdjęcia 4x. `Ultra` to romiar oryginalny zdjęcia.

Opcja `Depth filtering` (Fig. 5.1) odpowiada za szczegółowość powierzchni rekonstruowanego obiektu obiektu, Mild - duża, Aggressive - niska (Fig. 5.3). Opcja `Aggressive` przelicza się szybciej.

<a href="photoscan_05_02.jpg" target="_blank">![Jakość High i Medium](photoscan_05_02.jpg "Jakość High i Medium")</a>
<sup>Fig. 5.2 Kolejno, jakość Medium i High</sup>

<a href="photoscan_05_03.jpg" target="_blank">![Build Dense Cloud](photoscan_05_03.jpg "Build Dense Cloud")</a>
<sup>Fig. 5.3 Kolejno, Aggressive i Mild</sup>

Po zakończeniu obliczeń przełączamy się na jeden z widoków zagęszczonej chmury punktów (Fig.5.4).

![Zagęszczona chmura punktów](photoscan_05_04.png "Zagęszczona chmura punktów")
<sup>Fig. 5.4 Zagęszczona chmura punktów</sup>

## 6. Usuwanie artefaktów

> **Uwaga**  
> Jeżeli tworzyliśmy maski w poprzednim kroku, możemy pominąć ten krok.

W tym kroku usuwamy problematyczne punkty, które mogą przyczynić się do powstania artefaktów rekonstrukcji geometrii oraz tekstury obiektu. Używając narzędzi selekcji (Fig. 6.1), zaznaczamy problematyczne / niepożądane strefy (zaznaczenia dodajemy przy wciśniętym klawiszu `CTRL`) i usuwamy je klawiszem `DEL`.

![Selekcja punktów](photoscan_06_01.png "Selekcja punktów")
<sup>Fig. 6.1 Selekcja punktów</sup>

## 7. Rekonstrukcja geometrii modelu

> **Opcjonalne czynności**  
> W tym kroku możemy również stworzyć maski, jeżeli nie utworzyliśmy ich poprzednio. Maski przyczynią się do zmniejszenia ilośći artefaktów rekonstruowanej geometrii. Tworzenie masek opisane jaest w **apendiksie**.

Aby wygenerować geometrię wchodzimy do menu `Workflow > Build Mesh`. Tu wybieramy zagęszczoną chmyrę punktów jako źródło danych, z maksymalną liczbą poligonów (Fig. 7.1).

![Menu Build Mesh](photoscan_07_01.png "Menu Build Mesh")
<sup>Fig. 7.1 Menu `Build Mesh`</sup>

> Istnieje możliwość wygenerowania geometrii na podstawie chmury punktó referencyjnych wygenerowanej w kroku **2** ( `Sourde data > Sparse cloud` ). Może to być przydatne przy testowaniu masek, jakości generowanej tekstury, kallibracji koordynat. Jednak model wygenerowany w ten sposób nie nadaje siędo celów produkcyjnych.

Po wygenerowaniu siatki, przełączamy się do jednego z widoków siatki, aby zobaczyć rezultat rekonstrukcji (Fig 7.2).

![Zrekonstruowana geometria](photoscan_07_02.png "Zrekonstruowana geometria")
<sup>Fig. 7.2 Zrekonstruowana geometria</sup>

## 8. Rekonstrukcja tekstury modelu

W tym kroku dokonujemy projekcji tekstury z materiału zdjęciowego na zrekostruowany model. W tym celu wchodzimy do menu `Workflow > Build Texture`. Rozdzielczość tekstury ustawiamy na `8192` (Fig. 8.1). Jeżeli pracujemy z wyjątkowo dużym modelem, możemy wygenerować więcej niż jeden arkusz tekstur, w celu zachowania należytej rozdzielczości detali (Fig. 8.1).

![Menu Build Texture](photoscan_08_01.png "Menu Build Texture")
<sup>Fig. 8.1 Menu `Build Texture`</sup>

Po wygenerowaniu tekstury, przełączamy się do widoku tekstury, aby zobaczyć rezultat rekonstrukcji (Fig 8.2).

![Zrekonstruowana tekstura](photoscan_08_02.png "Zrekonstruowana tekstura")
<sup>Fig. 8.2 Zrekonstruowana tekstura</sup>

## 9. Kalibracja skali (wersja PRO)

Po zrekonstruowaniu modelu należy dokonać kalibracji skali. Umożliwi to zachowanie rzeczywistej skali (wielkości modelu).

> Zachowanie skali jest konieczne - po optymalizacji w zewnętrznym programie (3DS Max, zBrush), zoptymalizowany model będzie musiał być ponownie zaimportowany do Photoscana w celu ponownej projekcji tekstury. Jeżeli skala nie będzie sie zgadzać (bo np. zmodyfikowaliśmy ją w zewnętrznym programie), poprawna projekcja tekstury nie będzie możliwa.

Aby skalibrować scenę musimy dodać przynajmniej 2 punkty referencyjne, które są widoczne an przynajmniej 2 zdjęciach.

> **UWAGA!**   
> W trakcie robienia zdjęć powinniśmy umieścić **znacznik skali** na fotografowanym obiekcie. Znacznikiem slaki moze być zwykłą linijka, ew. możemy zmierzyć odległość pomiędzy dwoma charakterystycznymi punktami na scenie. W obu przypadkach chodzi o pomiar z **milimetrową dokłądnością**.

Otwieramy panel `View > Panes > Ground Control`. Mamy 2 możliwości mozemy dodać punkty referencyjne na zdjęciach (sekcja cameras, Fig. 9.1) lub bezpośrednio na obiekcie. Najpierw uaktywniamy widok punktó przyciskiem `Show Markers` (Fig. 9.1). Następnie dodajemy punkt, klikając prawym przyciskiem myszy (PPM) w wybranym miejscu z którego pobieraliśmy wymiary (Fig. 9.1). Wybieramy opcjie `Add marker` z menu kontekstowego. Powtarzamy czynnośćdla drugiego punktu referencyjnego. Rozmieszczenie punktów można dostosować bezpośrednio w widoku zdjęcia (Fig. 9.1).

![Create Markers"](photoscan_09_01.png "Create Markers")
<sup>Fig. 9.1 Tworzenie punktu referencyjnego"</sup>

Drugi punkt referencyjny tworzymy w analogiczny sposób. Następnie tworzymy podziałkę skali (`Scale bar`) zaznaczając oba markery (SHIFT + Click) w panelu `Markers` (Fig. 9.2), i klikamy PPM wybierając opcję `Create scale bar` (Fig. 9.3).
 
![Tworzenie podziałki"](photoscan_09_02.png "Tworzenie podziałki")
<sup>Fig. 9.2 Tworzenie podziałki"</sup>

![Tworzenie podziałki"](photoscan_09_03.png "Tworzenie podziałki")
<sup>Fig. 9.3 Tworzenie podziałki c.d."</sup>

Podziałka skali pojawiła się w panelu `Scale bars` Wpisujemy odległość referencyjną **w centymetrach** (Fig. 9.4). Następnie klikamy ikonkę `Update` (Fig. 9.4). Porces kalibracji skali został zakończony!.

![Pomiar referencyjny"](photoscan_09_04.png "Pomiar referencyjny")
<sup>Fig. 9.4 Pomiar referencyjny i kalibracja"</sup>

Po wyeksportowaniu modelu do zewnętrzengo programu (w parametrach importu domyślną jednostką były **centymetry**), widzimy że skala modelu jest poprawna (Fig. 9.5).

![Wyeksportowany model"](photoscan_09_05.png "Wyeksportowany model")
<sup>Fig. 9.5 Wyeksportowany model</sup>

## 10. Kalibracja koordynat (wersja PRO)

Ostatnim krokiem w procesie kalibracji sceny, jest kalibracja koordynat. Poprawnia orientacja obiektu względem systemu koordynat ułatwia póżniejszą pracę przy optymalizacji modelu w zewnętrznych programach.

Przy kalibracji koordynat bardzo pomocny jest autorski skrypt, stworzony przez studio Inifinite Realities.

> Photoscan obsługuje autorskie skrypty tylko w wersji Pro.

[Skrypty Agisoft Pro](http://www.ir-ltd.net/uploads/posts/Agisoft-Photoscan-PRO-scripts.zip)

Po ściągnięciu skryptów, kopiujemy zawartość archiwum do folderu `C:\Users\[username]\AppData\Local\Agisoft\PhotoScan Pro\scripts`. Nasatępnie restartujemy program. Skrypty uruchamiamy z menu `Custom Scripts`.

Następnie ustawiamy widok na ortograficzny i używając ortograficznych rzutów opisanych w 3 kroku (Front, Top, Right, etc.), obracamy region zakrezu (`Bounding box`, Fig. 10.1), tak aby wcięcia znajdowały się po prawej stronie, a czerwona płaszczyzna z tyłu modelu (Fig. 10.2). 

![Custom scripts](photoscan_10_01.png "Custom scripts")
<sup>Fig. 10.2 Custom scripts</sup>

![Standardowy system koordynat](photoscan_10_02.png "Standardowy system koordynat")
<sup>Fig. 10.2 Standardowy system koordynat</sup>

Jeżeli eksportujemy do formatu FBX, czerwona płąszczyzna powinna znajdować się u góry modelu (Fig. 10.3).

![Koordynaty FBX](photoscan_10_03.png "Koordynaty FBX")
<sup>Fig. 10.3 Koordynaty FBX (osie Z i X zamienione)</sup>

Następnie uruchamiamy skrypt z menu `Custom menu > Cordinates to bounding box + rotate`.

Ostatnią czynnością jaką należy wykonać jest ustalenie punktu `zero` sceny. Punkt ten znajduje się w centrum regionu zakresu. Aby dokładniej określić położenie punktu zero, możemy pomniejszyć region zakresu (Fig. 10.1, 10.4).

![Określanie punktu zero](photoscan_10_04.png "Określanie punktu zero")
<sup>Fig. 10.4 Określanie punktu zero</sup>

> **UWAGA!**   
> Jeżeli przy obracamy również sam model, zmienia się przy tym też orientacja regionu zakresu. Po rotacji modelu należu ponownnie zorientować bounding box, a następnie uruchomić skrypt.

## 11. Eksport

* Decimate
* FBX
* Obj
* 3DS Max
* zBrush

## Apendiks

### Jak fotografować

* Use a digital camera with reasonably high resolution (5 MPix or more).
* Avoid ultra-wide angle and fish-eye lenses. The best choice is 50 mm focal length (35 mm film equivalent) lenses. It is allowed to vary from 20 to 80 mm.
* Fixed lenses are preferred. If zoom lenses are used - focal length should be set either to maximal or minimal value. Using RAW data losslessly converted to the TIFF files is preferred, since JPG compression induces unwanted noise to the images.
* Take images at maximal possible resolution.
* ISO should be set to the lowest value, otherwise high ISO values will induce additional noise to images.
* Aperture value should be high enough to result in sufficient focal depth: it is important to capture sharp, not blurred photos.
* Shutter speed should not be too fast, otherwise blur can occur due to slight movements. Avoid not textured, shiny, mirror or transparent objects.
* If still have to, shoot shiny objects under a cloudy sky.
* Avoid unwanted foregrounds.
* Avoid moving objects within the scene to be reconstructed.
* Avoid absolutely flat objects or scenes. PhotoScan operates with the original images. So do not crop or geometrically transform, i.e. resize or rotate, the images.
* More photos is better than not enough.
* Number of "blind-zones" should be minimized since PhotoScan is able to reconstruct only geometry visible from at least two cameras.
* In case of aerial photography the overlap requirement can be put in the following figures: 60% of side overlap + 80% of forward overlap.
* Each photo should effectively use the frame size: object of interest should take up the maximum area. In some cases portrait camera orientation should be used.
* Do not try to place full object in the image frame, if some parts are missing it is not a problem whereas these parts appear on other images.
* Good lighting is required to achieve better quality of the results, yet blinks should be avoided. It is recommended to remove sources of light from camera fields of view.
* If you are planning to carry out any measurements based on the reconstructed model, do not forget to locate at least two markers with a known distance between them on the object. Alternatively, you could place a ruler within the shooting area.
* In case of aerial photography and demand to fulfil georeferencing task, even spread of ground control points (GCPs) (at least 10 across the area to be reconstructed) is required to achieve results of highest quality, both in terms of the geometrical precision and georeferencing accuracy. Yet, Agisoft PhotoScan is able to complete the reconstruction and georeferencing tasks without GCPs, too.

### Maskowanie zdjęć

[Maskowanie i rekonstrukcja - video tutorial](https://www.youtube.com/watch?v=2_insfYWPkA)