# Notatki do prezentacji — Deformacje terenu w Salonikach

## Slajd 1: Deformacje terenu w Salonikach

Ok. 20 s. „Dzień dobry. Przedstawimy analizę ruchów terenu w Salonikach na podstawie danych z europejskiej usługi EGMS, czyli European Ground Motion Service programu Copernicus. Na zdjęciu w tle widać nasz obszar: zatokę, lotnisko Makedonia wchodzące pasem startowym w morze i rolniczą dolinę na wschód od niego. Chcieliśmy odpowiedzieć na trzy pytania: które punkty zachowują się nietypowo, jakie typy zachowania tu występują i które budynki leżą w strefach ruchu terenu.”

## Slajd 2: Obszar badań i dane

Ok. 40 s. EGMS to bezpłatna usługa Copernicusa: z radarowych zdjęć satelitów Sentinel-1 liczone są przemieszczenia terenu z dokładnością do milimetrów. Użyliśmy produktu Ortho, w którym punkty są uśrednione do siatki co 100 metrów — każdy punkt opisuje ruch terenu w kwadracie 100 na 100 metrów, a nie konkretny obiekt. To będzie ważne przy budynkach. Pomiary są mniej więcej co 6 dni, bez braków danych. Większość punktów osiada powoli, około 1,6 mm na rok, ale są punkty osiadające prawie 27 mm na rok — tych szukaliśmy.

## Slajd 3: Metoda analizy

Ok. 45 s. Model nie dostaje całego szeregu 303 pomiarów, tylko trzy cechy, które da się zinterpretować. Trendy policzyliśmy sami regresją liniową — korelacja z prędkością podaną przez EGMS wynosi 0,999, więc obliczenia są poprawne. Isolation Forest nie wie, co jest „uszkodzeniem”; wskazuje punkty, które najłatwiej odizolować od reszty, czyli najbardziej nietypowe. Próg 5% jest umowny. Przy doborze liczby klas: najwyższy silhouette jest dla dwóch klas, ale taki podział rozdziela tylko teren stabilny i osiadający. Dla czterech klas jakość jest podobna jak dla trzech czy pięciu, a klasy mają czytelny sens. Histogram po prawej pokazuje, że rozkład prędkości ma długi ogon w stronę szybkiego osiadania.

## Slajd 4: Cztery klasy zachowania punktów

Ok. 60 s — najważniejszy slajd. Każdy kwadrat to jeden punkt EGMS, kolor to klasa z K-means, czarny obrys to anomalia. Ponad połowa punktów, klasa zielona, to stabilna zabudowa na północy obszaru. Klasa żółta osiada tak samo wolno, ale w ostatnim roku przyspieszyła — warto ją obserwować. Najważniejsze są klasy 3 i 4, pomarańczowa i bordowa. Proszę zwrócić uwagę, że nie są to pojedyncze, losowe punkty, tylko jedna zwarta strefa w niskiej dolinie rzeki Anthemountas: od wybrzeża przy lotnisku na wschód, aż po pola uprawne. Taki układ przestrzenny wskazuje na przyczynę o charakterze regionalnym, a nie na błędy pomiaru. Klasa 3 leży bardziej na południowym wschodzie, na terenach rolniczych — wrócimy do tego przy interpretacji.

## Slajd 5: Przebieg przemieszczeń w czasie

Ok. 45 s. Wykres pokazuje medianę szeregu w każdej klasie, czyli typowy przebieg punktu z danej klasy. Zielona i żółta linia to praktycznie teren stabilny. Bordowa, klasa 4, to stałe, prawie liniowe osiadanie — około 10 mm rocznie. Najciekawsza jest pomarańczowa, klasa 3: co roku teren obniża się w okresie od wiosny do jesieni, a zimą częściowo wraca, a mimo to ogólny trend jest silnie ujemny. Taki rytm jest typowy dla zmian poziomu wód podziemnych: latem pompuje się wodę do nawadniania pól, poziom wody spada i osady się zagęszczają, zimą zasoby częściowo się odnawiają. Żółta klasa jest sygnałem ostrzegawczym — w 2024 roku zaczęła odchodzić od stabilnej klasy.

## Slajd 6: Anomalie wskazane przez Isolation Forest

