# SO
Pytania do SO( lepiej nie można)<br>
WAŻNE INFO:
- egzamin z 2025 roku jest kopią z 2024 roku, więc duże prawdopodobieństwo, że będzie to samo
- pytania w readme to są w całości z wykładu, a odpowiedzi są bazowane na wykładach z CEZ
- miłej nauki życzę


## Wykład 1
1. **Jaka jest różnica pomiędzy systemem czasu rzeczywistego hard real time, a soft real time?**

   W systemach hard real-time odpowiedź na wystąpienie zdarzenia musi nastąpić przed upływem ściśle zdefiniowanego czasu, ponieważ poprawność całego systemu jest uzależniona od spełnienia tego czasowego rygoru. Natomiast w systemach soft real-time zadania obsługiwane w czasie rzeczywistym dostają wyższy priorytet (pierwszeństwo przed innymi), ale system nie daje twardej gwarancji, że ostateczny czas reakcji na pewno nie zostanie przekroczony.

2. **Opisz jak wygląda praca wsadowa z perspektywy użytkownika systemu.**

   Programista koduje swoje zadanie wraz z danymi (np. używając pliku kart perforowanych) i po prostu przekazuje je operatorowi komputera. Następnie operator łączy zgromadzone zadania o zbliżonym charakterze w pojedynczy wsad (batch), który trafia do wykonania. Programista nie ma w tym procesie żadnej kontroli nad przebiegiem uruchomienia i musi czekać na gotowe wyniki, co często trwa kilka godzin.

3. **Jaka jest różnica między jednoprogramowym systemem wsadowym, a wieloprogramowym systemem wsadowym?**

   W jednoprogramowym systemie wsadowym do pamięci ładuje się naraz tylko jeden program użytkownika. Kiedy program ten zgłasza potrzebę operacji wejścia-wyjścia, procesor czeka bezczynnie na jej finał. Z kolei w wieloprogramowym systemie wsadowym utrzymuje się w pamięci kilka programów jednocześnie. Gdy jedno z zadań wstrzymuje się oczekując na operację we-wy, procesor natychmiast jest przekazywany innemu gotowemu zadaniu, minimalizując czas bezczynności sprzętu.

4. **Dlaczego powłoka (shell) w Linuxie nie jest uważana za część jądra (kernel) systemu?**

   Jądro to podstawowy i kluczowy komponent systemu operacyjnego, który zarządza zasobami całej maszyny i funkcjonuje w uprzywilejowanym trybie procesora. Shell (interpreter poleceń) to tak zwany program systemowy i wykonuje się na prawach zwykłego programu użytkownika, działając poza monolityczną przestrzenią uprzywilejowaną jądra.

## Wykład 2
1. **Czy wpadnięcie w nieskończoną pętlę zatrzyma system operacyjny Unix/Linux?**

   Nie, nie zatrzyma. Do zabezpieczenia systemu służy przerwanie zegara, które chroni procesor przed programami uwięzionymi w nieskończonej pętli. Dzięki przerwaniom zegarowym system operacyjny cyklicznie odzyskuje kontrolę i może np. usunąć błędny proces lub przekazać moc obliczeniową innemu zadaniu.

2. **Podaj przykłady instrukcji uprzywilejowanych.**

   Instrukcje uprzywilejowane to na przykład instrukcje do obsługi i komunikacji z urządzeniami we-wy , blokowanie i odblokowywanie przerwań , dokonywanie zmian w wektorze przerwań , a także komendy modyfikujące kluczowe rejestry ochronne pamięci (jak rejestr bazowy i rejestr limitu).

3. **Podaj przykład wyjaśniający problem spójności pamięci podręcznej (cache).**

   W systemie wieloprocesorowym, każdy z procesorów posiada własną, niezależną pamięć podręczną. Problem spójności powstaje, gdy ta sama komórka pamięci znajduje się w cache obydwu procesorów i procesor A zdecyduje się ją nadpisać. Jeśli następnie procesor B spróbuje z niej odczytać wartość, a dysponuje tylko starym odzwierciedleniem komórki z własnego cache'a, dojdzie do odczytu błędnych (niezaktualizowanych) informacji, co jest esencją problemu braku spójności.

4. **Różnica między sync oraz async w we/wy (IO).**

   Podczas operacji synchronicznej proces żądający wejścia/wyjścia zostaje całkowicie wstrzymany do momentu pełnego ukończenia transmisji i dopiero wtedy następuje powrót z systemu operacyjnego do aplikacji. Przy wywołaniu asynchronicznym powrót następuje od razu po zainicjalizowaniu operacji, przez co procesor może realizować dalsze instrukcje tego samego programu w tle podczas trwania wejścia/wyjścia.

5. **Najszybsza i najwolniejsza metoda z trzech sposobów komunikacji z urządzeniami IO.**

   Bezpośredni dostęp do pamięci (DMA) jest najszybszy przy kopiowaniu bloków danych, ponieważ transfer przebiega bez ciągłego udziału procesora i to przy bardzo małym narzucie na poszczególny bajt. Metoda programowanego we/wy (PIO) wymaga bezczynnego oczekiwania procesora w pętli opóźniającej na zmianę statusu i z punktu widzenia obciążenia CPU jest najwolniejsza/najmniej optymalna. Ciekawostką jest natomiast, że w przypadku potężnych prędkości ponad 500 000 B/s technika we-wy sterowanego przerwaniami może doprowadzić do dławienia z uwagi na ekstremalnie częste przełączanie kontekstu. W tym specyficznym przypadku wręcz prymitywne PIO może być od niej szybsze.

6. **Podaj przykład procesu wykonującego się w trybie użytkownika, oraz procesu wykonującego się w trybie jądra.**

   Pojęcia te nie rozróżniają dwóch procesów, lecz opisują sposób egzekucji tego samego procesu w danym momencie czasu. Proces "wykonuje się w trybie użytkownika", kiedy przetwarza instrukcje należące wprost do własnego programu aplikacji. Kiedy jednak np. spróbuje otworzyć plik poprzez wywołanie systemowe open, oddaje pałeczkę funkcji systemu. W tym momencie wchodzi w "tryb jądra" – wciąż operując jako ten sam byt, po prostu realizuje kod jądra z racji dostępu do usług niskopoziomowych.

