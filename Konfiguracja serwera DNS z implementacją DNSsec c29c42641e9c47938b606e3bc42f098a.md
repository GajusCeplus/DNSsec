# Konfiguracja serwera DNS z implementacją DNSsec

---

---

# Konfiguracja Serwera DNS

- Przygotowanie środowiska:
    
    *Do zadań należy wykorzystać trzy dowolne serwery VPS posiadające publiczny adres IPv4, oraz adres IPv6 unikalny globalny.*
    
    *Do czynności wymagających systemu Windows należy użyć własnego systemu "gospodarza" na którym się pracuje.*
    

## Scenariusz zadania

Jesteś administratorem sieci w firmie Northwind Traders (nwtraders.msft).

W organizacji zapadła decyzja, aby zakupić domenę {wybierz_co_chcesz}.pl, która ma działać w publicznym systemie DNS.
Z pewnych różnych powodów, otrzymałeś polecenie, aby dla tej zakupionej domeny DNS, utrzymywać jej strefę na dwóch serwerach własnych firmy (posiadających publiczny adres IP), jak również z wydelegowaną domeną na potrzeby jednego z oddziałów gliwice.{wybierz_co_chcesz}.pl, utrzymywaną również na jednym serwerze własnym firmy (posiadającym publiczny adres IP).

Twoim zadaniem jest ponadto:

- zarejestrować/wykupić domenę *{wybierz_co_chcesz}.pl* (tutaj można wybrać sobie dowolną wolną w publicznym systemie DNS domenę z dowolną nazwą i dowolnym rozszerzeniem), która ma działać w publicznym systemie DNS

---

Na potrzeby zadania zakupiłem domenę pawlaczkowo.pl.

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled.png)

---

- skonfigurować z wykorzystaniem dwóch dowolnych serwerów VPS (posiadających publiczny adres IPv4 oraz adres IPv6 unikalny globalny) zakupioną domenę DNS tak, aby na jednym była przechowywana strefa podstawowa, a na drugim strefa zapasowa

---

Ustawienia w serwerze zapasowym, serwer podstawowy punkt niżej.

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%201.png)

---

- skonfigurowanie podstawowego serwera DNS tak, aby automatycznie powiadamiał serwer podrzędny o zmianach dokonanych w strefie, i uniemożliwiał nikomu poza serwerem podrzędnym transferu strefy, jak również uniemożliwiał żadnemu urządzeniu dynamicznej modyfikacji rekordów w strefie domeny

---

Ustawienia strefy na serwerze podstawowym.

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%202.png)

---

- skonfigurowanie podstawowego serwera DNS tak, aby rekordy które "trafią" do pamięci podręcznej klientów DNS (komputerów na których ktoś wpisał do jakiejś aplikacji naszą poddomenę), wygasały w tej pamięci podręcznej po 2 godzinach (czyli czas reakcji klientów w sieci, propagacji dokonanej zmiany w sieci, na dokonywane przez administratorów zmiany w ramach strefy, np. zmiana adresu IP dla poddomeny, ma być nie większy niż dwie godziny).

---

Konfiguracja pliku strefy:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%203.png)

---

- skonfigurować z wykorzystaniem jednego (tego trzeciego) serwera VPS tak, aby była na nim przechowywana strefa podstawowa domeny *gliwice.{wybierz_co_chcesz}.pl* (czyli chodzi o poddomenę "*gliwice"* dla wybranej wcześniej dowolnej domeny z dowolnym rozszerzeniem), a następnie skonfigurować dla niej delegację domeny w ramach strefy podstawowej *{wybierz_co_chcesz}.pl*

---

Konfiguracja strefy na serwerze DNS3:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%204.png)

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%205.png)

Delegacja poddomeny na serwerze DNS1:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%206.png)

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%207.png)

---

- na wszystkich serwerach VPS, poza serwerem podstawowym dla domeny *{wybierz_co_chcesz}.pl* należy wyłączyć możliwość obsługi zapytań rekursywnych, dla wszystkich za wyjątkiem własnego komputera (pozostawione tak na cele diagnostyczne)

---

W liście acl umieściłem publiczny adres ip komputera, z którego łączyłem się do serwerów.