Ok. 40 s. Sprawdziliśmy, czy anomalie nie są po prostu błędami pomiaru. Gdyby były, wynik anomalii byłby silnie związany z RMSE, czyli z tym, jak bardzo szereg jest „poszarpany” — a korelacja to tylko 0,30. Sprawdziliśmy też, czy model nie wskazuje po prostu najszybszych punktów: z 20 najbardziej nietypowych tylko 13 jest wśród 20 najszybciej osiadających; pozostałe są nietypowe przez zmianę tempa w ostatnim roku albo silną sezonowość. Na wykresach sześć punktów z najwyższym wynikiem: to punkty, które w pięć lat obniżyły się o 11 do 13 centymetrów. Część anomalii tworzy jedno bardzo zwarte skupisko — pokażemy je na następnym slajdzie.

## Slajd 7: Skupisko anomalii na końcu pasa startowego

Ok. 35 s. To jest ten zwarty blok anomalii, który widać na mapie klas przy wybrzeżu. Po nałożeniu go na zdjęcie satelitarne okazało się, że punkty leżą dokładnie na sztucznym nasypie, który wchodzi w morze — na nim przedłużono pas startowy. Nasyp usypany na miękkich osadach dna zatoki osiada przez lata, bo i materiał nasypu, i osady pod nim stopniowo się zagęszczają. To dobry przykład, że Isolation Forest wskazuje miejsca o innej przyczynie ruchu niż reszta obszaru — tu przyczyną jest budowla, a nie pobór wody. Zaznaczmy, że to interpretacja na podstawie zdjęcia; nie mamy danych o samej konstrukcji nasypu.

## Slajd 8: Budynki w strefach osiadania

Ok. 40 s. Każdemu budynkowi przypisaliśmy najbliższy punkt EGMS w promieniu 50 metrów — to połowa oczka siatki, więc budynek dostaje punkt kwadratu, w którym leży. Dwie trzecie budynków jest w stabilnej klasie 1. W strefach silnego osiadania jest około 1500 budynków, z czego 375 stoi przy punktach anomalnych — najwięcej w klasie 3, to aż 35% budynków tej klasy. Pełną listę budynków wymagających uwagi, posortowaną od najszybszego osiadania, oddajemy w pliku CSV. Ważne zastrzeżenie: punkt z siatki 100 m opisuje ruch terenu w otoczeniu budynku, a nie ruch samego budynku. To wskazanie, gdzie warto prowadzić dokładniejsze pomiary, a nie ocena stanu technicznego.

## Slajd 9: Prawdopodobna przyczyna: pobór wód podziemnych

Ok. 40 s. Trzy rzeczy wskazują na jedną przyczynę. Po pierwsze położenie: silne osiadanie tworzy spójną strefę w niskiej dolinie rzeki Anthemountas, wypełnionej miękkimi, ściśliwymi osadami — na zdjęciu widać, że punkty klasy 3 leżą wśród pól uprawnych. Po drugie rytm: teren opada latem i częściowo wraca zimą, tak jak zmienia się poziom wód podziemnych przy sezonowym pompowaniu wody do nawadniania. Po trzecie literatura: Raspini i współautorzy w 2013 roku, na danych satelitarnych ERS z lat 1995–2001, znaleźli w tym basenie najszybsze osiadanie właśnie przy lotnisku, ponad 20 mm na rok, i powiązali je z intensywnym poborem wód podziemnych. Badanie na danych Sentinel-1 z lat 2015–2019 w Remote Sensing z 2020 roku pokazało, że osiadanie trwa. Nasze dane z lat 2020–2024 pokazują, że proces trwa nadal. To hipoteza zgodna z danymi i literaturą — nie mamy pomiarów poziomu wód, żeby potwierdzić ją wprost.

## Slajd 10: Wnioski i ograniczenia

Ok. 40 s. Podsumowując: większość obszaru — zabudowa Kalamarii, Thermi i Triadi — jest stabilna. Około 15% punktów osiada szybko i tworzy jedną strefę w dolinie Anthemountas, wokół lotniska i na polach na wschód od niego. Isolation Forest wskazał dwa różne typy miejsc: nasyp pasa startowego, który osiada z powodu konsolidacji, oraz pola z sezonowym osiadaniem, prawdopodobnie przez pobór wody. W strefach silnego osiadania jest około 1500 budynków — te warto objąć dokładniejszymi pomiarami, na przykład niwelacją. Ograniczenia: siatka 100 m opisuje teren, a nie budynek; próg 5% przyjęliśmy umownie; nie mieliśmy pomiarów naziemnych ani danych o poziomie wód. Cały kod, wyniki i projekt QGIS są w repozytorium. Dziękujemy, chętnie odpowiemy na pytania.