7. **Co to połączenie kontekstu (context switch)?**

   Gdy pojawia się na przykład przerwanie sprzętowe zdejmujące procesor procesowi, system ratuje cały jego stan. Przełączenie kontekstu następuje w sytuacji, gdy po podjęciu decyzji o zamianie bieżącego zadania system "odtworzy" z pamięci stan procesora zarezerwowany dla absolutnie innego procesu, po czym wyjdzie z procedury obsługi i podejmie egzekucję tamtego.

8. **Przykład niepowodzenia (fault).**

   Błędy wewnętrzne hardware'owe, określane mianem niepowodzeń to na przykład: pomyłki dzielenia przez zero , przepełnienia rejonu stosu , sygnalizowanie braku wymiernej strony fizycznej podczas operacji stronicowania albo po prostu naruszenie obowiązujących mechanizmów ochronnych.

## Wykład 3
1. **Czym różni się proces od programu?**

   Program należy określić pojęciem całkowicie statycznym. Jest to po prostu bierny zbiór informacji w postaci wykonywalnego pliku na przestrzeni dyskowej. Natomiast proces jest pojęciem dynamicznym, ponieważ to załadowany do pamięci operacyjnej i w pełni aktywny program. Stan procesu ulega ciągłym modyfikacjom wraz ze zmianą zawartości przydzielonej pamięci, stanu rejestrów i wartości licznika rozkazów.

2. **Czy możliwe jest przejście ze stanu (procesu) Oczekującego do Aktywnego? Jeśli tak, podaj scenariusz przejścia.**

   Nie, nie jest możliwe bezpośrednie przeskoczenie między tymi stanami. Wymagany jest stan pośredni. Ze stanu Oczekujący (kiedy czeka np. na przerwanie sprzętowe), zdarzenie wybudzające przenosi proces do stanu Gotowy. To dopiero ze stanu Gotowego system operacyjny powołuje go ponownie do wykonywania poleceń, wprowadzając go w stan Aktywny.

3. **Podaj przykład sytuacji w której proces będący w stanie aktywnym przechodzi w stan oczekujący**

   Mechanika ta zadziała wtedy, gdy program zatrzyma swą pracę, albowiem zażądał informacji niedostępnej od ręki – na przykład uruchomił operację wczytania synchronicznego poprzez urządzenie wejścia-wyjścia i ugrzązł czekając w letargu na finał tej transmisji.

4. **Podaj funkcję planisty (długo/krótko/średnio)-terminowego.**

   Planista długoterminowy: Zarządza przenoszeniem nowo powstałych zadań wejściowych do puli układów "Gotowych" w charakterystycznych rozwiązaniach wsadowych. Planista krótkoterminowy: Odpowiada wprost za przydział czasu procesora rozwiązując dylemat „któremu procesowi w stanie Gotowym trzeba udostępnić w danej chwili procesor" (steruje on akcjami z wchodzeniem w stan Aktywny i wypadaniem do Gotowy). Planista średnioterminowy: Nadzoruje procesami przenoszenia niewykorzystywanych czasowo procesów do odrębnego stanu "zawieszenia", zrzucając i zaczytując z powrotem ich ramy pamięci z dyskowego obszaru wymiany (swapping).

5. **Który planista jest najważniejszy i musi istnieć zawsze?**

   Kluczowy we wszystkich implementacjach systemów operacyjnych jest planista krótkoterminowy.

6. **Z dwóch operacji: przełączenie kontekstu i przełączenie trybu w systemie operacyjnym zaprojektowanym z myślą o procesorach bez bugów etc szybsze jest:… .**

   ...przełączenie trybu. Wejście z kodu programu użytkownika w wywołanie systemu poprzez przełączenie trybu nakłada w skali całej operacji drastycznie mniejszy koszt technologiczny, w stosunku do przeprowadzania zamiany całych zrzutów pamięci systemowych podczas przełączania kontekstu między osobnymi procesami.

7. **Co oznacza kod powrotu z funkcji fork() (3 wyjścia).**

   Zwrócenie wartości ujemnej (np. < 0) wskazuje, że nie udało się poprawnie utworzyć procesu (pojawił się błąd). Zwrócenie na zewnątrz idealnego zera jest informacją potwierdzającą wywołanie bezpośrednio wewnątrz nowego procesu potomnego. Powrót wartości dodatniej wysyła do aplikacji rodzicielskiej namacalny identyfikator utworzonego odnogi.

8. **Wymień 2 zalety wielowątkowości.**

   Operowanie i kasowanie z przestrzeni operacyjnej poszczególnych wątków absorbuje znacznie mniejszą ilość czasu niż operacje na ociężałych procesach. Implementacja wątków dopuszcza wyjątkowo szybkie przetasowanie kontekstów (bez wtrąceń sytemu) oraz natywne uruchomienie współbieżności wielordzeniowej dla tego samego procesu.

9. **Dwa wątki zostały stworzone wewnątrz jednego procesu wykonują funkcje f, a x jest zmienną lokalną auto. Jeżeli najpierw watek pierwszy wykona przypisanie x = 1 a pozniej watek drugi wykona przypisanie x = 2 to później watek pierwszy odczytując zmienna x odczyta … .**

   ...odczyta zawsze wartość wynoszącą 1. Każdy funkcjonujący wątek, mimo dostępu do wspólnej przestrzeni bazowej i zmiennych zadeklarowanych jako globalne, kategorycznie przechowuje swoje zmienne z funkcji lokalnych i argumenty na swym całkowicie odseparowanym, autonomicznym stosie. Zmiana jakiej dopuścił się wątek numer dwa operowała fizycznie w obszarze należącym do jego wirtualnego stosu. Wątek 1 widzi nadal swoją zmienną.

## Wykład 4
1. **Kiedy mówimy o sytuacji wyścigu?**

   O warunku wyścigu mówimy, gdy ostateczny wynik działania programu zależy od kolejności wykonywania instrukcji przez współbieżne procesy.

