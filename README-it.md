[中文版](./README-zh.md)
| [日本語版](./README-ja.md)
| [한국어](./README-ko.md)
| [Русский](./README-ru.md)
| [Português](./README-pt-BR.md)

[<img src="./images/elsewhen-logo.png" width="180" height="180">](https://www.elsewhen.com/)

# Linee guida di un progetto &middot; [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

> Se sviluppare un nuovo progetto è per voi come rotolarsi in un campo erboso, il mantenimento
> è un potenziale oscuro incubo per qualcun altro.
> Ecco un elenco delle linee guida che abbiamo trovato, scritto e raccolto che (pensiamo), possano
> realmente ben funzionare con la maggior parte dei progetti qui a [elsewhen](https://www.elsewhen.com).
> Se volete condividere una pratica ottimale, o pensate che qualcuna di queste linee guida debba essere rimossa, [fatecelo sapere](http://makeapullrequest.com).

<hr>

- [Linee guida di un progetto &middot; ![PRs Welcome](http://makeapullrequest.com)](#linee-guida-di-un-progetto--img-srchttpsimgshieldsiobadgeprs-welcome-brightgreensvgstyleflat-square-altprs-welcome)
  - [1. Git](#1-git)
    - [1.1 Alcune regole di Git](#11-some-git-rules)
    - [1.2 Flusso di lavoro di Git](#12-git-workflow)
    - [1.3 Scrivere efficaci messaggi di commit](#13-writing-good-commit-messages)
  - [2. Documentazione](#2-documentation)
  - [3. Ambienti](#3-environments)
    - [3.1 Ambienti di sviluppo consistenti:](#31-consistent-dev-environments)
    - [3.2 Dipendenze consistenti:](#32-consistent-dependencies)
  - [4. Dipendenze](#4-dependencies)
  - [5. Collaudo](#5-testing)
  - [6. Denominazioni e strutture](#6-structure-and-naming)
  - [7. Stile di codice](#7-code-style)
    - [7.1 Alcune linee guida di stile di codice](#71-some-code-style-guidelines)
    - [7.2 Applicare uno standard nello stile di codice](#72-enforcing-code-style-standards)
  - [8. Logging](#8-logging)
  - [9. API](#9-api)
    - [9.1 Progettazione API](#91-api-design)
    - [9.2 Sicurezza API](#92-api-security)
    - [9.3 Documentazione API](#93-api-documentation)
  - [10. Gestione Licenze](#10-licensing)

<a name="git"></a>

## 1. Git

![Git](/images/branching.png)
<a name="some-git-rules"></a>

### 1.1 Alcune regole Git

Ecco un insieme di regole da tenere a mente:

- Eseguire il lavoro in un ramo di funzionalità.

  _Perchè:_

  > In questo modo tutto il lavoro viene fatto in isolamento su un ramo dedicato piuttosto che nel ramo principale. Vi consente di sottomettere delle richieste _pull_ multiple senza creare confusione. Potete iterare senza inquinare il ramo principale con codice potenzialmente instabile e non completato. [leggi di più...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)

- Branch out from `develop`

  _Perchè:_

  > In questo modo ci si può assicurare che il codice nel ramo principale possa essere quasi sempre compilato senza problemi, e che possa essere principalmente usato per i rilasci (potrebbe essere una esagerazione per alcuni progetti).

- Mai eseguire _push_ nei rami di `develop` o `master`. Eseguire una richiesta _pull_

  _Perchè:_

  > Notifica i membri della squadra che una caratteristica è stata completata. Consente anche una facile revisione tra i propri pari del codice e una discussione della caratteristica proposta sui _forum_ dedicati.

- Aggiornate il ramo `develop` locale ed eseguite un _rebase_ interattivo prima proporre la propria caratteristica ed eseguire una richiesta _pull_.

  _Perchè:_

  > L'azione di _rebase_ integrerà i commit fatti localmente nei rami richiesti (`master` o `develop`) e all'inizio della storicizzazione senza creare un _merge commit_ (assumendo che non ci siano conflitti). Il risultato è una buona e pulita storicizzazione. [leggi di più ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

- Resolvete conflitti potenziali durante l'azione di _rebase_ e prima di eseguire una richiesta _pull_.

- Eliminate i rami di caratteristiche locali e remoti dopo l'integrazione.

  _Perchè:_

  > Il non farlo sporcherà il vostro elenco di rami con rami morti. Assicura che possiate integrare il rano in (`master` o `develop`) una volta sola. I rami di caratteristica dovrebbero esistere solo se il lavoro è ancora in corso.

- Prima di eseguire una richiesta _pull_, assicuratevi che il vostro ramo di caratteristica venga compilato con successo e superi tutti i test (compresi quelli di stile di codice).

  _Perchè:_

  > State per aggiungere il vostro codice a un ramo stabile. Se i test nel vostro ramo di caratteristica falliscono, ci sarà un'altra probabilità che la compilazione nel ramo di destinazione fallirà anch'essa. Inoltre, occorre applicare un controllo di stile di codice prima di eseguire una richiesta _pull_. Aggiunge leggibilità e riduce le possibilità che correzioni di formattazione vengano mescolate con le vere modifiche.

- Usate [this](./.gitignore) il file `.gitignore`.

  _Perchè:_

  > Ha già un elenco di file di sistema che non dovrebbero essere inviati assieme al proprio codice in un deposito di codice remoto. Inoltre esclude cartelle e file di impostazioni per la maggior parte degli editor utilizzati, così come molte delle più comuni cartelle di dipendenze.

- Proteggete i vostri rami di `develop` e `master`.

  _Perchè:_

  > Protegge i vostri rami pronti per la produzione dal ricevere modifiche inattese e irreversibili. leggi di più... [Github](https://help.github.com/articles/about-protected-branches/), [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html) and [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)

<a name="git-workflow"></a>

### 1.2 Flusso di lavoro di Git

Per la maggior parte delle ragioni sopra esposte usiamo il flusso di lavoro [Feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) con [Rebase Interativo Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) ed alcuni elementi di [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) (denominazioni e avere un ramo di sviluppo). I passi principali sono i seguenti:

- Per un nuovo progetto, inizializzare un deposito git nella directory del progetto. **Per le caratteristiche/modifiche successive questo passo dovrebbe essere ignorato**.

  ```sh
  cd <directory del progetto>
  git init
  ```

- Eseguire il _checkout_ di un nuovo ramo di caratteristica/risoluzione errore.

  ```sh
  git checkout -b <nome ramo>
  ```

- Eseguire le modifiche.

  ```sh
  git add <file1> <file2> ...
  git commit
  ```

  _Perchè:_

  > `git add <file1> <file2> ... ` - si dovrebbero aggiungere solo file che costituiscono una piccola e coerente modifica.

  > `git commit` lancerà un editor che vi consente di separare il soggetto dal corpo.

  > Leggete di più in merito nella _section 1.3_.

  _Suggerimento:_

  > Potete invece usare `git add -p`, che potrebbe darvi la possibilità di rivedere tutte le modifiche introdotte una ad una e decidere se includerle in un _commit_ oppure no.

- Sincronizzare con il deposito remoto per ottenere modifiche che altrimenti avreste perso.

  ```sh
  git checkout develop
  git pull
  ```

  _Perchè:_

  > Vi fornisce la possibilità di gestire i conflitti sulla propria macchina mentre si esegue la successiva azione di _rebase_ invece che creare una richiesta _pull_ che contiene conflitti.

- Aggiornate il vostro ramo di caratteristica con le ultime modifiche da `develop` tramite _rebase_ interattivo.

  ```sh
  git checkout <branchname>
  git rebase -i --autosquash develop
  ```

  _Perchè:_

  > Potete usare `--autosquash` per comprimere tutti i vostri _commit_ in un _commit_ singolo. Nessuno vuole molti _commit_ per una singola caratteristica in un ramo di sviluppo. [leggi di più...](https://robots.thoughtbot.com/autosquashing-git-commits)

- Se non avete conflitti saltate questo passo. Se avete conflitti, [risolveteli](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/) e continuate l'azione di _rebase._

  ```sh
  git add <file1> <file2> ...
  git rebase --continue
  ```

- Eseguite l'azione di _push_ del vostro ramo. L'azione di _rebase_ modificherà la storicizzazione, quindi dovrete usare `-f` per forzare le modifiche nel ramo remoto. Se qualcun altro sta lavorando sul vostro ramo, usare l'opzione meno distruttiva `--force-with-lease`.

  ```sh
  git push -f
  ```

  _Perchè:_

  > Quando eseguite una azione di _rebase_, state modificando la storicizzazione del vostro ramo di caratteristiche. Come risultato, Git respingerà i normali `git push`. Dovrete invece usare l'opzione `-f` o `--force flag`. [leggi di più...](https://developer.atlassian.com/blog/2015/04/force-with-lease/)

- Eseguite una richiesta _pull_.
- La richiesta _pull_ verrà accettata, incorporata e chiusa da un revisore.
- Rimuovete il vostro ramo locale di caratteristica se avete finito.

  ```sh
  git branch -d <branchname>
  ```

  rimuovere tutti i rami che non sono più nel deposito remoto

  ```sh
  git fetch -p && for branch in `git branch -vv --no-color | grep ': gone]' | awk '{print $1}'`; do git branch -D $branch; done
  ```

<a name="writing-good-commit-messages"></a>

### 1.3 Scrivere messaggi di _commit_ efficaci

Avere buone linee guida per la creazione di _commit_ e osservarle rende molto più facile lavorare con Git e collaborare con altri. Ecco alcune regole di massima ([sorgente](https://chris.beams.io/posts/git-commit/#seven-rules)):

- Separare l'oggetto dal corpo con una riga vuota tra i due.

  _Perchè:_

  > Git è sufficientemente capace di considerare la prima riga del vostro messagio di _commit_ come il vostro sommario. In effetti se eseguite `git shortlog`, invece che `git log`, vedrete un lungo elenco di messaggi di _commit_, che contengono l'identificativo del commit e il solo sommario.

- Limitate la riga dell'oggetto a 50 caratteri e la lunghezza della riga nel corpo a massimo 72 caratteri.

  _Perchè:_

  > I _commit_ dovrebbero essere più dettagliati e specifici possibile, non è il posto per essere prolissi. [leggi di più...](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)

- Maiuscole nella riga di oggetto.
- Non terminate la riga dell'oggetto con un punto.
- Usete [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) nella riga dell'oggetto.

  _Perchè:_

  > Invece che scrivere messaggi che dicono cosa ha fatto chi ha eseguito il commit, è meglio considerare questi messaggi come istruzioni per quello che dovrà essere fatto dopo che il _commit_ è applicato nel deposito. [leggi di più...](https://news.ycombinator.com/item?id=2079612)

- Usare il corpo per spiegare **cosa** e **perchè** invece di **come**.

<a name="documentation"></a>

## 2. Documentazione

![Documentazione](/images/documentation.png)

- Usete questo [modello](./README.sample.md) o `README.md`. Siate liberi di aggiungere sezioni non trattate.
- Per progetti con più di un deposito, fornire collegamenti a essi nei rispettivi file `README.md`.
- Mantenere aggiornato `README.md` mano a mano che il progetto evolve.
- Commentate il vostro codice. Cercate di renderlo il più chiaro possibile cosa intendete con ogni sezione principale.
- Se esiste una discussione aperta su github o stackoverflow riguardo al codice o all'approccio che state usando, includete il collegamento nel vostro commento.
- Non usate commenti come scusa per cattivo codice.
- Mantenere il proprio codice pulito.
- Non usare il codice pulito come scusa per non commentarlo.
- Mantenete i commenti rilevanti mano a mano che il vostro codice evolve.

<a name="environments"></a>

## 3. Ambienti

![Environments](/images/laptop.png)

- Definite ambienti `development` (sviluppo), `test` (collaudo) e `production` (produzione) separati se serve.

  _Perchè:_

  > Dati diversi, token, API, porte ecc... potrebbero essere necessari in ambienti diversi. Potreste volere un ambiente `development` isolato che chiami delle "false" API che forniscono dati predeterminati, rendendo i test sia manuali che automatici molto più facili. Oppure potreste voler abilitare Google Analytics solo in `production` e così via. [leggi di più...](https://stackoverflow.com/questions/8332333/node-js-setting-up-environment-specific-configs-to-be-used-with-everyauth)

- Caricate le vostre configurazioni di sviluppo specifiche da variabili di ambientee e non aggiungetele mai alla base di codice come costanti, [guardate questo esempio](./config.sample.js).

  _Perchè:_

  > Avete token, password e altre preziose informmazioni. La vostra configurazione dovrebbe essere correttamente separata dalle logiche interne dell'app come se la base di codice potesse essere resa pubblica in qualsiasi momento.

  _Come:_

  > Usate file `.env` per conservare le vostre variabili ed aggiungeteli a `.gitignore` per escluderli. Eseguite un _commit_ di un `.env.esempio` che serva come guida per gli sviluppatori. Per la produzione, dovreste comunque impostare le vostre variabili nel modo standard. [leggi di più](https://medium.com/@rafaelvidaurre/managing-environment-variables-in-node-js-2cb45a55195f)

- E' raccomandato che si validino le variabili di ambiente prima che la vostra app venga lanciata.. [Guardate questo esempio](./configWithTest.sample.js) che usa `joi` per validare i valori passati.

  _Perchè:_

  > Potrebbe risparmiarvi ore passate a risolvere problemi.

<a name="consistent-dev-environments"></a>

### 3.1 Ambienti di sviluppo consistenti:

- Impostate la vostra versione di node in `engines` e `package.json`.

  _Perchè:_

  > Consente agli altri di sapere su quale versione di node il progetto lavora. [leggi di più...](https://docs.npmjs.com/files/package.json#engines)

- Inoltre usate `nvm` e create un `.nvmrc` in radice del vostro progetto. Non dimenticate di citarlo nella documentazione

  _Perchè:_

  > Chiunque usi `nvm` piò semplicemente usare `nvm use` per passare alla versione di node adatta. [leggi di più...](https://github.com/creationix/nvm)

- E' una buona idea impostare uno stript `preinstall` che verifichi le versioni di node e npm.

  _Perchè:_

  > Alcune dipendenze potrebbero fallire quando installate da versioni più nuove di npm.

- Usate immagini Docker se potete.

  _Perchè:_

  > Vi può fornire un ambiente consistente lungo tutto il processo di lavoro. Senza tanto bisogno di armeggiare con dipendenze o configurazioni. [leggi di più...](https://hackernoon.com/how-to-dockerize-a-node-js-application-4fbab45a0c19)

- Usate moduli locali invece di quelli installati globalmente.

  _Perchè:_

  > Vi consente di condividere il vostro equipaggiamento con il vostro collega invece di supporre che li abbia installati globalmente sul proprio sistema.

<a name="consistent-dependencies"></a>

### 3.2 Consistenza nella dipendenze:\*\*\*\*

- Assicuratevi che i membri della vostra squadra abbiano le stesse esatte vostre dipendenze.

  _Perchè:_

  > Perchè volete che il codice si comporti come atteso e in modo identico in qualsiasi macchina di sviluppo [leggi di più...](https://kostasbariotis.com/consistent-dependencies-across-teams/)

  _how:_

  > Usate `package-lock.json` su `npm@5` o superiori

  _Non ho npm@5:_

  > Come alternativa potreste usare `Yarn` e assicurarvi di citarlo nel `README.md`. I vostri file di lock e `package.json` dovrebbero avere le stesse versioni dopo qualsiasi aggiornamento di ciascuna dipendenza. [leggi di più...](https://yarnpkg.com/en/)

  _Non mi piace il name `Yarn`:_

  > Peccato. Per versioni più vecchie di `npm`, usate `—save --save-exact` quando installate una nuova dipendenza e create `npm-shrinkwrap.json` prima della pubblicazione. [leggi di più...](https://docs.npmjs.com/files/package-locks)

<a name="dependencies"></a>

## 4. Dipendenze

![Github](/images/modules.png)

- Tenete traccia dei vostri pacchetti attualmente disponibili: `npm ls --depth=0`. [leggi di più...](https://docs.npmjs.com/cli/ls)
- Verificate se qualcuno dei vostri pacchetti è diventato irrilevante o inutilizzato: `depcheck`. [leggi di più...](https://www.npmjs.com/package/depcheck)

  _Perchè:_

  > Potreste includere una libreria inutilizzata nel vostro codice aumentando la dimensione del pacchetto di produzione. Cercate le dipendenze inutilizzate e sbarazzatevene.

- Prima di usare una dipendenza, verificate le statistiche degli scaricamenti per verificare se sia ampiamente utilizzata dalla comunità: `npm-stat`. [leggi di più...](https://npm-stat.com/)

  _Perchè:_

  > Più utilizzi in genere significa più collaboratori, il che in genere significa migliore manutenzione, e la conseguenza è che i bug vengono scoperti e corretti più velocemente.

- Prima di usare una dipendenza, verificate se ha un rilascio di versione buona, matura e con un vasto numero di manutentori: `npm view async`. [leggi di più...](https://docs.npmjs.com/cli/view)

  _Perchè:_

  > Avere un gran numero di sottomissioni di codice da parte dei collaboratori non è così efficace se non ci sono manutentori che incorporano le correzioni e le patch con sufficiente velocità.

- Se è necesaria una dipendenza poco conosciuta, discutetene con la squadra prima di usarla.
- Assicuratevi sempre che la vostra app funzioni con le ultime versioni delle proprie dipendenze senza errori: `npm outdated`. [leggi di più...](https://docs.npmjs.com/cli/outdated)

  _Perchè:_

  > Gli aggiornamenti delle dipendenze talvolta contengono modifiche che rompono l'app. Verificate sempre le loro note di rilascio quando vengono messi a disposizione gli aggiornamenti. Aggiornate le vostre dipendenze una ad una, il che facilita la risoluzione dei problemi se qualcosa dovesse andare storto. Usate uno strumento tipo [npm-check-updates](https://github.com/tjunnone/npm-check-updates).

- Verificate se il pacchetto abbia delle vulnerabilità di sicurezza note con [Snyk](https://snyk.io/test?utm_source=risingstack_blog).

<a name="testing"></a>

## 5. Eseguire Test

![Testing](/images/testing.png)

- Se necessario abbiate un ambiente in modalità `test`.

  _Perchè:_

  > Sebbene qualche volta il test dall'inizio alla fine in `produzione` possa sembrare sufficiente, ci sono alcune eccezioni: un esempio è che potreste non voler abilitare informazioni analitiche in una modalità `produzione` e inquinare il cruscotto di qualcuno con dati di test. L'altro esempio è che la vostra API potrebber avere dei parametri di limite in `produzione` e bloccare le vostre chiamate di test dopo un certo numero di richieste.

- Posizionate i vostri file di test vicino ai moduli testati usando la convenzione nominale `*.test.js` o `*.spec.js`, tipo `nomeModulo.spec.js`.

  _Perchè:_

  > Non vorreste rovistare all'interno di una struttura di directory per trovare una unità di test. [leggi di più...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

- Inserite i vostri file di test addizionali in una cartella di test separata per evitare confusione.

  _Perchè:_

  > Alcuni file di test non sono particolarmente legati a specifici file di implementazione. Dovrete inserirli in una cartella che sia facile da trovare dagli altri sviluppatori: `__test__` Questo nome: `__test__` è anche uno standard ora e viene scelto dalla maggior parte delle infrastrutture di test di Javascript.

- Scrivete codice che si possa testare, evitate effetti collaterali, eliminate effetti collaterali, scrivete funzioni pure

  _Perchè:_

  > Vorrete testare una logica di _business_ come unità separate. Dovete "minimizzare l'impatto della casualità e dei processi non deterministici sulla affidabilità del vostro codice". [leggi di più...](https://medium.com/javascript-scene/tdd-the-rite-way-53c9b46f45e3)

  > Una funzione pura è una funzione che ritorna sempre lo stesso risultato per lo stesso input. Al contrario una funzione impura è quella che potrebbe avere effetti collaterali o dipende da condizioni esterne per produrre un valore. Il che la rende meno prevedibile. [leggi di più...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

- Usate un verificatore di tipo statico

  _Perchè:_

  > Talvolta dovreste aver bisogno di un verificatore di tipo statico. Porta un certo grado di affidabilità al vostro codice. [leggi di più...](https://medium.freecodecamp.org/why-use-static-types-in-javascript-part-1-8382da1e0adb)

- Eseguire i test localmente prima di eseguire una richiesta _pull_ in `sviluppo`.

  _Perchè:_

  > Non vorrete essere quelli che hanno causato una fallita compilazione in un ramo pronto per la produzione. Eseguite i vostri test prima della vostra azione di _rebase_ e prima di inviare il vostro ramo di caratteristica in un deposito remoto.

- Documentate i vostri test includendo istruzioni nelle sezioni rilevandt del vostro file `README.md`.

  _Perchè:_

  > E' una nota utile che voi lasciate a disposizione degli altri sviluppatori o esperti DevOps o chiunque sia abbastanza fortunato da lavorare con il vostro codice.

<a name="structure-and-naming"></a>

## 6. Struttura e Assegnazione dei Nomi

![Structure and Naming](/images/folder-tree.png)

- Organizzate i vostri file attorno a caratteristiche / pagine / componenti, non ruoli. Inoltre inserite i vostri file di testo vicino alla loro implementazione.

  **Cattivo**

  ```
  .
  ├── controllers
  |   ├── product.js
  |   └── user.js
  ├── models
  |   ├── product.js
  |   └── user.js
  ```

  **Buono**

  ```
  .****
  ├── product
  |   ├── index.js
  |   ├── product.js
  |   └── product.test.js
  ├── user
  |   ├── index.js
  |   ├── user.js
  |   └── user.test.js
  ```

  _Perchè:_

  > Invece di un lungo elenco di file, creerete piccoli moduli che incapsulano una responsabilità compresi i propri test e così via. E' molto più facile navigarli e le cose si possono trovare a colpo d'occhio.

- Inserite i vostri file di test aggiuntivi in cartelle di test separate per evitare confusione.

  _Perchè:_

  > Costituisce un risparmio di tempo per gli altri sviluppatori o esperti DevOps nella vostra squadra.

- Usate una cartella `./config` e non create file di configurazione diversi per i diversi ambienti.

  _Perchè:_

  > Quando dividete un file di configurazione per diversi scopi (database, API eccetera) metteteli in una cartella con un nome molto riconoscibile tipo `config`. Ricordate di non generare diversi file di configurazione per diversi ambienti. Saranno necessari nuovi nomi di ambiente per ogni deploy dell'app che state creando.
  > I valori da usare nei file di configurazione dovrebbero essere forniti da variabili di ambiente. [leggi di più...](https://medium.com/@fedorHK/no-config-b3f1171eecd5)

- Inserite i vostri script in una cartella `./scripts` . Compresi gli script `bash` e `node`.

  _Perchè:_

  > E' molto probabile che finirete per avere più di uno script, per la produzione, lo sviluppo, alimentatori di database, sincronizzatori di database eccetera.

- Piazzate il risultato delle compilazioni in una cartella `./build`. Aggiungete `build/` a `.gitignore`.

  _Perchè:_

  > Chiamatela come vi pare, anche `dist` va bene, ma assicuratevi di mantenere consistenza con la vostra squadra. Quello che finisce lì per la maggior parte è generato (assemblato, compilato, soggetto a _transpiling_), o ivi spostato. I componenti della vostra squadra dovrebbero essere in grado di generare quello che generate voi, quindi non ha senso portare questi dati nel deposito remoto. A meno che non lo si voglia specificatamente.

<a name="code-style"></a>

## 7. Stile di codice

![Code style](/images/code-style.png)

<a name="code-style-check"></a>

### 7.1 Alcune linee guida sullo stile di codice

- Usate una sintassi di secondo stadio o superiore (moderna) di Javascript per i vostri nuovi progetti. Per quelli vecchi restate consistenti con la sintassi esistente a meno che intendiate modernizzare il progetto.

  _Perchè:_

  > Questo dipende interamente da voi. Usiamo programmi per il _transpiling_ per trarre vantaggio dalla nuova sintassi, è probabile che _stage-2_ diventi alla fine parte delle specifiche con poche minori revisioni.

- Includete verifiche di stile di codice nel vostro processo di compilazione.

  _Perchè:_

  > Interrompere la compilazione è un modo per imporre uno stile di codice. Vi evita di prenderlo sotto gamba. Fatelo sia per il codice della parte client che per quella server. [leggi di più...](https://www.robinwieruch.de/react-eslint-webpack-babel/)

- Usate [ESLint - Pluggable JavaScript linter](http://eslint.org/) per imporre lo stile di codice.

  _Perchè:_

  > Semplicemente noi preferiameo `eslint`, voi non siete obbligati. Supporta più regole e la possibilità di configurarle nonchè di aggiungerne di personalizzate.

- Usiamo [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) per JavaScript, [Read more](https://www.gitbook.com/book/duk/airbnb-javascript-guidelines/details). Usate lo stile di codice javascript richiesto dal vostro progetto o dalla vostra squadra.

- Usiamo [Flow type style check rules for ESLint](https://github.com/gajus/eslint-plugin-flowtype) quando usiamo [FlowType](https://flow.org/).

  _Perchè:_

  > _Flow_ introduce poca sintassi, la quale deve seguire certe regole di stile di codice e possono essere verificate.

- Usate `.eslintignore` per escludere file o cartelle dalle verifiche di stile di codice.

  _Perchè:_

  > Non dovete inquinare il vostro codice con commenti `eslint-disable` ogni volta che dovete escludere un paio di file dalla verifica di stile.

- Rimuovete tutti i vostri commenti di disabilitazione di `eslint` prima di eseguire una richiesta _pull_

  _Perchè:_

  > E' normale disabilitare verifiche di stile mentre si lavora a un blocco di codice per focalizzarsi più sulla logica. Solo ricordate di rimuovere quei commenti `eslint-disable` e seguite le regole

- A seconda della dimensione dell'attività usate commenti `//TODO:` oppure aprite un ticket.

  _Perchè:_

  > In questo modo potete ricordare agli altri e a voi stessi di una piccola attività (tipo refattorizzare una funzione o aggiornare un commento). Per attività più complessi usate `//TODO(#3456)` che viene impostato da una regola di _lint_ e dal numero del ticket aperto.

- Commentate sempre e mantenete i commenti in linea con le modifiche fino ad ora apportate al codice. Elminate i blocchi di codice commentati.

  _Perchè:_

  > Il vostro codice dovrebbe essere il più leggibile possibile, dovreste sbarazzarvi di ogni distrazione. Se rifattorizzate una funzione non commentate la vecchia, eliminatela.

- Evitate commenti, log e attribuzione di nominativi irrilevanti o divertenti.

  _Perchè:_

  > Anche se il vostro processo di compilazione potrebbe (dovrebbe) sbarazzarsi di questi, talvolta il vostro codice sorgente potrebbe essere affidato ad altra ditta/cliente e potrebbero non non trovarli così divertenti.

- Rendete i vostri nomi ricercabili con distinzioni significative ed evitate abbreviazioni di nomi. Per le funzioni usate nomi lunghi e descrittivi. Un nome di funzione dovrebbe essere un verbo o una frase verbale, e deve comuncare le proprie intenzioni.

  _Perchè:_

  > Rende la lettura del codice sorgente più naturale.

- Organizzate le vostre fuznoni in un file a seconda della regole di discesa. Funzioni di alto livello dovrebbero essere in testa e quelle di basso livello più in basso.

  _Perchè:_

  > Rende la lettura del codice sorgente più naturale.

<a name="enforcing-code-style-standards"></a>

### 7.2 Imporre standard di stile di codice

- Usate un file [.editorconfig](http://editorconfig.org/) che aiuta gli sviluppatori a definire e mantenere stili di codice consistente tra i diversi editor e IDE usati nel progetto.

  _Perchè:_

  > Il progetto EditorConfig consiste in un formato di file per definire stili di codice e una collezione di plugin per editor testi che consentono agli editor di leggere il formato di stile e aderire a stili definiti. I file EditorConfig sono facilmente leggibili e funzionano bene con sistemi di controllo di versione.

- Fate in modo che il vostro editor vi notifichi circa gli errori di stile di codice. Usate [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier) e [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier) con vostra configurazione esistente di ESLint. [leggi di più...](https://github.com/prettier/eslint-config-prettier#installation)

- Considerate l'uso dei _Git hooks_.

  _Perchè:_

  > Accrescono notevolmente la produttività di uno sviluppatore. Fate modifiche, confermate e portate sugli ambienti di staging o produzione senza paura di rompere la compilazione. [leggi di più...](http://githooks.com/)

- Usate _Prettier_ con un _hook_ prima del commit.

  _Perchè:_

  > Sebbene `prettier` per se stesso possa essere molto potente, non è molto produttivo se eseguito semplicemente come una attività npm a se stante ogni volta per formattare il codice. Ecco dove `lint-staged` (e `husky`) entrano in gioco. Leggete di più sul come configurare `lint-staged` [qui](https://github.com/okonet/lint-staged#configuration) e `husky` [here](https://github.com/typicode/husky).

<a name="logging"></a>

## 8. Logging

![Logging](/images/logging.png)

- Evitare log lato console client in produzione.

  _Perchè:_

  > Anche se il vostro processo di compilazione possa (dovrebbe) sbarazzarsene, assicuratevi che il vostro verificatore di stile di codice vi avvisi rispetto a log su console lasciati nel codice.

- Producete dei log di produzione leggibili. Idealmente utilizzare librerie di _logging_ in produzione (tipo [winston](https://github.com/winstonjs/winston) o
  [node-bunyan](https://github.com/trentm/node-bunyan)).

  _Perchè:_

  > Rende l'identificazione dei problemi molto meno spiacevole con colorizzazioni, marcature temporali, registrazioni a un file oltre a quelle su console, anche la registrazione su file che ruota giornalmente. [leggi di più...](https://blog.risingstack.com/node-js-logging-tutorial/)

<a name="api"></a>

## 9. API

<a name="api-design"></a>

![API](/images/api.png)

### 9.1 Progettazione API

_Perchè:_

> Cerchiamo di imporre lo sviluppo di interfacce _RESTFUL_ ben costruite, che possono essere consumate dai membri della squadra e i client in modo semplice e consistente.

_Perchè:_

> La mancanza di consistenza e semplicità può accrescere enormemente i costi di integrazione e mantenimento. Ecco perchè `Progettazione API` è incluso in questo documento.

- Seguite per la maggior parte una progettazione orientata alle risorse. Ci sono tre fattori principali: risorse, collezioni e URL.

  - Una risorsa ha dati, viene annidata e ci sono metodi che operano su di essa.
  - Un gruppo di risorse è chiamata collezione.
  - URL identifica la locazione online di risorse o collezioni.

  _Perchè:_

  > Questa è una progettazione ben nota agli sviluppatori (i vostri principali consumantori di API). A parte la leggibilità e la facilità d'uso, consente di scrivere librerie generiche e connettori senza neppure sapere come sia fatta l'API stessa.

- usete il _kebab-case_ per gli URLs.
- usate il _camelCase_ per parametri in _query string_ o campi che rappresentano uno risorsa.
- usate il _kebab-case_ al plurale per nomi di risorse negli URL.

- Usate sempre la forma plurale dei nomi per denominare un url che punta a una collezione: `/users`.

  _Perchè:_

  > Fondamentalmente risulta meglio leggibile e rende l'URL consistente. [leggi di più...](https://apigee.com/about/blog/technology/restful-api-design-plural-nouns-and-concrete-names)

- Nel codice sorgente convertite le forme plurali in variabili e le proprietà con un suffisso List.

  _Why_:

  > La forma plurale va bene negli URL ma nel codice sorgente è troppo debole e incline a errori.

- Usate sempre un concetto al singolare che parte da una collezione e finisce con un identificatore:

  ```
  /students/245743
  /airports/kjfk
  ```

- Evitare URL tipo questo:

  ```
  GET /blogs/:blogId/posts/:postId/summary
  ```

  _Perchè:_

  > Non punta a una risorsa ma a una proprietà. Si possono passare le proprietà come parametro per ridurre la propria risposta.

- Mantenere i verbi al di fuori dei vostri URL di risorse.

  _Perchè:_

  > Se usate un verbo per ogni operazione su una risorsa presto avrete una enorme lista di URL e un modello non consistente che lo rende difficile da imparare per gli sviluppatori. Inoltre i verbi si usano per altri scopi.

- Usate verbi per non-risorse. In questo caso, la vostra API non ritorna alcuna risorsa, viceversa voi eseguite una operazione e ritornate il risultato. Queste **non sono** operazioni CRUD (creazione, recupero, aggiornamento e cancellazione):

  ```
  /translate?text=Hello
  ```

  _Perchè:_

  > Per le operazioni CRUD usiamo i metodi HTTP su URL di `risorse` o `collezioni`. I verbi di cui stiamo parlando sono in realtà `Controllers`. In genere non ne svilupperete molti di questi. [leggi di più...](https://byrondover.github.io/post/restful-api-guidelines/#controller)

- Il corpo della richiesta o il tipo di risposta è JSON pertanto seguite la forma `camelCase` per i nomi di proprietà per mantenere una consistenza.

  _Perchè:_

  > Queste sono linee guida per un progetto Javascript, dove il linguaggio di programmazione per generare ed elaborare JSON si assume sia JavaScript.

- Anche se una risorsa rappresenta un concetto al singolare, simile a una istanza di un oggetto o un record di database, non dovreste usare il `nome_tabella` per un nome di risorsa e il `nome_colonna` per una proprietà..

  _Perchè:_

  > Il vostro intendimento è di esporre risorse, non i dettagli dello schema del vostro database.

- Ancora una volta, non usate nomi nei vostri URL quando dovete nominare le vostre risorse e non cercate di spiegarne la loro funzionalità.

  _Perchè:_

  > Usate nomi solamente nei vostri URL di risorsa, evitate URL che finiscono con `/addNewUser` or `/updateUser`. Evitate inoltre di inviare operazioni su risorse come parametro.

- Esprimente le funzionalità CRUD usando i metodi HTTP:

  _How:_

  > `GET`: Per ottenere una rappresentazione di una risorsa.

  > `POST`: Per creare nuove risorse e sotto risorse.

  > `PUT`: Per aggiornare risorse esistenti.

  > `PATCH`: Per aggiornare risorse esistenti. Aggirna solo i campi che gli sono stati forniti lasciando gli altri invariati.

  > `DELETE`: Per eliminare risorse esistenti

- Per risorse annidate, usate la relazione tra loro nell'URL. Ad esempio usando `id` per collegare un dipendente a una ditta.

  _Perchè:_

  > Questo è il modo naturale per rendere le risorse esplorabili.

  _Come:_

  > `GET /schools/2/students ` , dovrebbe ottenere la lista di tutti gli studenti dalla scuola 2.

  > `GET /schools/2/students/31` , dovrebbe ottenere i dettagli dello studente 31, che appartiene alla scuola 2.

  > `DELETE /schools/2/students/31` , dovrebbe eliminare lo studente 31, che appartiene alla scuola 2.

  > `PUT /schools/2/students/31` , dovrebbe aggiornare le info sullo studente 31, usate PUT solo su URL che rappresentano risorse, non collezioni.

  > `POST /schools` , dovrebbe creare una nuova scuola e ritornare i dettagli della nuova scuola creata. Usate POST su URL che rappresentano una collezione.

- Usate un semplice numero ordinale per una versione con un prefisso `v` (v1, v2). Spostate tutto alla sinistra nell'URL in modo che abbia l'ordine di identificazione maggiore:

  ```
  http://api.domain.com/v1/schools/3/students
  ```

  _Perchè:_

  > Quando le vostre API sono disponibili per terze parti, l'aggiornamento di API con alcune modifiche incompatiili con le versioni precedenti faranno sì che i prodotti o servizi che si basano su di esse non funzioneranno più. L'utilizzo di un versionamento nel vostro URL previene questa eventualità. [leggi di più...](https://apigee.com/about/blog/technology/restful-api-design-tips-versioning)

- I messaggi di risposta devono essere auto descrittivi. Un buon messaggio di errore in risposta potrebbe essere tipo questo:

  ```json
  {
    "code": 1234,
    "message": "E' successo qualcosa di brutto",
    "description": "Maggiori dettagli"
  }
  ```

  oppure per errori di validazione:

  ```json
  {
    "code": 2314,
    "message": "Validazione fallita",
    "errors": [
      {
        "code": 1233,
        "field": "email",
        "message": "Email non valida"
      },
      {
        "code": 1234,
        "field": "password",
        "message": "Nessuna password fornita"
      }
    ]
  }
  ```

  _Perchè:_

  > Gli sviluppatori fanno affidamento su messaggi di errore ben concepiti quando stanno cercando di risolvere il problema dopo che l'applicazione che hanno costruito usando la vostra API viene utilizzata dai loro utenti.

- \_Note: Mantenere i messaggi di eccezzione di sicurezza più generici possibile. Ad esempio invece di 'password errata' utilizzare 'utente o password errati' in modo che l'utente non possa dedurre che il nome utente sia corretto e la sola password sia sbagliata.

- Usare questi codici di stato per inviare i vostri codici di risposta per descrivere che **tutto ha funzionato**
  L'**app client ha fatto qualcosa di errato** oppure l'**API ha fatto qualcosa di errato**.

      _Quali sono:_
      > `200 OK` la risposta rappresenta un successo per le richieste `GET`, `PUT` o `POST`.

      > `201 Created` quando viene creata una nuova istanza. Quando si crea una nuova istanza usando un metodo  `POST` ritornare il codice di stato `201`.

      > `204 No Content` la risposta rappresenta un successo ma non c'è contenuto da inviare con la risposta. Usarlo quando una operazione di `DELETE` ha successo.

      > `304 Not Modified` la risposta è per minimizzare la info da trasferire quando il ricevente ne ha già una rappresentazione in cache.

      > `400 Bad Request` quando la richiesta non viene elaborata, come se il server non potesse capire cosa gli è stato chiesto.

      > `401 Unauthorized` quando la richiesta è priva di credenziali e dovrebbe essere riproposta con le credenziali richieste.

      > `403 Forbidden` il server ha compreso la richiesta ma si rifiuta di autorizzarla.

      > `404 Not Found` la risorsa richiesta non esiste

      > `500 Internal Server Error` la richiesta è valida, ma il server non può esaudirla a causa di condizioni inaspettate.

      _Perchè:_
      > La magggior parte dei fornitori di API usa un piccolo sottoinsieme di codici di stato HTTP. Ad esempio l'api di Google GData usa solo 10 codici di stato, Netflix ne usa 9 e Digg solo 8. Naturalmente queste risposte contentgono un corpo con info aggiuntive. Ci sono oltre 70 codici di stato HTTP. In ogni caso la maggior parte degli sviluppatori non li ha tutti memorizzati. Quindi quando scegliete codici di stato che non sono molto comuni obbligherete gli sviluppatori dell'applicazione a consultare wikipedia per scoprire cosa state cercando di dirgli. [leggi di più...](https://apigee.com/about/blog/technology/restful-api-design-what-about-errors)

- Fornite il numero totale di risposte nella vostra risposta.
- Accettate parametri di `limit` (limite) e `offset` (scostamento).

- Il volume di dati che la risorsa espone dovrebbe essere tenuto in considerazione. Il consumatore dell'API non sempre necessita della piena rappresentazione di una risorsa. Usate un parametro di specifica di campi che riceve un elenco separato da virgola di campi che devono essere inclusi nella risposta:

```
GET /student?fields=id,name,age,class
```

- Paginazioni, filtri, e oridnamento non devono essere supportati dall'inizio per tutte le risorse. Documentate quali risorse offrono filtro e ordinamento.

<a name="api-security"></a>

### 9.2 Sicurezza delle API

Ci sono alcune migliori pratiche consigliate

- Non usate l'autenticazione basica a meno di avere una connessione sicura (HTTPS). I token di autenticazione non devono esser trasmessi nell'URL: `GET /users/123?token=asdf....`

  _Perchè:_

  > I token o utente e password sono passati attraverso la rete in chiato (codificato in base64, che è una codifica reversibile), lo schema di autenticazione basica non è sicuro. [leggi di più...](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

- I token devono essere trasmessi usando l'intestazione di autorizzazione per ogni richiesta: `Authorization: Bearer xxxxxx, Extra yyyyy`.

- I codici di autorizzazione dovrebbero avere vita breve.

- Rifiutate qualsiasi richiesta non-TLS non rispondendo per evitare qualunque scambio di dati non sicuro. Rispondete a queste richieste con `403 Forbidden`.

- Considerate l'adozione di limiti di utilizzo

  _Perchè:_

  > Per proteggere le vostre API sia da minacce di bot che da qualcuno che chiami la vostra API migliaia di volte all'ora. Dovreste considerare l'implementazione di limiti di utilizzo ai primi stadi dello sviluppo.

- L'appropriata impostazione delle intestazioni HTTP può aiutare a isolare e mettere in sicurezza la vostra applicazione web. [leggi di più...](https://github.com/helmetjs/helmet)

- La vostra API dovrebbe convertire i dati ricevuti nella loro forma canonica o rifiutarli. Ritornate un `400 Bad Request` con dettagli circa gli errori per dati non corretti o mancanti.

- Tutti i dati scambiati con API REST devono essere validati dall'API.

- Serializzare il vostro JSON.

  _Perchè:_

  > Una preoccupazione chiave con i codificatori JSON è la prevenzione dell'esecuzione di codice Javascript arbitrario all'interno del browser oppure, se state usando node.js, sul server. E' vitale utilizzare un appropriato serializzatore JSON per codificare correttamente i dati forniti dall'utente per prevenire l'esecuzione di input fornito dall'utente nel browser.

- Validate il content-type e per lo più utilizzate l'intestazione Content-Type `application/*json` .

  _Perchè:_

  > Ad esempio accettando il tipo mime `application/x-www-form-urlencoded` consente a un attaccante di creare un form e scatenare una semplice richiesta POST. Il server non dovrebbe mai dare per scontato il Content-Type. La mancanza di una intestazione Content-Type oppure una non attesa dovrebbe risultare in un rifiuto del contenuto da parte del server con una risposta `4XX`.

- Verificate il "Progetto per la Lista delle Verifiche per la Sicurezza delle API". [leggi di più...](https://github.com/shieldfy/API-Security-Checklist)

<a name="api-documentation"></a>

### 9.3 Documentazione delle API

- Riempite la sezione `API Reference` in [README.md template](./README.sample.md) per l'API.
- Descrivete i metodi di autenticazione dell'API con un esempio di codice.
- Spiegate la struttura dell'URL (solo il percorso, non la radice dell'URL) includendo il tipo di richiesta (metodo).

Per ogni _endpoint_ spiegate:

- I parametri per l'URL, se esistono; specificateli in base al nome citato nella sezione URL:

  ```
  Richiesto: id=[integer]
  Opzionale: photo_id=[alphanumeric]
  ```

- Se il tipo di richiesta è POST, fornite esempi funzionanti. Le regole per i parametri di URL si applicano anche qui. Separate le sezioni tra Richiesto e Opzionale.

- Risposte di successo. Quale dovrebbe essere il codice di stato e ci sono anche dei dati da ritornare? Questo è utile quando a qualcuno occorre sapere cosa dovrebbero aspettarsi i propri callback:

  ```
  Code: 200
  Content: { id : 12 }
  ```

- Risposte di errore, la maggior parte degli _endpoint_ hanno molti modi per fallire. Da accesso non autorizzato a parametri errati ecc. Tutto ciò dovrebbe essere documentato qui. Può sembrare ripetitivo, ma aiuta ad evitare che vengano fatte delle assunzioni. Ad esempio

  ```json
  {
    "code": 401,
    "message": "Authentication failed",
    "description": "Invalid username or password"
  }
  ```

- Usate strumenti di sviluppo di API. Se ne sono molti open source con buona documentazione tipo [API Blueprint](https://apiblueprint.org/) e [Swagger](https://swagger.io/).

<a name="licensing"></a>

## 10. Licenza

![Licensing](/images/licensing.png)

Assicuratevi di usare risorse per la quali avete il diritto di usarle. Se utilizzate librerie, ricordate di cercare se licenza MIT, Apache o BSD ma se le modificate, verificate anche i dettagli della licenza. Le immagini o i video sottoposte a copyright potrebbero causarvi problemi legali.

---

Sorgenti:
[RisingStack Engineering](https://blog.risingstack.com/),
[Mozilla Developer Network](https://developer.mozilla.org/),
[Heroku Dev Center](https://devcenter.heroku.com),
[Airbnb/javascript](https://github.com/airbnb/javascript),
[Atlassian Git tutorials](https://www.atlassian.com/git/tutorials),
[Apigee](https://apigee.com/about/blog),
[Wishtack](https://blog.wishtack.com)

Icone di [icons8](https://icons8.com/)
