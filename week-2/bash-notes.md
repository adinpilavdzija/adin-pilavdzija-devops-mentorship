#Bash-notes

##0-1
ssh bandit0@bandit.labs.overthewire.org -p 2220 *#ssh pristup na portu 2220*
ls *#prikaze sadrzaj foldera u kojem se nalazimo*
cat readme *#prikaze sadrzaj fajla*
exit *#izlazi iz trenutnog shella*
echo NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL > 1 *#sprema kopirani password u fajl 1*
sudo apt install sshpass *#instalira sshpass*
sshpass -p $(cat 1) ssh bandit1@bandit.labs.overthewire.org -p 2220 *#login koristeci sshpass i password iz fajla 1*

##1-2
ls
cat ./- #prikazuje fajl '-' specificirajuci da je to fajl u trenutnom folderu, da ne ocekuje neku opciju/parametar
exit
ssh bandit3@bandit.labs.overthewire.org -p 2220 #ssh login

##2-3
ls
cat "spaces in this filename" #jedan od nacina da se zaobidju spaceovi u nazivu buduci da bi se bez navodnika ovo posmatralo kao 4 odvojena fajla
exit
echo aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG > 3
sshpass -p $(cat 3) ssh bandit3@bandit.labs.overthewire.org -p 2220

##3-4
ls
ls -la #prikaz svega, ukljucujuci hidden fajlove, u long listing formatu
cat .hidden
exit
echo 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe > 4
sshpass -p $(cat 4) ssh bandit4@bandit.labs.overthewire.org -p 2220

##4-5
ls
cd inhere #ulazi u folder inhere
ls -la
file ./-file01 #vraca podatak o kojem se tipu fajla radi
file ./-file02
file ./-file03
file ./-file04
file ./-file05
file ./-file06
file ./-file07
file ./-file08
file ./-file09
cat ./-file07
exit
echo lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR > 5
sshpass -p $(cat 5) ssh bandit5@bandit.labs.overthewire.org -p 2220

##5-6
cd inhere
ls -Rl #ispis u long formatu svih fajlova iz svih foldera i podfoldera
find . -readable -size 1033c -not -executable #pronalazi sve fajlove koji su readable, velicine 1033 bytea i nisu executable
cat ./maybehere07/.file2
exit
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
echo P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU > 6
sshpass -p $(cat 6) ssh bandit6@bandit.labs.overthewire.org -p 2220

##6-7
find / -size 33c -user bandit7 -group bandit6 2>/dev/null #pronalazi fajl ako je velicine 33 bytea, user je bandit7, a grupa bandit6. Pretragu pocinje od root foldera, a error poruke preusmjerava u /dev/null
cat /var/lib/dpkg/info/bandit7.password
exit
echo z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S > 7
sshpass -p $(cat 7) ssh bandit7@bandit.labs.overthewire.org -p 2220

##7-8
ls
cat data.txt | grep "millionth" #u sadrzaju navedenog fajla pronalazi rijeci millionth uz koju stoji password
exit
echo TESKZC0XvTetK0S9xNwm25STk5iWrBvP > 8
sshpass -p $(cat 8) ssh bandit8@bandit.labs.overthewire.org -p 2220

##8-9
ls
cat data.txt | sort | uniq -c #prikazuje sortiran sadrzaj fajla i broj ponavljanja za svaku liniju
exit
echo EN632PlfYiZbn3PhVK3XOGSlNInNE00t > 9
sshpass -p $(cat 9) ssh bandit9@bandit.labs.overthewire.org -p 2220

##9-10
ls
strings data.txt | grep = #trazi printable stringove koji sadrze '='
exit
echo G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s > 10
sshpass -p $(cat 10) ssh bandit10@bandit.labs.overthewire.org -p 2220

##10-11
ls
cat data.txt
cat data.txt | base64 -d
echo 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM > 11
sshpass -p $(cat 11) ssh bandit11@bandit.labs.overthewire.org -p 2220