2. **Opisz warunki jakie powinno spełniać rozwiązanie problemu sekcji krytycznej**

   Wzajemne wykluczanie: W danej chwili tylko jeden proces może znajdować się w sekcji krytycznej. Postęp: Proces, który aktualnie nie wykonuje swojej sekcji krytycznej, nie może blokować innych procesów, które chcą do niej wejść. Ograniczone czekanie: Żaden proces nie może czekać w nieskończoność na wejście do sekcji krytycznej.

3. **Jak działa operacja s.wait, gdzie s jest semaforem zaliczającym?**

   Jeżeli wartość semafora $S>0$, to zmniejsza ją o 1 ($S=S-1$). W przeciwnym wypadku wykonywanie procesu zostaje wstrzymane (proces przechodzi w stan oczekujący).

4. **Podaj przykład zakleszczenia (definicja).**

   Definicja: Zbiór procesów znajduje się w stanie blokady (zakleszczenia), kiedy każdy proces z tego zbioru czeka na zdarzenie, które może zostać wywołane wyłącznie przez inny proces z tego samego zbioru. Przykład (życiowy): Samochody blokujące się nawzajem na skrzyżowaniu, gdzie żaden nie ma biegu wstecznego (brak wywłaszczeń zasobów). Przykład (procesy): Proces $P_{0}$ wszedł w posiadanie semafora A i czeka na zwolnienie semafora B przez proces $P_{1}$. Jednocześnie proces $P_{1}$ wszedł w posiadanie semafora B i czeka na zwolnienie semafora A przez proces $P_{0}$.

5. **Podaj definicję zakleszczenia.**

   Definicja: Zbiór procesów znajduje się w stanie blokady (zakleszczenia), kiedy każdy proces z tego zbioru czeka na zdarzenie, które może zostać wywołane wyłącznie przez inny proces z tego samego zbioru.

6. **Podaj przykład zagłodzenia (starvation).**

   Zagłodzenie to sytuacja, gdy proces czeka w nieskończoność, mimo że zdarzenie, na które czeka, regularnie występuje, ale reagują na nie inne procesy. Przykład 1: Jednokierunkowe przejście dla pieszych, przez które może przejść tylko jedna osoba. Jeśli z kolejki czekających zawsze wpuszczana jest najwyższa osoba, niska osoba może czekać tam w nieskończoność. Przykład 2: System przydzielający procesor zawsze najpierw procesom profesorów. Przy ich dużej liczbie proces studenta będzie czekał w nieskończoność.

7. **Wyspecyfikuj problem czytelników i pisarzy.**

   Modyfikacja problemu sekcji krytycznej, w której mamy współdzielony obiekt (czytelnię). W danej chwili w czytelni może przebywać: Jeden proces pisarza i absolutnie żaden czytelnik. Dowolna liczba czytelników pod warunkiem, że nie ma tam żadnego pisarza.

## Wykład 5
1. **Podaj różnice pomiędzy blokującą a nieblokującą operacją odbioru komunikatu.**

   Blokujące receive: Proces odbierający zostaje zablokowany i musi czekać aż do momentu fizycznego przekazania komunikatu. Nieblokujące receive: Proces nie musi czekać ; jeśli komunikatu nie ma, operacja natychmiast sygnalizuje jego brak.

2. **Wymień 2 czynności jakie wykonuje operacja C.wait()**

   Zawiesza wykonanie procesu. Jednocześnie zwalnia monitor, umożliwiając innym procesom wejście do niego.

3. **Jakie są ograniczenia obiektu w Java 4 traktowanego jako monitor?**

   Klasa (obiekt) w Javie w przybliżeniu odpowiada monitorowi wyposażonemu maksymalnie w jedną zmienną warunkową. Obniża to drastycznie wydajność synchronizacji, ponieważ po wywołaniu notifyAll() budzeni są wszyscy czekający, ale tylko jeden z nich może kontynuować, a reszta po sprawdzeniu warunku wraca do czekania.

4. **Korzystając z biblioteki POSIX Threads implementuje monitor z 3 zmiennymi warunkowymi. Potrzebuje do tego ... zmiennych typu sem_t , ... zmiennych pthread_mutex_t, ... zmiennych pthread_cond_t.**

   0 zmiennych typu sem_t (semafory to odrębny mechanizm). 1 zmiennych pthread_mutex_t (do wejścia/wyjścia i wzajemnego wykluczania wewnątrz monitora). 3 zmiennych pthread_cond_t (ponieważ każda zmienna warunkowa wymaga swojej deklaracji).
