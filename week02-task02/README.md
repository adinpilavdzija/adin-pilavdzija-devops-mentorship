# 02 Bash: OverTheWire - bandit-labs

## Task

Create new branch called: **`week-2-bandit-labs`** from your current **`main`** branch.

Add new dir called **`week-2`** inside that dir you should create file called **`bash-notes.md`**.

File **`bash-notes.md`** should contain **`minimum`** all commands executed durring this task. Recomended is to add your own comments about each executed **`command`**.

For example:  
**`$ cat <file> - #koristimo za ispis sadrzaja fajla`**

For actual task execution you need to do following:
- Access Wargames labs available on the following link: [https://overthewire.org/wargames/](https://overthewire.org/wargames/) and on level Bandit solve tasks up to level 10
- After completing each level take screenshoot of your screen as a confirmation of successfully completed level

Once when you complete required levels create Pull Request where you will inside PR description attach screenshots with confirmation of completed levels.  
Pull Request should contain **`bash-notes.md`** file with executed commands and screenshots.

[Here](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) you can find markdown language syntax and best practices:

**`NOTE`**: While doing the labs most likely elevating privileges to root or sudo will not be possible, so read the task and keep up with instructions.

Only members from Tier-1-Group-2 need to post PRs in comment for review. Check before you post PR, if your reviewer (mentor) has collaboration access to your repo.

---

## Level 0

The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

```bash
$ ssh bandit0@bandit.labs.overthewire.org -p 2220 #ssh pristup na portu 2220
```

## Level 1

The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

```bash
$ ls #prikaz sadrzaja trenutnog foldera
$ cat readme #prikaz sadrzaj fajla
$ exit #izlazi iz trenutnog shella
$ echo NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL > 1 #preusmjerava string/password u fajl 1
$ sudo apt install sshpass #instalacija sshpass
$ sshpass -p $(cat 1) ssh bandit1@bandit.labs.overthewire.org -p 2220 #login koristeci sshpass i password iz fajla 1
```

## Level 2

The password for the next level is stored in a file called - located in the home directory

```bash
ls
cat ./- #prikazuje fajl '-' specificirajuci da je to fajl u trenutnom folderu, da ne ocekuje neku opciju/parametar
exit
ssh bandit3@bandit.labs.overthewire.org -p 2220 #ssh login
```

## Level 3

The password for the next level is stored in a file called spaces in this filename located in the home directory

```bash
ls
cat "spaces in this filename" #jedan od nacina da se zaobidju spaceovi u nazivu buduci da bi se bez navodnika ovo posmatralo kao 4 odvojena fajla
exit
echo aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG > 3
sshpass -p $(cat 3) ssh bandit3@bandit.labs.overthewire.org -p 2220
```

## Level 4

The password for the next level is stored in a hidden file in the inhere directory.

```bash
ls
ls -la #prikaz svega, ukljucujuci hidden fajlove, u long listing formatu
cat .hidden
exit
echo 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe > 4
sshpass -p $(cat 4) ssh bandit4@bandit.labs.overthewire.org -p 2220
```

## Level 5

The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

```bash
ls
cd inhere #promjena pozicije na folder inhere
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
```

## Level 6

The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
- human-readable
- 1033 bytes in size
- not executable

```bash
$ cd inhere
$ ls -Rl #ispis u long formatu svih fajlova iz svih foldera i podfoldera
$ find . -readable -size 1033c -not -executable #pretraga svih fajlova koji su readable, velicine 1033 bytea i nisu executable
$ cat ./maybehere07/.file2
$ exit
$ P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
$ echo P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU > 6
$ sshpass -p $(cat 6) ssh bandit6@bandit.labs.overthewire.org -p 2220
```

## Level 7

The password for the next level is stored somewhere on the server and has all of the following properties:
- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

```bash
$ find / -size 33c -user bandit7 -group bandit6 2>/dev/null #pretraga fajla velicine 33 bytea, user je bandit7, a grupa bandit6. Pretragu pocinje od root foldera, a error poruke preusmjerava u /dev/null
$ cat /var/lib/dpkg/info/bandit7.password
$ exit
$ echo z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S > 7
$ sshpass -p $(cat 7) ssh bandit7@bandit.labs.overthewire.org -p 2220
```

## Level 8

The password for the next level is stored in the file data.txt next to the word millionth

```bash
$ ls
$ cat data.txt | grep "millionth" #u sadrzaju navedenog fajla pronalazi rijeci millionth uz koju stoji password
$ exit
$ echo TESKZC0XvTetK0S9xNwm25STk5iWrBvP > 8
$ sshpass -p $(cat 8) ssh bandit8@bandit.labs.overthewire.org -p 2220
```

## Level 9

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

```bash
$ ls
$ cat data.txt | sort | uniq -c #prikaz sortiranpg sadrzaja fajla i broj ponavljanja za svaku liniju
$ exit
$ echo EN632PlfYiZbn3PhVK3XOGSlNInNE00t > 9
$ sshpass -p $(cat 9) ssh bandit9@bandit.labs.overthewire.org -p 2220
```

## Level 10

The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

```bash
$ ls
$ strings data.txt | grep = #pretraga printable stringova koji sadrze '='
$ exit
$ echo G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s > 10
$ sshpass -p $(cat 10) ssh bandit10@bandit.labs.overthewire.org -p 2220
```

## Level 11

The password for the next level is stored in the file data.txt, which contains base64 encoded data

```bash
$ ls
$ cat data.txt
$ cat data.txt | base64 -d
$ echo 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM > 11
$ sshpass -p $(cat 11) ssh bandit11@bandit.labs.overthewire.org -p 2220
```