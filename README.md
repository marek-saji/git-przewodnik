![](http://git-scm.com/images/logos/downloads/Git-Logo-2Color.png)
# Początek podróży

## Gettin' git

### Linux

Zainstaluj paczkę `git`.

###### Resources

* [Instructions](http://git-scm.com/download/linux) on git-scm.com

### Windows

http://git-scm.com/download/win

Paczka msysgit zawiera także bash-a z podstawowymi poleceniami — w ten sposób znacznie łatwiej korzystać z gita i dodatkowych skryptów. 

###### Resouces

* [Downloads](http://git-scm.com/downloads) page on git-scm.com
* [windows-owy klient GitHub-a](http://windows.github.com/) można używać nie tylko do repozytoriów tam hostowanych

#### Polecenia GNU dla Windows

Gdy brakuje Ci jakiegoś polecenia w bashu, np. do jakiegoś hooka, dobre kompilacje znajdziesz na http://gnuwin32.sourceforge.net/packages.html

##### `grep`

`grep` zawarty w msysgit nie obsługuje opcji `-P`/`--perl-regexp` (włączenie perl-owych, bardziej zaawansowanych wyrażeń regularnych).

Sprawdź, czy twój `grep` ma tą opcję:
```sh
$ grep --help | grep -- --perl-regexp
```

Jeśli jej brakuje, pobierz kompilację grepa z http://gnuwin32.sourceforge.net/packages/grep.htm i nadpisz plik `grep.exe` w katalogu `bin/`.

###### Resources

* `man [grep](http://linux.die.net/man/1/grep)`

#### Ładniejszy terminal

##### ConEmu

https://code.google.com/p/conemu-maximus5/

![ConEmu obsługuje m.i. kolory i przezroczyste tła](http://conemu-maximus5.googlecode.com/svn/files/ConEmuTransparency2.png)

## Wspólne ustawienia dla wszystkich repozytoriów

### Edytor

Edytor używany jest w gicie m.i. do wpisywania komentarzy do komitów.

Wybrany edytor musi działać w następujący sposób:

* `git` odpala edytor i czeka, aż polecenie skończy się wykonywać,
* zamykacie edytor i "zwracacie kontrolę" `git`-owi.

Także IDE odpadają. [nano](http://linux.die.net/man/1/nano), czy [pico](http://linux.die.net/man/1/pico) dają radę. Można też użyć [SublimeText dodając parametr `-w`](http://stackoverflow.com/a/9408117.)

Zasadniczo dowolny edytor, którego da się przekonać do poprawnego kodowania i znaków końca linii. Ustawienia dokomujemy w sposób nastepujący:

```sh
$ git config --global core.editor "editor command" 
```

###### Resources

* `man [git-config](http://git-scm.com/docs/git-config)`

### Nazwa aktywnego brancha/taga w linii poleceń

```sh
$ echo >> ~/.bashrc
$ wget -qO- https://gist.github.com/marek-saji/5090416/raw/.bashrc >> ~/.bashrc
# jeśli nie ma polecenia "wget":
$ curl -s https://gist.github.com/marek-saji/5090416/raw/.bashrc >> ~/.bashrc
```

Po ponownym włączeniu konsoli, przed znakiem zachęty pojawi się nazwa aktualnego tag-a (jeśli nie istnieje to branch-a) w nawiasach.
Jeśli komuś nie podoba się kolor, można skorzystać z tego skryptu, żeby wyświetlić listę dostępnych kolorów: https://gist.github.com/marek-saji/5090435

###### Resources

* `man [bash](http://linux.die.net/man/1/bash) | less -p^PROMPTING`
* [modularny PS1](https://github.com/marek-saji/PS1_modules), zawiera moduły do git-a

### Poprawne diff-owanie plików PHP

Wyświetlając diff-y git wyświetla kontekst, w którym zaszła zmiana. Jeśli zamiast funkcji/metody wyświetla się klasa, można to naprawić:

```sh
echo '*.php diff=php' >> ~/.gitattributes
git config --global core.attributesfile "~/.gitattributes"
```

###### Resources

* [pytanie na StackOverflow, z którego pochodzi rozwiązanie](http://stackoverflow.com/a/7347993)

## Zdalne repozytorium

Do repozytorium git można odwoływać się przez wiele protokołów:

* `ssh://` — dostęp przez ssh
* `http://` i `https://` — dostęp via HTTP
* `file://` — dostęp przez lokalną ścieżkę do repozytorium. Można pominąć `file://` i po prostu podać ścieżkę
* `git://` — git-owy demon działający na dedykowanym porcie

Naturalnie ze względów bezpieczeństwa najlepiej korzystać przez `ssh`. Żeby za każdym razem nie trzeba było wpisywać hasła, zwykle ustawia się klucze publiczne.

### Klucz publiczny SSH

Generowanie klucza:

```sh
# wchodzimy do katalogu konfiguracji ssh, może już istnieć.
$ mkdir ~/.ssh
$ cd ~/.ssh
# jeśli klucz istnieje, jesteśmy szczęśliwi:
$ ls id_rsa.pub
# jeśli nie, należy go wygenerować (bez hasła):
$ ssh-keygen -t rsa -C "twój.email`example.com"
```

Tak wygenerowany klucz wklejamy w ustawieniach w GitHubie, Bitbucket’cie, bądź przekazujemy komuś, kto ma moce to ustawić.

###### Resources

* github:help — [Generating SSH Keys](https://help.github.com/articles/generating-ssh-keys)
* `man [ssh-keygen](http://linux.die.net/man/1/ssh-keygen)`

### Klonowanie

```sh
$ cd ~/SOMEWHERE/crls/
$ git clone ssh://git@git.holonglobe.com/szp src/
# wejdźmy do katalogu z repozytorium
$ cd src/
```

BAM! (W końcu) Masz repozytorium. Nie przestawaj czytać. {;

###### Resources

* `man [git-clone](http://git-scm.com/docs/git-clone)`

### Klonowanie submodułów

Możliwe, że sklonowane repozytorium zawiera submoduły. Jest to nic innego, jak inne repozytoria git "osadzone" w tym repozytorium. Polecenie `git clone` nie pobiera ich automatycznie, należy zrobić to oddzielnie:

```sh
$ git submodule init
```

###### Resources

* `man [git-submodule](http://git-scm.com/docs/git-submodule)`

### Podstawowe ustawienia

Git pozwala wam ustawić jak chcecie się przedstawić w swoich commitach:

```sh
$ git config --add user.name "Imię Nazwisko"
$ git config --add user.email "e.mail@example.com"
```

Dodanie parametru `--global` spowoduje zapisanie tych ustawień w pliku globalnej konfiguracji (`~/.gitconfig`), zamiast wewnątrz repozytorium (`.git/config`).

###### Resources

* `man [git-config](http://git-scm.com/docs/git-config)`

#### Wspólne ustawienia dla projektu

Ustawienia gita nie są wysyłane na serwer. Jednak jeśli jest taka potrzeba (dzielenie się aliasami, o których za chwilę, wewnątrz zespołu jest sympatyczną praktyką), można to zrobić. Na repozytorium wrzucamy plik ze spólnymi ustawieniami, np. do `.gitconfig`. Następnie każdy, kto sklonował repozytorium musi dodać do swojego `.git/config`:

```ini
[include]
    path = ../.gitconfig
```

### Rozejrzenie się

Ok, _sklonowałeś_ repozytorium. Oznacza to, że jest na nim wszystko to, co znajduje się na upstream-owym serwerze. W razie wojny nuklearnej projekt jest bezpieczny.

Rozejrzyjmy się, co właściwie dostaliśmy.

```sh
$ git remote
origin
```

Istnieje jeden remote, domyślnie nazwany `origin`. To nasz upstream, z którego klonowaliśmy repozytorium i na który będziemy wysyłać rzeczy. Przyjrzyjmy się bliżej:

```sh
$ git remote -v
origin  git.holonglobe.com:szp (fetch)
origin  git.holonglobe.com:szp (push)
```

Fun fact: URL, z którego pobieramy zmiany w ramach danego remote-a wcale nie musi być taki sam, jak ten, na który wysyłamy rzeczy.

Patrzmy dalej.

```sh
$ git branch
* master
```

Znajdujemy się na jedynym _lokalnym_ branchu. `master` jest zwyczajową nazwą dla głównej, stabilnej gałęzi projektu. Wszelkie zmiany na nim powinny być wprowadzane ze szczególną ostrożnością.

```sh
$ git branch -vv
* master     5959cb9 [origin/master] Make two report generating scripts act more consistent.
```

Wygląda na to, że nasz lokalny branch `master` śledzi zdalny branch `master` (nazwy nie muszą wcale być takie same) z remote-a `origin`.

```sh
$ git branch -a
* master
remotes/origin/converter
remotes/origin/devel
remotes/origin/master
remotes/origin/next
remotes/origin/story-3319
```

Istnieje kilka zdalnych branch-y, z których dwa, które nas interesują, nie mają jeszcze lokalnych odpowiedników.

```sh
$ git checkout -b next --track origin/next
$ git checkout -b devel --track origin/devel
$ git branch
* devel
  master
  next
  story-3319
```

Jesteśmy teraz na branch-u `devel`. Tutaj głównie toczą się prace nad aktualnym sprintem. Jeśli historyjka, bądź kierat mają być długotrwałe, bądź prace nad nimi mogą przeszkadzać innym użytkownikom repozytorium, będziemy tworzyć branch dla tego issue-a (tutaj: `story-3319` dla zlecenia o numerze #3319).

###### Resources

* `man [git-remote](http://git-scm.com/docs/git-remote)`
* `man [git-branch](http://git-scm.com/docs/git-branch)`
* `man [git-checkout](http://git-scm.com/docs/git-checkout)`

### Aliasy

Aliasy to nic innego jak skróty do dłuższych, a często wykonywanych poleceń. Definiuje się je w pliku konfiguracyjnym. Jeśli alias _nie_ zaczyna się od wykrzyknika (`!`), to traktowany jest jako pod-polecenie `git`-a (np. alias `1log` ustawiony na `log --oneline --decorate` wykona `git log --oneline --decorate`); poprzedzenie aliasy wykrzyknikiem pozwala na wykonanie dowolnego polecenia (np. alias `test` ustawiony na `!echo test`). Jeśli polecenie jest długie, dobrym zwyczajem jest umieścić je w skrypcie w katalogu `.git/bin/` i w aliasie wywołać skrypt.

Wszystkie parametry podane przy wywołaniu aliasu są przekazywane, więc np. `1log` z powyższego przykładu możemy wywołać jako `git 1log --graph`. Możemy odwoływać się do konkretnych parametrów aliasu tak jak w bashu (`$1`, `$2`…); jeśli chcemy, żeby parametry _nie_ zostały przekazane do zaliasowanego polecenia, można na jego końcu dodać znak komentarza (`#`), który wykomentuje dodatkowe argumenty. Tak więc alias `test2` ustawiony na `!echo $2 #` wyświetli tylko drugi parametr.

Podanie parametru `--help` przy wywołaniu aliasu wyświetli zaliasowane polecenie..

Trochę przydatnych aliasów (można dodać do globalnie, w `~/gitconfig`):

```ini
[alias]
    ; slim log
    lg  = log --graph --pretty=slim --abbrev-commit --date=relative
    ; lg date: sorted by date and with absolute dates
    lgd  = log --graph --pretty=slim --abbrev-commit --date=local --date-order
    ; lg upstream: also show upstream branch
    lgu  = !git lg  $( git rev-parse --symbolic @{u} ) HEAD
    ; lg date, upstream
    lgdu = !git lgd $( git rev-parse --symbolic @{u} ) HEAD
    ; lg me: my commits from all branches, by date
    lgme = !git lgd --author=\"$( git config --get user.name )\" --all

    ; grep with some context (and a header)
    hgrep = grep --heading -B3 -A3

    ; diff by word
    diffword = diff --word-diff --word-diff-regex='\\w+|[^[:space:]]'
    ; diff by character
    diffchar = diff --word-diff --word-diff-regex='.'

    ; checkout files with only whitespace changes
    whitespacecheckout = "!git status --porcelain | grep '^.M' | cut -b4- | while read file ; do if test -z "$( git diff -w "$file" )"; then git checkout -- "$file" ; fi; done"

    ; rebase not-yet-pushed commits
    rebaselocal = "!REMOTE=$( git rev-parse --abbrev-ref HEAD@{u} ) ; if [ -n "$REMOTE" ]; then git rebase -i $REMOTE ; else echo "ERROR: Unable to determine remote branch" ; fi"

[pretty]
    slim = "%C(red)%h%C(yellow)%d%C(reset) %s %C(green)(%cd) %C(bold blue)<%an>%C(reset)"
```

Sekcja `[pretty]` definiuje ładniejszy, jednolinijkowy format dla `git-log`.

###### Resources

* `man [git-config](http://git-scm.com/docs/git-config) | less -p'alias.*'`

### Hooki

Hooki to rzeczy, które uruchamiają się w reakcji na jakieś zdarzenie. Można ich użyć do sprawdzenia, czy komitowany przez nas kod jest dobrze sformatowany, nie ma w nim pozostałości z debug-owania etc.

Zestaw przydatnych hook-ów:

```sh
# wchodzimy do katalogu, w którym znajduą się lokalne hook-i
$ cd .git/hooks/
# usuwamy to, co tam teraz jest (przykładowe rzeczy)
$ rm *
# klonujemy gist-a hookami:
# (git-in-a-git, fancy that!)
$ git clone https://gist.github.com/0b3e22847a2d5b732373.git .
```

Po nazwach można się łatwo domyślić co robi każdy z nich:

* `common`: wspólne funkcje etc
* `pre-commit`, `post-checkout`: hooki uruchamiane przed komitowaniem (mogą powstrzymać commit) i po checkout-cie (np. zmiana brancha)
* `pre-commit-*`, `post-checkout-*`: sprawdzanie/robienie poszczególnych rzeczy w ramach tych hooków.

Jeśli nie chcemy używać jakiegoś hooka, można go jednorazowo wyłączyć:

```sh
# wyłączenie sprawdzania białych znaków
$ git -c hooks.whitespace=false commit
# wyłączenie hooków w ogóle
$ git -c hooks.all=false checkout
```

Można też wyłączyć hook na stałe, np. większość z was zechce wyłączyć `swap-ctags`:

```sh
$ git config --add hooks.swap-ctags false
```

#### Współdzielone hooki

Podobnie jak w przypadku konfiguracji miło jest dzielić się hookami z resztą zespołu. Można to zrobić trzymając je na w repozytorium, np. w `git-hooks` i podlinkować go do wewnątrz `.git`:

```sh
cd .git
ln -sfn ../git-hooks hooks
```

###### Resources

* `man [githooks](http://git-scm.com/docs/githooks)`

# Używanie

Zanim zaczniemy pracę ważne jest zrozumienie podstawowych konceptów związanych z `git`-em.

Kod (bądź ogólnie "treści") mogą znajdować się w jednej z następujących przestrzeni:

* *stash*
  Przestrzeń służąca do "odkładania" na bok rzeczy z katalogu roboczego.
* *workspace* / *working directory*
  Katalog roboczy — wszystko to, co leży poza katalogiem `.git`. Nad tym pracujemy.
* *index* / *staging area*
  Służy do komponowania komitów.
* *local repository*
  Nasze lokalne repozytorium. Póki nie wypchniemy z niego komitów, możemy je dowolnie zmieniać, usuwać, przestawiać. Tutaj istnieją również lokalne branch-e, które wcale nie muszą nigdy zostać wypchnięte nigdzie wyżej (mogą zostać usunięte, bądź zmerge-owane do innego branch-a, który zostanie wypchnięty).
* *upstream repository*
  Repozytorium, przez które zachodzi synchronizacja, między ludźmi pracującymi w projekcie.

###### Resources

* [Interactive Git Cheatsheet](http://ndpsoftware.com/git-cheatsheet.html)
  Dobrze pokazuje relacje między "przestrzeniami", a poszczególnymi poleceniami.

## `git checkout`: przełączanie branchy i nie tylko

Jak być może zauważyliście rozglądając się, repozytorium posiada wiele branch-y, jednak w katalogu roboczym widzimy tylko jeden z nich. Możemy przełączać branch, nad którym pracujemy:

```sh
$ git branch
* devel
  master
  next
  story-3319
# Przełączenie z devel na next
$ git checkout next
$ git branch
  devel
  master
* next
  story-3319
# Powrót do ostatniego branch-a -- analogicznie do `cd -`
$ git checkout -
* devel
  master
  next
  story-3319
```

Poleceniem można również "wgrać" stan z wybranego branch-a, bądź innego REF dla wybranych plików:

```sh
# Wycofanie lokalnych zmian
# (wgranie stanu z ostatniego commit-a)
$ git checkout .
# To samo, co powyżej: `HEAD` określa commit "na którym jesteśmy",
# natomiast `--` oddziela REF od 
$ git checkout HEAD -- .
# Możemy też użyć trybu interaktywnego, i wybrać, co chcemy
# wyrzucić po kawałku:
$ git checkout -p .
# Możemy też np. zcheckout-ować kawałek
# wybranego pliku z innego branch-a.
$ git checkout -p devel -- public/index.php
```

… acz do ostatniego prawdopodobnie lepiej użyć `git-cherry-pick` służący do przenoszenia pojedynczych commitów pomiędzy branchami.

###### Resources

* `man [git-checkout](http://git-scm.com/docs/git-checkout)`
* `man [git-cherry-pick](http://git-scm.com/docs/git-cherry-pick)`

## `git add`, `git commit`: komitowanie

Komitując zapisujemy zmiany w _lokalnym_ repozytorium.

```sh
# status plików w repo (zmodyfikowane, nie śledzone, usunięte, zmieniona nazwa)
$ git status
# dodanie plików
$ git add PLIK PLIK PLIK
# interaktywne dodawanie plików po kawałku
$ git add -p PLIK PLIK
# commit
$ git commit
# możemy też od razu podać commit msg:
$ git commit -m "Komentarz"
# obejrzenie ostatniego commit-a, czy wszystko jest w porządku
$ git show
```

Wywołanie `git commit` bez opcji `-m` spowoduje użycie ustawionego edytora

W odróżnieniu od subversion, skomitowanie nie równa się wysłaniu zmian na zdalny serwer. Patrz rozdział "Synchronizowanie z upstream-em".

###### Resources

* `man [git-status](http://git-scm.com/docs/git-status)`
* `man [git-add](http://git-scm.com/docs/git-add)`
* `man [git-commit](http://git-scm.com/docs/git-commit)`
* `man [git-show](http://git-scm.com/docs/git-show)`

### Treść commitów

Wpisując commit message należy przyjąć konwencję, że pierwsza linijka jest krótkim opisem (najlepiej do 70 znaków). Po niej może nastąpić pusta linijka przerwy i dłuższy opis (linie do 70 znaków).

Należy się tego trzymać, bo `git lg`, `git log --oneline`, czy `git merge --log` używają tylko tej pierwszej linijki.

#### Dobre praktyki odnośnie formułowania commit message-ów

* Odniesienia do issue-ów poprzedzamy słowami:
  * "fixes", jeśli commit naprawia błąd,
  * "closes", jeśli commit zamyka issue innego typu,
  * "refs", jeśli commit jest powiązany w inny sposób.
    np. `fixes #42, refs #13`
* Opisujemy, czym jest commit, a nie jaki był motyw jego wprowadzenia:
  * "Redirect to main page, when removing status. fixes #42",
  * a *nie*: "Fix weird bug",
  * czy nawet *nie*: "fixes #42: Weird thing happens, when removing status",
  * i brońboże *nie* samo: "fixes #42".
* Używamy formy:
  * "Change property visiblity",
  * a *nie* "Changes property visibility".

Przykład:

> Make two report generating scripts more consistent. refs #4596, #5167
>
> - Generated files start with "report-" and contain date and
>   time of generation.
> - Both script and generated files names start with "report-".
> - Make passing year to report-error.log.sh optional.

### Poprawianie ostatniego commita

Literówka w commit msg? Zapomniany plik? W łatwy sposób można dodać coś do ostatniego commita:

```sh
# jeśli zapomnieliśmy jakiegoś pliku dodajemy go do staging area
$ git add PLIK
$ git commit --amend
# uruchomi się nam edytor pozwalający na zmianę commit msg-a.
```

_Nie_ należy tego robić, jeśli już spushowaliśmy ostatni commit.

###### Resources

* `man [git-commit](http://git-scm.com/docs/git-commit)`

## Synchronizowanie z upstream-em

Nie podając nazwy remote-a, będziemy pracować na domyślnym (zwykle `origin`; zwykle używamy tylko jednego).

### `push`

Mając lokalne commity możemy wypchnąć je na serwer:

```sh
# spójrzmy, co chcemy wysłać (zamiast $BRANCH nazwa obecnego branch-a)
$ git lgu
# jeśli wygląda w porządku:
$ git push
```

###### Resources

* `man [git-push](http://git-scm.com/docs/git-push)`

### `pull`

Ściągnięcie zmian z upstream-u:

```sh
$ git pull
# Jeśli mieliśmy niespushowane commity, może pojawić się
# rozgałęzienie w kodzie i dodatkowy merge-ujacy commit;
$ git log
*   163ee1f Merge branch 'devel' of ssh://example.com/fooer into devel (Mon Mar 25 12:51:52 2013) <Gallus Anonymus>
|\  
| * 5660b07 Always cast report filters to array. refs #4399 (Mon Mar 25 12:37:57 2013) <Other Guy>
* | 0cd6b6c fix recursive directory removing, fixes #5418 (Mon Mar 25 12:51:32 2013) <Gallus Anonymus>
|/  
* adecdab Fix legacy invocation of getEntity(). refs #5131 (Mon Mar 25 11:25:36 2013) <Other Guy>
* 5b666c1 Properly filter RDashboard, when upgrading order_number. refs #5390 (Mon Mar 25 10:43:55 2013) <Other Guy>
…
# Jeśli chcemy tego uniknąć, możemy poprosić pull, żeby
# użył rebase-owania, zamiast merge-owania:
$ git pull --rebase
# można też zrobić po kolei
$ git fetch
$ git rebase # bądź `git merge`
```

`git pull --rebase` różni się pod kilkoma względami:

* niezpushowane commity zmienią swój hash (ponieważ zmieni im się rodzic)
* w przypadku konfliktów będziemy musieli je rozwiązywać dla każdego ze swoich commitów z osobna — tak, aby dały się zaaplikować na swoich nowych rodziców

###### Resources

* `man [git-pull](http://git-scm.com/docs/git-pull)`

## Praca z branch-ami

Tworzenie branch-a dla każdej niebanalnej zmiany jest dobrym pomysłem. Dzięki temu:

* nie blokujemy wyrzutów z branch-a:
  Zwłaszcza istotne na branch-ach powyzej deweloperskiego: tam najlepiej nawet drobne naprawy robić na osobnych branch-ach i oddawać je komuś do spojrzenia.
* wygodniej przeprowadzić code review:
  wystarczy przejrzeć commit-y z branch-a,
* nie musimy dodawać `refs #STORY_ISSUE` do każdego commit-a:
  wystarczy dodać do commit-a merge-ującego do głównej gałęzi,
* łatwiej dołączyć kogoś do realizacji historyjki:
  w odróżnieniu od grzebania w niej na nieskomitowanych plikach: "Chcesz pomóc? Ok.. tylko to ukryję to, skomituję, później będziesz musiał w konfigu sobie lokalnie....."
* łatwiej pracować nad kilkoma rzeczami równolegle:
  Niespushowane commity z historyjki nie przeszkadzają w niczym, żeby zabrać się za krytyczny błąd na głównym branchu,
* zen and stuff.

### Tworzenie lokalnego branch-a

```sh
$ git branch story-1337-kittens
$ git checkout story-1337-kittens
# bądź jedym poleceniem
$ git checkout -b story-1337-kittens
```

To utworzy branch na podstawie tego, na którym jesteśmy aktualnie zcheckoutowani.Możemy utworzyć branch z innego commit-a podając dodatkowy parametr:

```sh
git branch story/1337-kittens 57ed6b3
git branch story/1337-kittens next
git branch story/1337-kittens HEAD~
```

###### Resources

* `man [git-checkout](http://git-scm.com/docs/git-checkout)`
* `man [gitrevisions](http://git-scm.com/docs/gitrevisions)`

#### Tworzenie lokalnego branch-a z niespushowanych commitów

Pracowałeś nad czymś przez chwilę, skomitowałeś ze dwie rzeczy i doszedłeś do wniosku, że może powinno to się znaleźć na dedykowanym branchu. Jak to zrobić?

☠ Jeśli nie czujesz się pewnie, zbackupuj w tym momencie swoje repozytorium.

```sh
# tworzymy lokalny branch z aktualnego HEAD-a
$ git branch story-1337-kittens
# szukamy ostatniego commit-a, który nie dotyczył story/1337-kittens
$ git lg
# przesuwamy branch bazowego na ten commit:
$ git reset --hard $COMMIT
# zobacz, czy wszystko wygląda, jak należy
$ git lg
# przechodzimy na nowy branch i piszemy dalej / pushujemy
$ git checkout story-1337-kittens
```

### Tworzenie zdalnego branch-a

Kiedy chcemy opublikować pracę (z kimś współpracujemy, bądź chcemy oddać historyjkę do testów).

Najlepiej robić to będąc na lokalnym branch-u, którego nie ma jeszcze w upstream-ie. Próbujemy go zpushować, co się nie uda, ale git podpowie nam co pewnie chcemy zrobić:

```sh
(bug-1337) $ git push
fatal: The current branch fo has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin bug-1337

(bug-1337) $ git push --set-upstream origin bug-1337
```

### Merge-owanie branch-a

Kiedy nasz branch jest gotowy do połączenia z głównym, możemy go zmerge-ować i usunąć zarówno lokalny, jak i zdalny branch:

```sh
git checkout devel
git merge --no-ff --edit story/1337-kittens
git branch -d story/1337-kittens
git push origin :story/1337-kittens
```

Opcja `-d` nie pozowli na usunięcie niezmerge-owanego brancha — niezmerge-owane commity zostały by wtedy "oderwane od drzewa". Można to wymusić używając `-D`.

Opcje `--no-ff --edit` pozwoli na zmodyfikowanie commit message-a: można tutaj wpisać `refs #1337` tak, aby commit pojawił się na stronie issue-a.

Ostatnie polecenie wygląda dość dziwnie — od publikowania różni się tylko głupim dwukropkiem. Wynika to z tego, że pełna postać polecenia `git-push` to:

```sh
git push REMOTE LOCAL_BRANCH:REMOTE_BRANCH
```

… a usunięcie zdalnego branch-a, to tak jakby wypchnięcie w niego nicości.

###### Resources

* `man [git-branch](http://git-scm.com/docs/git-branch)`
* `man [git-push](http://git-scm.com/docs/git-push)`

## Praca z submodułami

Submoduły pozwalają na podpięcie upstream-owych projektów, bądź na rozbicie naszego własnego projektu na mniejsze, logiczne, reużywalne części. W odróżnieniu od svn-owych externals submoduły pozwalają na określenie rewizji, w której ma zostać użyty dany submoduł. Chroni nas to przed sytuacją, w której autorzy któregoś z submodułów zmienią API, co popsuje naszą aplikację. Zmiana rewizji submodułu jest zawsze akcją świadomą.

### Zainicjowanie submodułów

Polecenie wywołujemy zarówno po sklonowaniu repozytorium, jak i kiedy ktoś doda nowe submoduły (pojawiają się w komunikacie przy `git pull`).

```sh
# zarejestrowanie nowych submodułów w .git/config
$ git submodule init
# sklonowanie ich
$ git submodule update
```

### Dodanie submodułu

```sh
$ git submodule add git://github.com/HolonGlobe/HoloGram.git library/HolonGlobe
```

Tak dodany moduł zostanie dodany do staging area w oczekiwaniu na skomitowanie.

### Aktualizowanie submodułów

Jak było wspominane, submoduły "ustawione" są na konkretny commit. Co robimy kiedy pojawia się np. nowa wersja upstreamowej biblioteki?:

```sh
# wchodzimy do katalogu submodułu
$ cd library/HolonGlobe
# checkout-ujemy inną wersję
$ git checkout v1337
# wracamy do katalogu głównego repozytorium
$ cd -
# tutaj widzimy, że katalog z submodułem jest oznaczony jako zmieniony
$ git status
# więc możemy go skomitować
$ git add library/HolonGlobe
$ git commit
```

### Usuwanie submodułu

Tu sprawa jest mniej banalna. Patrząc na manual od `git-submodule` okazuje się, że nie ma tam operacji uzuwania. Trzeba ręcznie wytrzeć wszelkie ślady istnienia modułu:

1. usunąć sekcję z `.gitmodules`
2. usunąc sekcję z `.git/config`
3. usunąć katalog z git-a: `git rm --cached path_to_submodule` (no trailing slash).
4. commit
5. usunąć już nieśledzone pliki submodułu

###### Resources

* `man [git-submodule](http://git-scm.com/docs/git-submodule)`
* [Git Submodule Tutorial](https://git.wiki.kernel.org/index.php/GitSubmoduleTutorial) at official git's wiki
* [Git Tools — Submodules](http://git-scm.com/book/en/Git-Tools-Submodules) chapter in the Git Book

# Administrowanie

## gitosis

Do zarządzania repozytoriami można używać z gitosis. Konfiugorwanie polega na sklonowaniu `git@example.com:gitosis-admin` edycji plików konfiguracji i pushowaniu.

Na serwerze wszystko leży zwykle w `/home/git` (`~git`).

* `.gitosis.conf → ~git/repositories/gitosis-admin.git/gitosis.conf`
  Obowiązująca konfiguracja.
* `repositories`
  Założone repozytoria oraz specjalne `gitosis-admin.git`.
* `.ssh/authorized_keys`
  Klucze publiczne autoryzowane do dostępu.

Należy ograniczyć ręczną konfigurację, bo może zostać nadpisana przez zpush-owanie na `gitosis-admin`.

###### Resources

* [gitosis at GitHub](https://github.com/tv42/gitosis)
* [article at ArchLinux's wiki](https://wiki.archlinux.org/index.php/Gitosis)

### gitosis troubleshooting

*UWAGA* Wszystkie polecenia zakłądają, że działamy jako użytkownik `git`! Robienie jako `root` może łatwo popsuć uprawnienia i odciąć ludziom dostęp do repo (patrz pierwszy trouble).

```sh
$ ssh USER@example.com -t sudo -u git -i
```

#### że nie można cośtam, że dostęp do `objects`

Jeśli sklonujemy `gitosis-admin` nie przez ssh, tylko system plików i tak zpushujemy, to w repozytorium powstaną pliki z uprawnienami dla tego użytkownika. Jeśli był nim `root`, to psujemy uprawnienia dla wszystkich innych użytkowników. Wystarczy przywrócić uprawnienia do katalogu domowego użytkownika `git`:

```sh
$ sudo chown -R git:git ~git
```

#### push-uję i nic się nie zmienia

Możliwe, że link do hook-a jest znów popsuty (nowa wersja python-a?):

```sh
$ cd ~/repositories/gitosis-admin.git/hooks/
$ ls -l post-update
```

#### wcześniej było coś popsute, teraz pushuję i nie tworzy mi się repozytorium

Zdarzyło mi się, że chciałem założyć repozytorium, ale był popsuty hook. Naprawiłem go, ale nawet jak wycofałem commit dodający repo i znów go pchnąłem, repozytorium mi się nie utworzyło. Można to zrobić ręcznie:

```sh
$ cd ~/repositories/
$ git init --bare repo_name.git
```

#### Czy na pewno się zpushowało/usunęło?

Repozytoria w `~/repositories` są takie same jak te, na których pracujemy. Jedyną różnicą jest to, że posiadają tylko zawartość katalogu `.git`. Można na nich wykonywać wszystkie polecenia dodając opcję `--bare`:

```sh
$ cd ~/repositories/repo_name.git
$ git --bare log
# nie mamy aliasu `lg`, to jest jest podobne (bez dat)
$ git --bare log --graph --oneline --decorate --date --branches
# lista branchy z ostatnimi komitami
$ git --bare branch -v
# lista tagów
$ git --bare tag
```

Nie trzeba chyba dodawać, że komitowanie/pushowanie/pullowanie tutaj jest *bardzo* złym pomysłem.

## Migracja z `svn` do `git`

Dzięki `git-svn` migracja odbywa się bardzo gładko.

Zasadniczo wystarczy pobrać repozytorium via `git-svn` i zpushować je do nowoutworzonego repozytorium `git`.

```sh
# klonujemy repozytorium svn przez git-svn
$ git svn clone …
# zapisz ignorowane pliki w .gitignore
$ git svn show-ignore >> .gitignore
$ git add .gitignore
$ git commit -m 'Convert svn:ignore properties to .gitignore.'
# dobrze jest w tym momencie zrobić backup
$ cp -ra . ../svn-git-migration-backup
# porządkujemy repozytorium:
# - można usunąć niepotrzebne branche
# - poustawiać tagi
# - naprawić autorów komitów (o tym poniżej)
$ …
# dodajemy remote z pustym repozytorium git
$ git remote add git-migration git.holonglobecom:repo_name.git
$ git push --all git-migration
```

Teraz dobrze na nowo sklonować repozytorium, tym razem z git-a, żeby pozbyć się `git-svn`-owych śmieci.

### Naprawianie autorów komitów z svn

`git-svn` pozwala na mapowanie autorów svn-owych na pełniejszą formę git-ową (`--autors-file` w `git-svn` / `svn.authorsfile` w konfigu). Skorzystamy z tego pliku, żeby zmodyfikować autorów w samym repozytorium, przed zpushowaniem go.

Teraz możemy zmodyfikować komity w lokalnym repozytorium używając skryptu z https://gist.github.com/marek-saji/5232629. Znajduje się tam też instrukcja jak utworzyć plik plik `authorsfile` dla repozytorium SVN.

###### Resources

* pytanie [How do I change the author of a commit in git?](http://stackoverflow.com/questions/750172/how-do-i-change-the-author-of-a-commit-in-git) na StackOverflow
* blogpost [Converting a Subversion repository to Git](http://john.albin.net/git/convert-subversion-to-git)
* `man [git-svn](http://git-scm.com/docs/git-svn)`

# git przewodnik

## Podstawowe polecenia `git` dla początkującego

### Przeniesienie zmian na inny branch.

Damn! Byłem na złym branchu, jak to (z)robiłem!

#### Przenoszenie commita zpushowanego na repo

```sh
git log # i szukasz hash-a commita
git checkout next
git cherry-pick TEN_HASH
```

#### Przenoszenie niezpushowanego na repo commita

Jeśli trzeba, użyj `git rebase -i`, żeby interesujący nas commit był ostatni.

```sh
git checkout INNY_BRANCH
git cherry-pick TAMTEN_BRANCH
git checkout INNY_BRANCH
git rebase -i HEAD~~
# i usuń z listy commit (inaczej zostanie na TAMTEN_BRANCH)
```

#### Przenoszenie niezcommitowanych zmian

```sh
git stash save 'możesz podać komentarz, ale nie musisz'
git checkout next
git stash pop
```

tylko niektórych:

```sh
# -p włączy interaktywne wybieranie kawałków
git stash save -p 'to, co tutaj ma zostać'
git stash save 'to, co ma iść gdzie indziej'
git checkout GDZIE_INDZIEJ
git stash pop
git commit -a
git svn dcommit
git checkout - # btw. to wraca na poprzedni branch, na którym się było
git stash pop
```

##### Podstawy pracy z `git stash`

* stashin'
  ```sh
git stash save "opcjonalna nazwa"
# jeśli nie podajemy nazwy, "save" też można pominąć
git stash
```
* listin'
  `git stash list`
* oglądanie ostatniej zestashowanej zmiany
  `git stash show -p`
* oglądanie _nie_ ostatniej zestashowanej zmiany
  ```sh
git stash listh # i szukamy identyfikatora (stash@{LICZBA})
git stash show -p stash@{LICZBA}
```
* więcej:
  * [git-stash(1)(git-stash man page)](http://git-scm.com/docs/git-stash)
  * [Stashing](http://git-scm.com/book/en/Git-Tools-Stashing) chapter in the Git Book

### Cofanie zmian

#### Ze zdalnego repozytorium — `git revert`

```sh
git revert SOME_COMMIT_HASH
```

Utworzy to nowy commit, który cofnie (założy odwrotne zmiany) wskazany commit.

#### Z lokalnego repozytorium — `git reset`

* Wycofanie wszystkich zmian w workingcopy (tylko w śledzonych plikach) do stanu z ostatniego commit-a:
  `git reset --hard`
* Wycofanie ostatniego *nieskomitowanego(!!)* commit-a do workingcopy:
  `git reset HEAD~`

Podstawowe opcje `git reset`:

* nie podanie żadnego ref zostanie użyte `HEAD`
* `--hard` usunie wszelkie zmiany, jakie mamy w workingcopy, podczas gdy pominięcie go pozostawi je nienaruszone.
* `--soft` przywróci zmiany nie do workingcopy, ale do staging area.

Więcej w [git-reset(1)](http://git-scm.com/docs/git-reset.)

## Udogodnienia

### I want a GUI!

Nie można zrobić tam wszystkiego tego, co z konsoli, ale czasem jednak wygodniej przegląda się logi, czy wybiera pliki do skomitowania przy pomocy graficznego interfejsu.

* TortoiseGit ssie, z tego co mi wiadomo.
* gitk
  Najwyraźniej na windowsie odpalany poleceniem:
  `git gui`
* gitg
  `apt-get install gitg`
  Ładniejszy, niż gitk, ale nie ma na windowsy.
* GitHub for Windows
  http://windows.github.com
  Z tego co wiem, działa nie tylko z repozytoriami z GitHub-a.
  Używałeś? Napisz, czy warto.

## Bardziej zaawansowane polecenia

### Modyfikacja lokalnych commitów

Przy czystym working copy (można użyć `git stash`).

```sh
# szukasz ostatniego commita, jaki jest na repo
git lg
git rebase -i TEN_COMMIT
```

Ostatnie polecenie wyświetli listę z:

1. co zrobić z tym commitem (`pick`, `reword`, `fixup`, `squash` etc) 
  Pod listą opisane jest co znaczą poszczególne rzeczy.
2. hash
3. commit msg

Można zmieniać kolejność oraz zmienić pierwszą kolumnę.

*UWAGA*

* rebase-owanie z udziałem comitów, które są już na repo to *bardzo* zły pomysł
* usunięcie linijki z komitem spowoduje usunięcie samego commita

Więcej:
* [git-rebase(1)](http://git-scm.com/docs/git-rebase)
* [Rebasing](http://git-scm.com/book/en/Git-Branching-Rebasing) chapter in the Git Book