### Dodatkowe
- **Który wątek uruchomi się pierwszy? (po wait/signal; semantyka Mesa lub Hoare'a)**

  Mesa: Proces, który wywołał operację signal, kontynuuje jako pierwszy. Hoare'a: Proces obudzony (odblokowany) kontynuuje jako pierwszy.

- **Co robi operacja wait w wątkach?**

  Wątek natychmiast zwalnia blokadę, po czym zostaje uśpiony i umieszczony w kolejce wątków zawieszonych (wait set).

- **Jakie są argumenty funkcji come wait?**

  Zmienna warunku (condition) oraz muteks (mutex).

- **Co nie jest zmienną warunkową w monitorze?**

  Zmienną warunkową nie jest blokada (mutex) zapewniająca wzajemne wykluczanie przy wejściu do monitora — to ona pilnuje, by w środku monitora przebywał tylko jeden wątek, ale sama nie utrzymuje kolejki uśpionych wątków. Zmiennymi warunkowymi nie są też zwykłe zmienne współdzielone (chronione dane) ani procedury/metody monitora. Co ważne, w odróżnieniu od semafora zmienna warunkowa nie przechowuje żadnej wartości ani „pamięci" sygnałów: jeśli wątek wykona signal, gdy nikt nie czeka, sygnał ten przepada (nie jest zliczany). Zmienną warunkową jest wyłącznie obiekt obsługujący operacje wait/signal i zarządzający kolejką zawieszonych na nim wątków.

## Wykład 6
1. **Na czym polega postarzanie(aging)/szeregowanie procesów?**

   Polega na stopniowym zwiększaniu priorytetu procesu, który bardzo długo oczekuje na dostęp do procesora, co zapobiega jego zagłodzeniu.

2. **Który znany ci algorytm szeregowania zastosowałbyś w systemie z podziałem czasu?**

   Algorytm planowania rotacyjnego (Round Robin).

3. **Dlaczego w systemie z ochroną nie stosuje się planowania bez wywłaszczeń?**

   W systemie bez wywłaszczania procesowi nigdy nie odbiera się procesora w sposób wymuszony. Proces o bardzo dużym zapotrzebowaniu obliczeniowym mógłby zmonopolizować procesor, opóźniając wszystkie pozostałe procesy stojące za nim w kolejce.

4. **Co to jest wywłaszczenie (preemption)?**

   Jest to odebranie procesu procesora , wykonywane zazwyczaj za pomocą przerwań zegarowych. Wykonuje się je m.in., gdy proces przechodzi ze stanu aktywnego do gotowego (np. przerwanie) lub z oczekiwania do gotowego.

5. **Narysować trzy diagramy Ganta ilustrujące działanie cpu według 3 algorytmów:**
    - FCFS (First Come First Served),
      <br><img width="603" height="722" align="middle" alt="Zrzut ekranu 2026-06-23 232401" src="https://github.com/user-attachments/assets/b0645055-7460-4b62-859f-4fa21012c0ef" />
    - SJF (Shortest Job First) bez wywłaszczenia,
      <br><img width="613" height="367" align="middle" alt="Zrzut ekranu 2026-06-23 232412" src="https://github.com/user-attachments/assets/5f2b0a85-802a-4111-a2a9-6bb89d05eb5a" />
    - SJF (Shortest Job First) z wywłaszczeniem,
      <br><img width="567" height="487" align="middle" alt="Zrzut ekranu 2026-06-23 232417" src="https://github.com/user-attachments/assets/be4e3cce-daac-4216-9df5-5c8b6272c260" />
    - Round Robin (planowanie rotacyjne).

   (Ze względu na specyfikę środowiska opiszę, jak na podstawie materiałów wyglądają te diagramy). FCFS (First Come First Served): Procesy są dodawane na diagram w dokładnej kolejności ich zgłoszeń. Każdy z nich wykonuje się jednym ciągiem bez przerywania aż do końca swojej fazy. SJF (Shortest Job First): * Bez wywłaszczania: Procesy układa się w kolejności od najkrótszego zapotrzebowania czasowego do najdłuższego (biorąc pod uwagę procesy aktualnie gotowe). Wykonywane są ciągiem bez przerw. Z wywłaszczaniem (SRTF): Jeżeli podczas wykonywania procesu nadejdzie nowy proces z krótszym czasem do końca cyklu, obecny proces jest wywłaszczany i zastępowany krótszym. Diagram jest podzielony na więcej, krótszych bloków. Round Robin (planowanie rotacyjne): Diagram podzielony jest na równe kwanty czasu (np. 20 ms). Każdy proces po kolei z kolejki FIFO otrzymuje jeden kwant; jeśli go nie skończy, przerywa działanie i przechodzi na koniec kolejki (na diagramie przeplatają się bloki różnych procesów wielkości jednego kwantu).
### Dodatkowe
- **Czy planowanie bez wywłaszczeń może być dobrą strategią?**

  Tak, SJF (Shortest Job First) może funkcjonować bez wywłaszczania i jest algorytmem optymalnym do minimalizacji średniego czasu oczekiwania procesów na obsługę.

- **Systemy interakcyjne, wsadowe, czasu rzeczywistego.**

  Wsadowe: Liczy się stopień wykorzystania procesora oraz przepustowość (liczba zadań na jednostkę czasu). Interakcyjne: Najważniejszy jest czas reakcji systemu na zdarzenie. Czasu rzeczywistego: Kluczowe jest zaspokojenie z góry ustalonych terminów zadań oraz przewidywalność działania.

## Wykład 7
1. **Kiedy mówimy o fragmentacji wewnętrznej?**

   Fragmentacja wewnętrzna: Występuje w sytuacji, gdy obszar pamięci przydzielony dla procesu jest większy niż obszar niezbędny do jego działania.

2. **W jakim znanym ci mechanizmie zarządzania przestrzenią fragmentacja wewnętrzna jest najbardziej dotkliwa?**

   Najbardziej dotkliwy mechanizm dla fragmentacji wewnętrznej: Problem ten jest szczególnie widoczny i poważny w przypadku systemów stosujących partycje o stałych rozmiarach.

3. **Co to jest kompakcja (compaction)?**

   Kompakcja (compaction): Jest to operacja polegająca na przesuwaniu procesów w pamięci w taki sposób, aby cała wolna przestrzeń połączyła się i utworzyła jeden duży blok. Służy to likwidacji fragmentacji zewnętrznej.

4. **Jaka jest funkcja bufora translacji adresów (TLB)?**

   Funkcja bufora TLB (Translation Lookaside Buffer): TLB służy do przyspieszenia procesu translacji adresów logicznych na fizyczne, przechowując ostatnio używane mapowania numerów stron na numery ramek. Jeżeli numer strony znajduje się w TLB, procesor nie musi odwoływać się do tablicy stron w pamięci, co znacznie skraca czas dostępu.

5. **Jaka jest funkcja bitu trybu jądra w pozycji tablicy stron?**

   Bit trybu jądra w tablicy stron: Zabezpiecza on pamięć przed nieuprawnionym dostępem. Próba dostępu do strony z ustawionym bitem trybu jądra przez proces działający w trybie użytkownika generuje wyjątek stronicowania, chroniąc tym samym pamięć systemu operacyjnego.

6. **Dlaczego w praktycznych realizacjach implementowania stosuje się stronicowania 2 lub 3 poziomowe.**

   Stronicowanie wielopoziomowe: Stosuje się je, ponieważ jednostopniowa tablica stron zajmuje zbyt dużo ciągłej pamięci operacyjnej. Stronicowanie wielopoziomowe pozwala na poddanie samej tablicy stron procesowi stronicowania, oszczędzając pamięć i ułatwiając zarządzanie przestrzenią adresową.

## Wykład 8
1. **Symulacja zastępowania stron (algorytmu stronicowania) (FIFO, zegarowy, optymalny, LRU, NFU, itd.).**

   Symulacja zastępowania stron (Zasady działania algorytmów): FIFO: Zastępuje stronę, która została sprowadzona do pamięci jako pierwsza, opierając się na zwykłej kolejce. Zegarowy: Modyfikacja algorytmu FIFO wykorzystująca wskaźnik ("wskazówkę zegara") oraz bit R. Zastępuje stronę z bitem $R==0$, a stronom z $R==1$ zeruje bit, przesuwa wskazówkę i daje im "drugą szansę". Optymalny: Teoretyczny algorytm zastępujący stronę, do której proces nie będzie odwoływał się przez najdłuższy czas w przyszłości. LRU: Zastępuje stronę, która nie była używana przez najdłuższy czas w przeszłości. NFU: Algorytm zliczający odwołania; jako ofiarę wybiera stronę o najmniejszej wartości licznika.

2. **Co to jest szamotanie (trashing)?**

   Szamotanie (Trashing): Zjawisko występujące, gdy system ma zbyt mało wolnych ramek, co prowadzi do bardzo dużej liczby błędów braku strony. Procesor jest minimalnie obciążony, ponieważ procesy ciągle oczekują na operacje wejścia/wyjścia z dysku.

3. **Jak działa postarzanie (aging) w algorytmie NFU?**

   Postarzanie (Aging) w NFU: Polega na zmniejszaniu wartości liczników stron wraz z upływem czasu, aby zapobiec faworyzowaniu stron używanych dawno temu. W każdym cyklu zegara bity licznika przesuwane są w prawo (dzielenie przez dwa), a jeśli do strony nastąpiło odwołanie, najstarszy bit ustawiany jest na 1.

4. **Jaka jest różnica pomiędzy algorytmami FIFO a drugiej szansy?**

   Różnica między FIFO a algorytmem drugiej szansy: Algorytm drugiej szansy weryfikuje dodatkowo bit odniesienia (R). W odróżnieniu od klasycznego FIFO, jeśli strona na początku kolejki ma ustawiony bit $R==1$, nie jest od razu usuwana – jej bit jest zerowany, a strona trafia na koniec kolejki (otrzymuje drugą szansę).

5. **Co to anomalia Belady'ego?**

   Anomalia Belady'ego: Sytuacja, w której przydzielenie procesowi większej liczby ramek pamięci nie powoduje zmniejszenia, a wręcz zwiększa liczbę błędów braku strony.

6. **Omów znaczenie bitów D (Dirty) i R (Reference) w tablicy stron.**

   Bity w tablicy stron: Bit R (Referenced) jest automatycznie ustawiany na 1, gdy do danej strony nastąpi odwołanie. Bit D (Dirty) jest ustawiany na 1, jeśli zawartość strony w pamięci została zmodyfikowana.

## Wykład 9
1. **Mając dany ciąg, podaj kolejność obługi żądań przez dysk według algorytmu X (jeden z kilku algorytmów: SSTF, SCAN, C-LOOK).**

   Algorytmy obsługi żądań przez dysk (Zasady działania): SSTF: Głowica w pierwszej kolejności obsługuje żądanie o najkrótszym czasie wyszukiwania, czyli znajdujące się najbliżej jej aktualnego położenia. SCAN: Głowica porusza się od jednego końca dysku do drugiego, obsługując zgłoszone żądania po drodze, a po osiągnięciu brzegu zmienia kierunek. C-LOOK: Głowica przesuwa się w jednym kierunku, obsługując żądania tylko do miejsca ostatniego zgłoszenia (nie do samego brzegu talerza). Po jego obsłużeniu natychmiast wraca na początek, nie obsługując niczego po drodze.

2. **Różnica między macierzą RAID 4 a RAID 5?**

   Różnica między RAID 4 a RAID 5: W konfiguracji RAID 4 wszystkie bity parzystości przechowywane są na jednym dedykowanym dysku, który staje się wąskim gardłem przy zapisach. W RAID 5 bity parzystości (paski) są rozproszone równomiernie po wszystkich dyskach macierzy, co eliminuje problem wąskiego gardła.

3. **Dlaczego RAID 0 nie jest uważany za prawdziwą macierz RAID?**

   Brak redundancji w RAID 0: Poziom RAID 0 nie jest uważany za prawdziwą macierz, ponieważ nie oferuje on żadnej nadmiarowości (redundancji) danych chroniącej przed awarią.

4. **Dlaczego klasyczne algorytmy szeregowania dysku (np. SSTF) nie mają sensu w przypadku dysków SSD?**

   Szeregowanie w dyskach SSD: Zastosowanie algorytmów takich jak SSTF w dyskach SSD mija się z celem, ponieważ w odróżnieniu od dysków HDD nie posiadają one żadnych ruchomych elementów mechanicznych, których ruch trzeba by optymalizować.

5. **Wymień składowe czasu operacji dysku hdd, które z nich są dominujące a które są bardzo małe.**

   Czas operacji dysku HDD: Czas operacji składa się z czasu dostępu (który obejmuje czas wyszukiwania i czas oczekiwania) oraz czasu transferu. Czas wyszukiwania i czas oczekiwania to składowe dominujące, podczas gdy czas samego transferu danych jest bardzo mały (jest o trzy rzędy wielkości mniejszy).

6. **Co to jest dostęp surowy do dysku, kiedy on jest wykorzystywany, podaj jeden przykład.**

   Dostęp surowy do dysku: Jest to sposób odwoływania się do dysku z pominięciem warstwy systemu plików, traktujący dysk wyłącznie jako ciągłą tablicę sektorów. Służy do zwiększania wydajności. Przykładami wykorzystania dostępu surowego są bazy danych (np. Oracle) oraz narzędzia systemowe takie jak program fsck w systemach Unix.

## Wykład 10
1. **Jakiej struktury danych użyłbyś, aby sprawdzić, czy blok dyskowy o numerze i z dysku o numerze J jest w pamięci podręcznej dysku**
    - A) listy na wskaźnikach 
    - B) drzewo czerwono czarne 
    - C) tablica mieszająca 

   (C) Tablica mieszająca. Mechanizm ten pozwala na bardzo szybkie sprawdzenie, czy poszukiwany blok został już wczytany do pamięci podręcznej.

