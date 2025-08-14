#Serwer SSH

**Serwer SSH(Secure Shell)** to usługa, która umożliwia zdalne połączenie terminalowe z komputerami. Serwer SSH można też określić jako proces
systemowy, który cały czas nasłuchuje na porcie 22, który jest domyślny dla SSH. Ściślej: **sshd(Secure Shell Daemon)** to proces. 
Istnieje jeszcze plik **sshd_config**. Jego lokalizacja to katalog konfiguracyjny Linuxa /etc/ssh. To plik konfiguracyjny serwera SSH. Domyślnie
ma ustawiony **Port 22** przez, który można połączyć termminale dwóch różnych systemów operacyjnych na komputerze. Np. Fedorę zainstalowaną jako
maszyna wirtualna w VirtualBox z terminalem systemu Windows na dysku twardym.

##Połączenie Fedory z VirtualBox z terminalem systemu Windows

-przy wyłączonej maszynie wirtualnej, w VirtualBox w ustawieniach *Sieć* w polu > Podłączona do
 zmieniamy wartość z NAT na > Mostkowana karta sieciowa(bridged).
-w polu > Nazwa
zaznaczamy sieć Wifi(u mnie Realtec xxx Wifi xxx). To spowoduje, że Fedora zainstalowana jako maszyna wirtualna w VirtualBox jest teraz jakby kolejnym
urządzeniem podpiętym pod sieć Wifi.
Zatem Fedora musi mieć własny adres IP. 
-ponownie uruchamiamy maszynę wirtualną(Fedorę) i w terminalu wpisujemy ip a aby poznać adres IP naszej Fedory. Wówczas poszukujemy wpisu > enp0s3 
a za pod nim interesuje nas > inet
Adres IP naszej Fedory to numer za > inet
np.:> 198.xxx.x.xxx bez ukośnika i liczby za nim.
-teraz należy połączyć się przez serwer SSH z systemem Windows
-na Windows otwieramy terminal PowerShell albo Wiersz poleceń(polecenie cmd)
-teraz należy wpisać polecenie ssh oraz nazwę użytkownika ze swojej Fedory oraz jej adres IP w formacie:
ssh nazwaużytkownika@IP Fedory
-przy pierwszym połączeniu należy zaakceptować klucz wpisyjąc > yes
-następnie należy podać hasło jakim logujemy się do Fedory
-wówczas w terminalu Windowsa wyświetli się nam ustawiony znak zachęty w Fedorze. 
Wówczas z poziomu terminala Windows można pisać polecenia linuxowe.
###I teraz jak to działa?

-proces ssh (SSH Daemon) nasłuchujący na domyślnym porcie linuxowym o numerze 22 to proces, który obsługuje połączenia przychodzące
-dzięki włączeniu **trybu mostkowanego** w VirtualBox Fedora jest widoczna przez sieć Wifi tak samo jak inne urządzenia podłączone do niej(do sieci).
Np. jak laptop jest podłączony do sieci. Wówczas Windows łączy się z Fedorą pod adresie IP. Klient serwera SSH w Windowsie przesyła komendy do Fedory
przez sieć. Można wówczas używać terminala Fedory z poziomu Wiersza poleceń Windows. Takie połączenie Windowsa i Fedory może służyć np. przesyłaniu plików
stworzonych wyłącznie w maszynie wirtualnej na dysk twardy Windowsa. W moim wypadku zrezygnowałem z folderów z Windowsa udostępnianych wirtualnej Fedorze.
Zatem jeśli stworzę w takim systemie Linux jakiś plik to istnieje on wyłącznie na dysku wirtualnym Fedory. Nie ma go na dysku twardym mojego komputera,
gdzie zainstalowany jest system Windows.

Przed konfiguracją serwera SSH należy sprawdzić czy Fedora ma z nim połączenie. Wówczas należy użyć polecenia 'sudo systemctl status sshd'
Jeśli w linijce > Active: active(running)
to znaczy, że istnieje połączenie z serwerem SSH.
Jeśli nie(enabled itp.) to wówczas należy uruchomić serwer SSH ręcznie poleceniem 'sudo systemctl start sshd'
Następnie jeśli chcemy aby serwer SSH startował automatycznie z każdym uruchomieniem Fedory to musimy użyć polecenia 'sudo systemctl enable sshd'

Teraz np. z innego laptopa podłączonego do tej samej sieci na którym nie ma zainstalowanej Fedory możemy połączyć się z wirtualną Fedorą na innym komputerze
podłączonym do tej samej sieci.
