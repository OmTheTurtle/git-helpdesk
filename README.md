# git-helpdesk

Az alábbi handoutban néhány terminal-os git parancsot / best practice-tx fogok bemutatni. Mindenkit ösztönzök, hogy ismerkedjen meg a terminal használatával legalább alap szinten, ugyanis a git kezelésnek szerintem ez a leggyorsabb, leguniverzálisabb módja (a terminál minden gépen ugyanúgy fog kinézni és viselkedni).

## Commit message

Néhány konvenció a commit message írásához:

- angol nyelvű
- jelen idejű
- lehetőleg tömör, kb. 40 karakter max.
- fejezze be a "When applied, this commit will..." mondatot, pl. "Update login form", vagy "Fix auth controller test".

## Merge vagy rebase

Alapszabály nálam, hogy fejlesztés közben mindig a rebase a preferált. Vigyánzni kell vele, mert átírja a it history-t, de egy rendezettebb, szebb tree-t eredményez. Merge commit-ot eddig csak pull request master-be húzásához használtam.

## Kerüljük el a merge commit-ot

A merge commit alapvetően nem eredményez szép history-t és nem praktikus, hogy a commitoló nevén lehet olyan változtatás, amit nem ő csinált. Ezért pull esetében mindig érdemes a --rebase kapcsolót használni, hogy megelőzzük a merge commit létrejöttét.

## Kimaradt valami a commitból, és szeretném még hozzáadni

`git add <file>`
`git commit --amend`

## Valami más is került a commitba, és szeretném kivenni

`git reset --soft HEAD~1` segítségével a legutolsó commit változtatásai vissza kerülnek staging-be. Innen kivenni fájlt a `git reset HEAD <file>` paranccsal lehet.

## Túl sok commitom lett, szeretném összegyúrni őket egybe

Ilyenkor jön jól az interactive rebase, amivel egyszerűen módosíthatunk régebbi commitokat is.

`git rebase -i HEAD~<count>`, ahol a `<count>` azt mondja meg, hány commit-ot szeretnénk kezelni. Ezután megadhatjuk, hogy melyik commitokat szeretnénk "összenyomni".

## Commitoltam a branchemen, de közben frissült a master és szeretném ezeket átvinni a saját branchemre

A `current` branchen `git rebase <branchname>` parancs kiadásával lehet frissíteni a `current` branch-et a `<banchname>`-en lévő commitokkal.

## Merge conflict

Bár érdemes elkerülni, de előfordulhat, hogy egyszerre két ember szerkesztette ugyanazon fájl egyazon sorát két külön commitban, és ez rebase esetében confliktushoz vezet (a git nem tudja kitalálni, melyik módosítást hajtsa végre, ezért ezt a fejlesztőre bízza). Ilyenkor érdemes a VS Code vagy egyéb IDE segítségéért folyamodni, ezek sokkal kényelmesebb módját adnak a conflict feloldására.

## Aliasok

Ha terminált használunk, érdemes aliasokat beállítani a gitre, hogy ne kelljen mindig hosszú parancsokat begépelnünk. az én .gitconfig fáljom aliasai a következők:

```
[alias]
        br = branch
        ch = checkout
        chb = checkout -b
        co = commit
        st = status
        lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
        lgo = log --oneline
        ps = push
        psf = push --force
        pu = pull
        pur = pull --rebase
        rb = rebase
        com = commit -m
        amend = commit --amend
        aa = add --all

```

## Hasznos linkek

- Commit message: <https://chris.beams.io/posts/git-commit/>
- Rebase vs merge: <https://www.atlassian.com/git/tutorials/merging-vs-rebasing>
- Avoid merge commits: <http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/>
- Best practices: <https://raygun.com/blog/git-workflow/>, <https://www.git-tower.com/learn/git/ebook/en/command-line/appendix/best-practices>