2. **Wiadomo, że na dysku HDD, im większy rozmiar bloku, tym większa średnia prędkość transmisji danych. Dlaczego rzadko albo wogole spotykane są systemy plików o rozmiarze bloku np. 16MB?**

   Większy rozmiar bloku powoduje znaczny spadek efektywności wykorzystania dostępnego miejsca na dysku. Przeciętnie z każdym zapisanym plikiem wiąże się utrata (tzw. fragmentacja wewnętrzna) obszaru równego połowie długości bloku. W przypadku bloku 16 MB powodowałoby to ogromne straty przestrzeni dla każdego mniejszego pliku.

3. **Jak działa strategia prealokacji bloków dyskowych i w przypadku jakich typów pamięci masowej się sprawdza?**

   Kiedy do zapisania pliku potrzebny jest jeden nowy blok, system z wyprzedzeniem przydziela mu od razu kilka bloków (np. 8 lub 16) w ciągłym obszarze na dysku. Czyni tak w nadziei, że rozrastający się plik wkrótce ich użyje. Jeśli nadmiarowe bloki nie zostaną wykorzystane, system zwalnia je w momencie zamykania pliku.

4. **Plik w UNIX ma 2KB. Ile bloków indeksowych potrzebuje ten plik?**

   Nie potrzebuje żadnego bloku indeksowego. W klasycznym Uniksie numery bloków dla małych plików (zwykle do 14 bloków danych) są przechowywane bezpośrednio w strukturze i-węzła.

