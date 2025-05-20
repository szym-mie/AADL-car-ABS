# AADL-car-ABS

System czasu rzeczywistego modelujący system ABS w samochodzie.

## Podstawowe komponenty

- kontroler układu z pamięcią RAM i ROM
- magistrale: wewn. i zewn.
- 4 czujniki prędkości obrotowej dla każdego koła
- 2 elektrozawory obu obwodów / 4 elektrozawory dla każdego koła
- pompa stabilizująca ciśnienie w układzie hamulcowym
- czujnik ciśnienia układu hamulcowego
- kontrolka informująca kierowce o aktywacji systemu
- kontrolka informująca o awarii systemu ABS / układu hamowania

## Interakcje w systemie

Mikroprocesor komunikuje się z pamięciami RAM i ROM poprzez wewnętrzną magistrale,
która sluży wyłącznie do celów oprogramowania układu, reszta urządzeń jest podłączona
do magistrali zewnętrznej tj. sensory i aktuatory.

Każde urządzenie ma własny wątek, wątki natomiast są pogrupowane wg typu urządzenia -
np. czujniki prędkości obrotowej obsługuje jeden proces. System zawiera zatem proces
zbierania danych predkości obrotowej, obsługi elektrozaworów, obsługi pompy,
sprawdzenia ciśnienia układu, kontrolek inforumujących o stanie systemu.

## Zachowanie systemu

Oprócz danych o prędkości obrotowej kół, istnieje szereg innych czynników wpływajacy
na parametry systemu - jednym z nich jest sam kierowca, który zmienia ciśnienie w
układzie hamulcowym za pomocą pedału hamulca. Ciśnienie ulega gwałtownym zmianom
w czasie działania elektrozaworów, które maja kilka razy na sekundę zmieniają swoje
położenia na:
1. stan otwarcia, ciśnienie w cylindrze jest wyrównane z resztą systemu
2. stan zamknięcia, ciśnienie w cylindrze hamulcowym pozostaje stałe
3. stan redukcji, ciśnienie w cylindrze ulega częsciowemu obniżeniu

Ciśnienie w układzie hamucowym jest bardzo dynamiczne w przypadku aktywacji systemu.
W celu stabilizacji ciśnienia w głównym układzie hamulcowym podczas pracy 
elektrozaworów, uruchamiana jest pompa doładowująca układ płynem hamulcowym upuszczonym
z cylindrów układu hamulcowego. Pompa jest modulowana w taki sposób aby ciśnienie
utrzymywało się na zadanym przez kontroler poziomie.

Wykrywanie wczesnych objawów poślizgu kół polega na porównywaniu prędkości kół i
detekcji nadmiernych wartości deakceleracji obrotowej kół. W tym wariancie systemu
w przypadku wykrycia poślizgu następuje redukcja ciśnienia w hamulcach położonych
na przeciwległych końcach pojazdu, tj. przednie prawe koło i tylnie lewe koło i 
vice versa. 

Kontrolki stanu systemu informują kierowce o zadziałaniu systemu i ew. uszkodzeniu układu
ABS lub/i hamulcowego.
