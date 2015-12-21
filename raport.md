# ZED Raport
Andrzej Nowicki  
Data przygotowania raportu: date: 21 grudzień 2015




## Podsumowanie
Blablabla

## Wstęp

W trakcie przygotowywania raportu zostały wykorzystane następujące biblioteki:

```r
library(dplyr)
library(ggplot2)
library(knitr)
```
Aby zapewnić powtarzalność wyników przy kolejnych uruchomieniach raportu ustawiono stałe ziarno generatora liczb pseudolosowych:

```r
set.seed(100)
```
Dane z pliku zostały wczytane za pomocą polecenia:

```r
r<-read.csv2("all_summary.txt",nrows=max_rows)
# max_rows pozwala na ograniczenie zbioru danych w celu przyspieszenia obliczeń w trakcie przygotowywania raportu
```

Usunięcie z danych wierszy o określonej wartości zmiennej res_name


```r
delete_list <- c("DA","DC","DT", "DU", "DG", "DI","UNK", "UNX", "UNL", "PR", "PD", "Y1", "EU", "N", "15P", "UQ", "PX4","NAN")
r <- r %>% filter(!res_name %in% delete_list)
```
Pozostawienie tylko unikatowych pary wartości (pdb_code, res_name)

```r
r_distinct <-r %>% distinct(pdb_code, res_name) 
```

Przed usunięciem było 1995 wierszy. Po usunięciu duplikatów jest 662 wierszy.

TODO: Krótkie podsumowanie wartości w każdej kolumnie;

TODO: Sekcje sprawdzającą korelacje między zmiennymi; sekcja ta powinna zawierać jakąś formę graficznej prezentacji korelacji;

TODO: Określenie ile przykładów ma każda z klas (res_name);

```r
classes_occurences <- r_distinct %>% group_by(res_name) %>% summarise(count=n())
top_classes <- classes_occurences %>% arrange(desc(count)) %>% top_n(10,count)
kable(top_classes)
```



|res_name | count|
|:--------|-----:|
|SO4      |    57|
|CA       |    40|
|ZN       |    38|
|GOL      |    36|
|MG       |    23|
|CL       |    20|
|EDO      |    19|
|NAG      |    19|
|HEM      |    18|
|PO4      |    14|

TODO: Wykresy rozkładów liczby atomów (local_res_atom_non_h_count) i elektronów (local_res_atom_non_h_electron_sum);


TODO: Próbę odtworzenia następującego wykresu (oś X - liczba elektronów, oś y - liczba atomów): Wykres liczby atomów i elektronów

TODO: Tabelę pokazującą 10 klas z największą niezgodnością liczby atomów (local_res_atom_non_h_count vs dict_atom_non_h_count) i tabelę pokazującą 10 klas z największą niezgodnością liczby elektronów (local_res_atom_non_h_electron_sum vs dict_atom_non_h_electron_sum;)

TODO: Sekcję pokazującą rozkład wartości wszystkich kolumn zaczynających się od part_01 z zaznaczeniem (graficznym i liczbowym) średniej wartości;

TODO: Sekcję sprawdzającą czy na podstawie wartości innych kolumn można przewidzieć liczbę elektronów i atomów oraz z jaką dokładnością można dokonać takiej predykcji; trafność regresji powinna zostać oszacowana na podstawie miar R^2 i RMSE;

TODO: Sekcję próbującą stworzyć klasyfikator przewidujący wartość atrybutu res_name (w tej sekcji należy wykorzystać wiedzę z pozostałych punktów oraz wykonać dodatkowe czynności, które mogą poprawić trafność klasyfikacji); klasyfikator powinien być wybrany w ramach optymalizacji parametrów na zbiorze walidującym; przewidywany błąd na danych z reszty populacji powinien zostać oszacowany na danych inne niż uczące za pomocą mechanizmu (stratyfikowanej!) oceny krzyżowej lub (stratyfikowanego!) zbioru testowego.