5. **Podaj, gdzie przechowywane są atrybuty plików w systemie MS DOS i w systemie UNIX.**

   MS-DOS: Wszystkie atrybuty pliku (oraz numery początkowych bloków) przechowywane są bezpośrednio w danej pozycji katalogu. UNIX: Atrybuty pliku są odseparowane od katalogu i przechowywane w odrębnej strukturze systemowej nazywanej i-węzłem (ang. i-node).

6. **Która metoda alokacji danych oferuje najwyższą wydajność operacji seek(), a która najmniejszą?**

   Najwyższa wydajność: Ciągła alokacja bloków dyskowych pozwala na bardzo szybką operację seek() (oraz szybki odczyt), ponieważ wymaga jedynie obliczenia fizycznego przesunięcia. Najmniejsza wydajność: Alokacja listowa (bez użycia dodatkowych tablic w RAM typu FAT) nie nadaje się do swobodnego dostępu, ponieważ aby "przewinąć" plik, system musi odczytywać z dysku kolejne bloki powiązane ze sobą na liście.

7. **Dlaczego odczyt pliku odwzorowanego w pamięci (mmap) jest szybszy od klasycznego wywoływania funkcji read()?**

   Zmapowany plik staje się w całości częścią przestrzeni adresowej procesu. Pozwala to uniknąć podwójnego kopiowania danych z buforów jądra systemu operacyjnego do przestrzeni użytkownika.
### Dodatkowe
- **Co odczytuje funkcja C mmap()?**

## Wykład 11
1. **Podaj wadę i zaletę kryptografii asymetrycznej w porównaniu z symetryczną.**

   Zaleta: W asymetrycznej nie ma konieczności wcześniejszego, bezpiecznego uzgadniania tajnego klucza między stronami komunikacji, co rozwiązuje największy problem szyfrowania symetrycznego (ryzyko podsłuchu klucza w sieci). Wada: Techniki asymetryczne są bardzo wolne (nawet 1000 razy wolniejsze od symetrycznych), co stanowi duży problem wydajnościowy przy przesyłaniu bardzo długich wiadomości czy bieżącego ruchu sieciowego.

2. **Jak działa bit SUID dla plików wykonywalnych w UNIX?**

   Uruchomiony program posiadający ustawioną flagę SUID wykonuje się z uprawnieniami swojego właściciela (np. konta root), a nie z uprawnieniami zwykłego użytkownika, który wywołał dany plik.

3. **Jakiego uprawnienia potrzebujesz, aby usunąć plik w UNIX?**

   Aby móc usunąć dany plik, należy posiadać uprawnienia do zapisu w katalogu, w którym ten plik jest umieszczony.

4. **Wyjaśnij w którym momencie podczas otwarcia pliku funkcją open() i jego odczytu funkcją read() wykorzystywane są listy dostępu i listy uprawnień.**

   Listy dostępów (ACL i prawa dla plików) są weryfikowane jednorazowo, w momencie wykonywania operacji otwarcia pliku funkcją open(). W przypadku pomyślnego otwarcia system dodaje odpowiedni wpis do tablicy otwartych plików danego procesu, która pełni w tym momencie funkcję listy uprawnień. Podczas późniejszych wywołań np. funkcji read(), sprawdzana jest wyłącznie ta lista uprawnień; uprawnienia samego obiektu na dysku nie są powtórnie weryfikowane.

5. **Czy bezpieczeństwo absolutne jest możliwe do osiągnięcia? Odpowiedź uzasadnij.**

   Nie, bezpieczeństwo absolutne jest nieosiągalne. Możliwe jest jedynie tworzenie i ulepszanie mechanizmów zabezpieczeń, które sprawią, że naruszenia będą odpowiednio rzadsze i trudniejsze do przeprowadzenia.

## Wykład 12
1. **Omów jak działa atak wykorzystujący przepełnienie bufora.**

   Proces przyjmujący dane z zewnątrz posiada wyznaczony bufor w pamięci o stałej, ograniczonej długości. Jeżeli program (z powodu luki) nie sprawdzi ilości napływających informacji, a zostaną do niego dostarczone dane dłuższe niż pojemność bufora, nastąpi nadpisanie pamięci wykraczającej poza jego granice. Ponieważ zmienne lokalne oraz istotne adresy powrotu przechowywane są na tzw. stosie maszynowym, włamywacz może celowo skonstruować nadmiarowe dane, by precyzyjnie zamazać wskaźnik adresu powrotu. W konsekwencji zmieniony adres skieruje program do innej części pamięci (zawierającej np. wstrzyknięty złośliwy kod wewnątrz tego samego bufora), co poskutkuje jego wykonaniem z wysokimi uprawnieniami przejętego procesu.