Konfiguracja pliku */etc/bind/named.conf.options* taka sama na systemach DNS2 i DNS3.

![Bez tytułu.png](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Bez_tytuu.png)

---

- skonfigurować na serwerze utrzymującym podstawową strefę dla domeny *gliwice.{wybierz_co_chcesz}.pl,* dodatkowo strefę podstawową dla lokalnej domeny DNS "{*nazwisko}.pl*" (gdzie należy podstawić swoje własne nazwisko), i następnie skonfigurować na serwerze utrzymującym podstawową strefę domeny *{wybierz_co_chcesz}.pl* usługę warunkowego przesyłania dalej na potrzeby tej domeny "{*nazwisko}.pl*" (tak, aby jeżeli ktoś wyśle zapytanie DNS dotyczące dowolnej domeny DNS "{*nazwisko}.pl*" do serwera utrzymującego podstawową strefę domeny *{wybierz_co_chcesz}.pl,* to aby tenże potrafił w sposób właściwy obsłużyć to zapytanie)

---

Konfiguracja strefy na serwerze DNS3:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%208.png)

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%209.png)

Utworzenie strefy przesyłania dalej w serwerze DNS1:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2010.png)

---

- utworzyć w ramach tejże domeny:
    - *{wybierz_co_chcesz}.pl* oraz *www.{wybierz_co_chcesz}.pl* nakierowane na adres IPv4 oraz IPv6 serwera VPS ze strefą podstawową
    - *sklep.{wybierz_co_chcesz}.pl* nakierowane w ramach mechanizmu pseudo-loadbalancingu na adres IPv4 oraz IPv6 obydwu serwerów VPS (ze strefą podstawową i zapasową)
    - *{wybierz_co_chcesz}.pl* nakierowane na adres na adres IPv4 oraz IPv6 serwera VPS ze strefą zapasową, na potrzeby usługi poczty elektronicznej (czyli zakładając, że w
    późniejszym czasie zostanie tam też uruchomiony serwer pocztowy)
    - *gliwice.{wybierz_co_chcesz}.pl* oraz *www.gliwice.{wybierz_co_chcesz}.pl* nakierowane na adres IPv4 oraz IPv6 serwera VPS ze strefą podstawową dla domeny *gliwice.{wybierz_co_chcesz}.pl*

---

Dodane rekordy w pliku strefy na serwerze podstawowym:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2011.png)

---

- dokonać takiej konfiguracji na serwerze utrzymującym podstawową strefę domeny *gliwice.{wybierz_co_chcesz}.pl*, aby własny komputer w domu korzystał z osobnej konfiguracji strefy dla tej w/w domeny (np. nazwa *gliwice.{wybierz_co_chcesz}.pl* oraz *www.gliwice {wybierz_co_chcesz}.pl* mają być nakierowane na adres IP: 1.1.1.1) niż wszystkie pozostałe komputery, i nie były dla niego również obsługiwane zapytania rekursywne.

---

Konfiguracja widoków na serwerze poddomeny:

![Bez tytułu.png](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Bez_tytuu%201.png)

Przykładowa podstawowa konfiguracja pliku strefy w widoku “jo”:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2012.png)

---

## Testy

### Połączenie z pawlaczkowo.pl

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2013.png)

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2014.png)

### Połączenie z gliwice.pawlaczkowo.pl

Zapytanie o domenę z innych serwerów:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2015.png)

Zapytanie wprost serwera DNS3:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2016.png)

Zapytanie “digiem”:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2017.png)

### Połączenie z poddomenami

- sklep

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2018.png)

Na zdjęciu widać sposób działania pseudo-loadbalancingu.

- mail

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2019.png)

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2020.png)

### Połączenie do poszczególnych serwerów

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2021.png)

## Pełna konfiguracja serwerów

Adresy IP serwerów:

DNS1 — 64.226.126.208, 2a03:b0c0:3:d0::920:6001

DNS2 — 46.101.153.232, 2a03:b0c0:3:d0::f11:f001

DNS3 — 167.99.254.113, 2a03:b0c0:3:d0::ff2:5001

---

### DNS1

- db.pawlaczkowo.pl

