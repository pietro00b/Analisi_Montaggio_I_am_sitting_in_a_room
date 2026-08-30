# I am sitting in a room di Alvin Lucier — tesina e realizzazione acusmatica

Pietro Barale — Conservatorio A. Casella, L'Aquila
Corsi Accademici di Musica Elettronica DCSL34
Esame di *Analisi, Esecuzione e Interpretazione della Musica Elettroacustica* — 05/07/2025

> **Leggi prima questo file.** Il `README.md` alla radice del repository **non è scritto a mano**:
> viene rigenerato automaticamente a ogni push da `.github/scripts/build-tesina.py`, che concatena
> le sezioni di `docs/sezioni/`. Qualsiasi modifica fatta direttamente a `README.md` viene persa.

## Cos'è questo repository

Contiene due cose:

1. **La tesina**, scritta in Markdown e compilata in PDF da una GitHub Action (Pandoc + XeLaTeX).
2. **La patch Pure Data** con cui ho realizzato la mia versione acusmatica del brano.

Non contiene i PDF delle fonti né i file audio: vedi le sezioni *Fonti* e *Audio* più sotto.

## Come si modifica la tesina

Si scrive **solo** dentro `docs/sezioni/`. Un file per capitolo, ordinati alfabeticamente dal
prefisso numerico:

| File | Contenuto |
|---|---|
| `RIASSUNTO.md` | Abstract. Finisce nel frontmatter, non è un capitolo. |
| `01-NOTE-BIOGRAFICHE.md` | Lucier, contesto storico, *Music for Solo Performer*, *Vespers* |
| `02-I-AM-SITTING-IN-A-ROOM.md` | Genesi, partitura verbale, configurazioni tecniche, materiale |
| `03-FEEDBACK-E-TRASFORMAZIONE.md` | Formalizzazione IIR, processo autonomo, voce/spazio |
| `04-INTERPRETE-E-ASCOLTATORE.md` | Ruolo dell'interprete, la mia realizzazione, il limite formale |
| `98-CONCLUSIONE.md` | Conclusione |

Il numero `98` serve a tenere la conclusione in fondo lasciando spazio per capitoli intermedi.

I metadati (titolo, autore, corso, data, indice) stanno in `metadata.yaml`.
Lo stile tipografico e quello bibliografico stanno in `styles/` — non li ho toccati, vengono dal
template ufficiale `consAq-tesineTemplate`.

## Come si ottiene il PDF

**Automaticamente:** a ogni push su `main` la GitHub Action `Build Tesina PDF` compila il documento,
lo pubblica come artifact e su GitHub Pages, e ricommitta il `README.md` assemblato.

**In locale**, servono `pandoc`, `pandoc-crossref`, `xelatex` e `python3` con `pyyaml`:

```sh
python3 .github/scripts/build-tesina.py
pandoc --defaults styles/defaults.yml README.md -o tesina.pdf
```

## Bibliografia

Il sistema usa **multibib**: ogni file `.bib` in `docs/` genera una sezione separata nel PDF.
Il collegamento è dato dalle **prime tre lettere del nome del file**, non dal campo `keywords`:

- `docs/bibliografia.bib` → sezione BIBLIOGRAFIA, div `#refs-bib`
- `docs/sitografia.bib` → sezione SITOGRAFIA, div `#refs-sit`

Aggiungere un file `discografia.bib` creerebbe automaticamente una terza sezione: questa tesina non
ne ha bisogno, quindi il file non esiste.

Nel testo si cita con `[@chiave]` o `[@chiave, p. 17]`. La corrispondenza tra chiavi e riferimenti
completi è in [`fonti.md`](fonti.md).

## Fonti

**I PDF delle fonti non stanno in questo repository.** Sono materiale protetto da copyright e
duplicherebbero l'archivio personale, che è la fonte unica. [`fonti.md`](fonti.md) elenca i
riferimenti bibliografici completi: nell'archivio i file sono nominati `cognome_anno_titolo`, quindi
il riferimento basta a ritrovarli.

## La realizzazione acusmatica

`realizzazione/i-am-sitting_pietro-barale.pd` è la patch Pure Data con cui ho implementato il
processo di Lucier in tempo reale.

Catena del segnale:

```
adc~ 1  →  *~ (inLevel)  →  delwrite~ memBuff 15000
                                    ↓
                            delread~ memBuff 15000
                                    ↓
                              *~ (fBack)  →  dac~ 1  +  dac~ 9
```

- `loadbang` inizializza `inLevel 1` e `fBack 0.7`.
- Il buffer di `delwrite~`/`delread~` è di **15000 ms** (l'argomento di `delwrite~` è in
  millisecondi, non in campioni): è il tempo di ciclo `k` dell'equazione IIR discussa nel capitolo 3.
- `fBack 0.7` è il coefficiente `A`: sotto la soglia di accumulazione, sopra quella di estinzione.
- Due `env~` pilotano i VU di ingresso e uscita per sorvegliare la saturazione durante il processo.
- `dac~ 9` è il canale di monitoraggio separato usato in studio.

Procedimento seguito: tre stanze, e in ciascuna distanze microfono-altoparlante di 1, 2 e 3 metri.
Di ogni esecuzione ho tenuto solo le ultime generazioni, quelle in cui il parlato è già
irriconoscibile. Il brano finale è il montaggio sovrapposto dei tre esiti.

## Audio e progetto Reaper — non versionati

Il montaggio è stato fatto in Reaper. Sessione, backup e render occupano circa 620 MB: sono esclusi
da git (`.gitignore`) perché superano i limiti pratici di un repository GitHub.

I file si trovano in `~/Documents/Conservatorio_Aquila/Lucier/`:

| File | Cos'è |
|---|---|
| `IamSitting_acusmatico.RPP` + `IamSitting/` | sessione Reaper del montaggio, con Media e Backups |
| `Pietro_Barale_IamSitting_montaggio.wav` | render consegnato |
| `IamSitting_montaggio.wav`, `IamSitting_montaggio_2.wav` | versioni intermedie del montaggio |
| `01-250307_2350_strech120-40.wav` | materiale di lavoro, stretch temporale |
| `lucier_2.wav` | registrazione di processo |
| `Lucier_PietroBarale.pdf` | il PDF consegnato all'esame, versione originale in LaTeX |

Per rigenerare il materiale da zero servono solo la patch Pd, una stanza, un microfono e un
altoparlante: è il senso stesso del brano. Il montaggio specifico invece non è riproducibile,
dipende dalle tre stanze usate nel 2025.

## Stato

Consegnato ed esaminato il 05/07/2025. Il repository serve da archivio e da base per eventuali
riprese del lavoro; la tesina è da considerarsi chiusa.