2. **Omów jak działa atak polegający na wstrzyknięciu kodu.**

   Atakujący bazuje tu często na niefiltrowanym wczytywaniu argumentów lub zmiennych do wnętrza funkcji powłoki, np. komendy system w języku C. Przekazując niebezpieczne komendy, połączone w sprytny sposób separatorami z poprawnymi zmiennymi (np. dodanie po docelowej nazwie pliku znaków ; rm -rf /), intruz wymusza na programie nieprzewidziane dopisanie ich do ciągu poleceń, które system operacyjny posłusznie potem interpretuje i realizuje.

3. **Dlaczego wprowadzony przez procudentow procesorów bit execute disable, nie likwiduje wszystkich problemów związanych z przepełnieniem bufora?**

   Ustawienie sprzętowego bitu zapisu z reguły zapobiega co prawda odpaleniu obcego złośliwego skryptu wprost nadpisanego ze stosu maszynowego. Atakujący mogą jednak zastosować technikę zwaną "powrotem do biblioteki" (return-to-libc). W tym wariancie włamywacz konstruuje zawartość bufora tak, aby sfałszowany i nadpisany na stosie adres powrotu nie odwoływał się do szkodliwego payloadu z bufora, lecz prowadził prosto do legalnego, uprzywilejowanego kodu jednej z bibliotek (np. do funkcji system() wykonującej polecenia powłoki).

4. **Podaj przykład ataku DoS (albo DDoS) wykorzystującego podatność sprzętową albo podatność programową.**

   Podatność programowa (DoS): Znany błąd protokołu w starszym systemie Windows 95; system nie dokonywał weryfikacji poprawności odebranego pola określającego długość komunikatu w nagłówku TCP/IP. Wysłanie sfabrykowanego pakietu o niebotycznej długości (np. wskazującej 4 GB) doprowadzało system do katastrofalnej próby "kopiowania", co kończyło się wyczerpaniem i kompletnym zamazaniem pamięci RAM komputera. Podatność sprzętowa (DoS): Dawny błąd budowy procesorów z rodziny Intel pozwalał zwykłemu i uprawnionemu jako user atakującemu na celowe zlecenie instrukcji maszynowej ze specyficznym niedozwolonym prefiksem (0x0f), co potrafiło fizycznie wstrzymać lub wręcz "wyłączyć" procesor, blokując maszynę. Rozproszony atak (DDoS): Atakujący po cichu wykorzystują i infekują rzesze niezależnych urządzeń lub serwerów (tworząc tzw. botnet / armię zombie). Komputery te następnie na polecenie oszusta zaczynają częściowo otwierać, z olbrzymią częstotliwością, setki połączeń w tle – powoduje to całkowite przeciążenie docelowego serwera ofiary WWW i odcięcie dostępu zwykłym, legalnym użytkownikom.

## Wykład 13
1. **Co to jest fałszywe współdzielenie (false sharing) w systemach wieloprocesorowych?**

   Fałszywe współdzielenie występuje w sytuacji, gdy dwa procesory równolegle operują na dwóch różnych zmiennych (np. zmiennej $x$ i zmiennej $y$), co teoretycznie nie powinno rodzić konfliktów. Problem pojawia się, gdy zmienne te są umieszczone w pamięci tak blisko siebie, że trafiają do tego samego wiersza pamięci podręcznej (który zazwyczaj ma rozmiar 64 bajtów). Wiersz ten staje się z perspektywy sprzętowej współdzielony, co wymusza ciągłą i kosztowną czasowo synchronizację pamięci podręcznych między procesorami, powodując duże spadki wydajności.

2. **Wyjaśnij na czym polega problem utrzymywania spójności pamięci podręcznej w systemie gdzie każdy procesor ma własną pamięcią podręczną (cache).**

   Jeżeli system korzysta z indywidualnej pamięci podręcznej typu write-back dla każdego procesora, jeden procesor (np. A) może zapisać zaktualizowane dane wyłącznie do własnego cache, opóźniając zapis do pamięci głównej. Jeśli w tym samym czasie procesor B pobrał do swojej pamięci podręcznej wcześniejszą, nieaktualną już kopię tych danych, dochodzi do niespójności systemu. W efekcie obydwa procesory widzą pod tym samym adresem różne wartości. Aby temu zapobiec, stosuje się sprzętowe śledzenie magistrali, które jednak znacznie spowalnia zapis współdzielonych danych.

3. **Dlaczego znany z klasycznego UNIXa model synchronizacji oparty o niewywłaszczalne jądro i maskowanie przerwań nie sprawdzi się w systemach wieloprocesowych?**

   Model z klasycznego Uniksa zawodzi z dwóch powodów. Po pierwsze, użycie instrukcji blokującej przerwania wyłącza je wyłącznie na lokalnym procesorze; to samo przerwanie może zostać wciąż odebrane i obsłużone przez inny procesor. Po drugie, jeśli dwa procesy równocześnie wywołają funkcje systemowe, w środowisku wieloprocesorowym istnieje ryzyko, że oba procesory będą próbowały w tym samym czasie modyfikować tę samą strukturę danych w jądrze systemu.

4. **Dlaczego ciężko zbudować procesor o zegarze 30 GHz?**

   Wynika to z dwóch kluczowych ograniczeń praw fizyki: Ograniczenia prędkości światła: Sygnał w miedzianym przewodzie przemieszcza się z prędkością ok. 20 cm/ns. Przy bardzo wysokich taktowaniach (rzędu dziesiątek GHz), dystans, jaki prąd może przebyć w jednym cyklu zegara, drastycznie maleje (przy 10 GHz jest to tylko 2 cm), co uniemożliwia prawidłową komunikację wewnątrz dużego układu. Emisja ciepła: Moc rozpraszana przez układ elektroniczny jest wprost proporcjonalna m.in. do częstotliwości taktowania. Próba zbudowania procesora o tak wysokim zegarze spowodowałaby natychmiastowe stopienie się układu, ponieważ gęstość oddawanej mocy przekroczyłaby możliwości jakiegokolwiek dostępnego chłodzenia.