```
D$TTL   7200
@       IN      SOA     dns1.pawlaczkowo.pl. michpawlak.proton.me. (
                             30         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      dns1.pawlaczkowo.pl.
@       IN      NS      dns2.pawlaczkowo.pl.
gliwice.pawlaczkowo.pl. IN      NS      dns3.gliwice

@       IN      A       64.226.126.208
@       IN      AAAA    2a03:b0c0:3:d0::920:6001
dns1    IN      A       64.226.126.208
dns1    IN      AAAA    2a03:b0c0:3:d0::920:6001

dns2    IN      A       46.101.153.232
dns2    IN      AAAA    2a03:b0c0:3:d0::f11:f001

www     IN      A       64.226.126.208
www     IN      AAAA    2a03:b0c0:3:d0::920:6001

sklep   IN      A       64.226.126.208
sklep   IN      AAAA    2a03:b0c0:3:d0::920:6001
sklep   IN      A       46.101.153.232
sklep   IN      AAAA    2a03:b0c0:3:d0::f11:f001

@       IN      MX      10      mail.pawlaczkowo.pl.
mail    IN      A       46.101.153.232
mail    IN      AAAA    2a03:b0c0:3:d0::f11:f001

dns3.gliwice     IN      A       167.99.254.113
dns3.gliwice     IN      AAAA    2a03:b0c0:3:d0::ff2:5001
www.gliwice     IN      CNAME   dns3.gliwice
```

- Dodane strefy w named.conf.default-zones:

```
zone "pawlaczkowo.pl" {
        type master;
        file "/etc/bind/db.pawlaczkowo.pl";
        notify yes;
        allow-transfer{46.101.153.232;};
        allow-update{none;};
};

zone "pawlak.pl" {
        type forward;
        forwarders {167.99.254.113;};
};
```

- Dodanie zezwolenia na zapytania w named.conf.options:

```
options {
	/.../
	allow-query { any; };
};
```

### DNS2

- Dodane strefy w named.conf.default-zones:

```
zone "pawlaczkowo.pl" {
        type slave;
        masters {64.226.126.208;};
        file "/var/cache/bind/db.pawlaczkowo.pl";
};
```

- zezwolenie na zapytania rekurencyjne tylko dla jednego komputera w named.conf.options:

```
acl trusted {XXX.XXX.XXX.XXX;};
options {
	/.../
	allow-recursion {trusted;};
};
```

### DNS3

- db.gliwice.pawlaczkowo.pl

```
$TTL    7200
@       IN      SOA     dns3.gliwice.pawlaczkowo.pl. michpawlak.proton.me. (
                              12                ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      dns3.gliwice.pawlaczkowo.pl.
@       IN      A       167.99.254.113
@       IN      AAAA    2a03:b0c0:3:d0::ff2:5001
dns3    IN      A       167.99.254.113
dns3    IN      AAAA    2a03:b0c0:3:d0::ff2:5001
www     IN      A       167.99.254.113
www     IN      AAAA    2a03:b0c0:3:d0::ff2:5001
```

- external.gliwice.pawlaczkowo.pl

```
$TTL    7200
@       IN      SOA     gliwice.pawlaczkowo.pl. michpawlak.proton.me. (
                              12                ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      dns3.gliwice.pawlaczkowo.pl.
@       IN      A       188.165.16.245
dns3    IN      A       188.165.16.245
```

- db.pawlak.pl

```
$TTL    7200
@       IN      SOA     pawlak.pl. bajojajo.pawlak.pl. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pawlak.pl.
@       IN      A       167.99.254.113
@       IN      AAAA    2a03:b0c0:3:d0::ff2:5001
```

- Dodane strefy w named.conf.default-zones:

```
view "jo" {
        match-clients {XXX.XXX.XXX.XXX;};
        allow-recursion {none;};
zone "gliwice.pawlaczkowo.pl" {
        type master;
        file "/etc/bind/external.gliwice.pawlaczkowo.pl";
};
view "wszyscy" {
        match-clients {any;};
/.../
zone "gliwice.pawlaczkowo.pl" {
        type master;
        file "/etc/bind/db.gliwice.pawlaczkowo.pl";
        allow-update{none;};
        allow-transfer {none;};
};

zone "pawlak.pl" {
        type master;
        file "/etc/bind/db.pawlak.pl";
        allow-update{none;};
        allow-transfer {none;};
};
};

```