## Wykład 14
1. **Dlaczego system uniksowy zgodnie ze standardem POSIX1.b nie jest systemem klasy hard real time.**

   System Uniksowy zgodny ze standardem POSIX1.b klasyfikowany jest jako system klasy soft real-time. Oznacza to, że oferuje on specjalne klasy priorytetów (SCHED_FIFO, SCHED_RR) i gwarantuje procesom czasu rzeczywistego bezwzględne pierwszeństwo nad procesami zwykłymi, ale wciąż nie daje stuprocentowej, algorytmicznej gwarancji dotrzymania nieprzekraczalnych terminów na obsługę, co jest absolutnym wymogiem systemów hard real-time.

2. **Podaj warunek konieczny wykonalności szeregowania w systemie czasu rzeczywistego, wyjaśniając dane symbole: p, d, t, Suma (U = Σ(Cᵢ/Tᵢ) ≤ 1; Cᵢ=czas wykonania, Tᵢ=okres zadania)**

   Warunek konieczny wykonalności szeregowania mówi, że łączna suma stopni wykorzystania procesora (ang. utilization) wszystkich procesów nie może przekroczyć 100%, co zapisujemy jako $\sum u \le 1$, gdzie $u = t/p$. $p$ – okres (ang. period), czyli czas upływający pomiędzy kolejnymi zdarzeniami, które proces musi obsłużyć. $d$ – nieprzekraczalny termin (ang. deadline), czyli maksymalny czas na obsłużenie zdarzenia, liczony od momentu jego wystąpienia. $t$ – czas procesora, którego dany proces fizycznie potrzebuje na obsłużenie zdarzenia. Zachodzi między nimi relacja: $0 \le t \le d \le p$.

3. **Dlaczego system operacyjny z niewywłaszczalnym jądrem ma bardzo wysoki czas reakcji na zdarzenie.**

   Niewywłaszczalne jądro nie pozwala odebrać procesora wątkowi, dopóki ten wykonuje kod w trybie jądra. Jeśli zdarzenie dla priorytetowego procesu czasu rzeczywistego nastąpi właśnie w tym momencie, proces ten jest blokowany. Musi on bezczynnie czekać, aż kod aktualnie obecny w jądrze sam zakończy działanie (zwróci sterowanie do trybu użytkownika) lub dobrowolnie przejdzie w stan uśpienia, co drastycznie i w nieprzewidywalny sposób wydłuża opóźnienie (czas reakcji).

4. **Podaj przykład współczesnego systemu komputerowego wykorzystywanego powszechnie w Polsce i na świecie który wykorzystuje planowanie długoterminowe.**

   Flagowym przykładem wdrożenia planistów długoterminowych są sieciowe klastry obliczeniowe typu HPC (High Performance Computing) sprzęgnięte z systemami kolejkowymi. Polskim przykładem takiego wdrożenia była budowa super-klastra obliczeniowego w ramach projektu Clusterix, wykorzystującego optyczną sieć PIONIER. Systemy kolejkowe (np. OpenPBS) przyjmują złożone zadania w trybie wsadowym i uruchamiają program na wybranych węzłach roboczych, dopiero gdy zwolnią się wystarczające zasoby procesorów.

5. **Co to jest klaster? Podaj definicję.**

   Klaster to architektura zbudowana z wielu niezależnych komputerów, które są połączone siecią w taki sposób, że dla użytkownika końcowego pracują i dają wrażenie działania pojedynczej, spójnej maszyny o potężnej wydajności.

6. **Omów różnice pomiędzy sieciowym systemem operacyjnym, a rozproszonym systemem operacyjnym.**

   Podstawowa różnica kryje się w przejrzystości architektury dla użytkownika: W sieciowym systemie operacyjnym użytkownik ma pełną świadomość istnienia wielu różnych maszyn w sieci. Aby skorzystać z odległego zasobu, musi jawnie wywołać połączenie (np. ssh) lub zdefiniować operacje transferu. W rozproszonym systemie operacyjnym użytkownik w ogóle nie jest świadomy wielości węzłów. Korzystanie z odległych zasobów (plików, a nawet mocy obliczeniowej podlegającej niewidocznej migracji) następuje automatycznie i w pełni maskuje się pod interfejsem typowych zasobów lokalnych.

7. **Scan edf.**

   SCAN-EDF to hybrydowy algorytm szeregowania używany przy planowaniu dysku w systemach multimedialnych. Opiera się on na sortowaniu żądań dostępu do danych w oparciu o ich nieprzekraczalne terminy, czyli algorytm EDF (Earliest Deadline First). Jednakże te żądania, które charakteryzują się jednakowym lub bardzo podobnym terminem (często łączone w tzw. kwanty na zasadzie grupowania - batching), są obsługiwane i wybierane przez głowicę w trybie zoptymalizowanego ruchu sekwencyjnego, zgodnie z algorytmem SCAN.

8. **Szeregowanie dysku.**

   Tradycyjne algorytmy szeregowania dysku (jak SCAN czy C-SCAN) były projektowane z myślą o minimalizacji ruchów głowicy i czasów przesunięć (seek time), przez co są całkowicie obojętne na nieprzekraczalne terminy (deadlines). To dyskwalifikuje je w strumieniach multimedialnych (np. wideo), gdzie dane muszą dotrzeć na czas. Z kolei czysty mechanizm EDF obsłużyłby wszystko na czas, ale spowodowałby ciągłe, chaotyczne skakanie głowicy z jednego końca dysku na drugi, drastycznie obniżając sumaryczną przepustowość. Z tego powodu systemy multimedialne wymuszają łączenie minimalizacji ruchu z terminowością – stąd algorytmy kompromisowe pokroju omówionego wyżej SCAN-EDF.