- zezwolenie na zapytania rekurencyjne tylko dla jednego komputera w named.conf.options:

```
acl trusted {XXX.XXX.XXX.XXX;};
options {
	/.../
	allow-recursion {trusted;};
};
```

# DNSsec

## Założenia implementacji

Dla skonfigurowanej w w/w zadaniu domeny zarejestrowanej w publicznym systemie DNS, należy skonfigurować pełną obsługę DNSSec, ze zgłoszonym do operatora rekordem DS, i z wdrożoną automatyczną rotacją kluczy ZSK - obsługa DNSSec ma zostać zrealizowana zarówno dla domeny głównej *{wybierz_co_chcesz}.pl* jak również delegowanej domeny *gliwice.{wybierz_co_chcesz}.pl*

---

## Wdrożenie DNSsec na domenie podstawowej

- Przeniesienie plików strefy do /var/cache/bind/pawlaczkowo i zmiany w named.conf
    
    Aby uprościć działanie na strefie powinno przenieść się jej pliki do folderu /var/cache/bind i nadać plikom odpowiednie uprawnienia.
    
    - zmiana w named.conf.default-zones
    
    ![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2022.png)
    
    - zmiana w named.conf.options
        
        ![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2023.png)
        
- utworzenie kluczy
    
    ![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2024.png)
    
    Wybrałem taki a nie inny algorytm, ponieważ mój rejestrator podał w zalecanej konfiguracji ten algorytm:
    
    ![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2025.png)
    
- nadanie upranień dla grupy bind

Wykorzystałem komendy:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2026.png)

Ponieważ samo nadanie uprawnień odczytu i zapisu dla grupy bind może nie wystarczyć. Konfigurując uprawnienia najlepiej sprawdzać logi (korzystając z komendy, np.: journalctl -xe).

- automatyczna rotacja kluczy ZSK

Pierwszy klucz ZSK:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2027.png)

Ustawienie dat ważności klucza i jego usunięcia, a także wygenerowanie dwóch następców tegoż klucza:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2028.png)

Oczywiście można ich wygenerować znacznie więcej, jednakże na potrzeby tego projektu powinna to być wystarczająca ilość. Niezależnie od tego, czy utworzy się ich dużo, czy mało trzeba pamiętać do kiedy jest ostatni z kluczy, aby za wczasu utworzyć nowe jeśli będzie taka potrzeba.

Uwzględnienie tych kluczy w pliku strefy:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2029.png)

- przekazanie rekordu DS do rejestratora:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2030.png)

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2031.png)

Jeśli tak jak na zdjęciu wyżej, popełni się błąd przy wklejaniu danych do panelu webowego rejestratora, nie trzeba się tym martwić. Można poprawić błąd, jednakże przez jakiś czas będą widniały oba rekordy DS.  

Po naprawieniu błedu przez zmienienie “Digest Type” na 2:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2032.png)

Sprawdzając następnego dnia, widoczny był już tylko jeden, prawidłowy, rekord ds. (Zdjęcie w sekcji “Testy”)

## DNSsec dla poddomeny

Konfiguracja przebiega podobnie jak na podstawowej domenie.

- Przeniesienie plików strefy do /var/cache/bind/gliwice.pawlaczkowo
- Zmiany w named.conf
- Utworzenie kluczy
- W tym wypadku utworzyłem tylko jednego następcę klucza ZSK

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2033.png)

- Uwzględnienie kluczy w pliku strefy (strefa external.gliwice.pawlaczkowo.pl, bez dnsseca bo jest to konfiguracja testowa)

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2034.png)

- Nadanie odpowiednich uprawnień dla plików w /var/cache/bind

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2035.png)

- Przekazanie rekordu DS do serwera strefy podstawowej

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2036.png)

## Testy

Sprawdzenie konfiguracji przez dnsviz.pl

- pawlaczkowo.pl

![pawlaczkowo.pl-2023-06-20-08_40_46-UTC.png](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/pawlaczkowo.pl-2023-06-20-08_40_46-UTC.png)

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2037.png)

Niestety mój rejestrator domeny nie przewidział utworzenia w panelu webowym glue rekordów dla ipv6, więc wprost nic nie można z tym zrobić.  

Podgląd panelu do rejestracji serwera:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2038.png)

---

- gliwice.pawlaczkowo.pl

![gliwice.pawlaczkowo.pl-2023-06-20-19 24 46-UTC.png](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/gliwice.pawlaczkowo.pl-2023-06-20-19_24_46-UTC.png)

## Lista zmian względem poprzedniego zadania

### Serwer DNS1

Pliki strefy przeniesione do /var/cache/bind/pawlaczkowo.

Utworzenie kluczy KSK i ZSK oraz ustawienie automatycznej wymiany.

Zmiana uprawnień w katalogu:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2039.png)

- db.pawlaczkowo.pl

```
$TTL    7200
$INCLUDE /var/cache/bind/pawlaczkowo/Kpawlaczkowo.pl.+013+43010.key
$INCLUDE /var/cache/bind/pawlaczkowo/Kpawlaczkowo.pl.+013+59336.key
$INCLUDE /var/cache/bind/pawlaczkowo/Kpawlaczkowo.pl.+013+21196.key
$INCLUDE /var/cache/bind/pawlaczkowo/Kpawlaczkowo.pl.+013+22461.key
@       IN      SOA     dns1.pawlaczkowo.pl. michpawlak.proton.me. (
/.../)
/.../
gliwice.pawlaczkowo.pl. IN      DS      26736   13      2       1B7EC751BCA4346A506F22192060439043004BE7CAD116C1CC3ABA10FF348002
/.../
```

- named.conf.default-zones:

```
zone "pawlaczkowo.pl" {
<------>type master;
<------>file "/var/cache/bind/pawlaczkowo/db.pawlaczkowo.pl";
<------>notify yes;
<------>allow-transfer{127.0.0.1; 46.101.153.232;};
<------>allow-update{none;};
<------>auto-dnssec maintain;
<------>inline-signing yes;
<------>key-directory "/var/cache/bind/pawlaczkowo/";
};
```

zezwolenie na transfer do 127.0.0.1 utworzone na cele diagnostyczne.

- named.conf.options:

```
options {
/.../
<------>dnssec-enable yes;
<------>dnssec-validation auto;
<------>key-directory "/var/cache/bind/pawlaczkowo";
/.../
};
```

### Serwer DNS3

Pliki strefy przeniesione do /var/cache/bind/gliwice.pawlaczkowo.

Utworzenie kluczy KSK i ZSK oraz ustawienie automatycznej wymiany.

Zmiana uprawnień w katalogu:

![Untitled](Konfiguracja%20serwera%20DNS%20z%20implementacja%CC%A8%20DNSsec%20c29c42641e9c47938b606e3bc42f098a/Untitled%2040.png)

- db.gliwice.pawlaczkowo.pl

```
$TTL<-->7200
$INCLUDE /var/cache/bind/gliwice.pawlaczkowo/Kgliwice.pawlaczkowo.pl.+013+26736.key
$INCLUDE /var/cache/bind/gliwice.pawlaczkowo/Kgliwice.pawlaczkowo.pl.+013+56670.key
$INCLUDE /var/cache/bind/gliwice.pawlaczkowo/Kgliwice.pawlaczkowo.pl.+013+65423.key
/.../
```

- named.conf.default-zones

```
zone "gliwice.pawlaczkowo.pl" {
<------>type master;
<------>file "/var/cache/bind/gliwice.pawlaczkowo/db.gliwice.pawlaczkowo.pl";
<------>allow-update{none;};
<------>allow-transfer {none;};
<------>auto-dnssec maintain;
<------>inline-signing yes;
<------>key-directory "/var/cache/bind/gliwice.pawlaczkowo/";
};
```

- named.conf.options

```
options {
/.../
<------>dnssec-enable yes;
<------>dnssec-validation auto;
<------>key-directory "/var/cache/bind/pawlaczkowo";
/.../
};
```