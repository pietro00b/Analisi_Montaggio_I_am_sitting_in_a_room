# I am sitting in a room di Alvin Lucier

**Il processo come materiale compositivo**

Pietro Barale — Conservatorio A. Casella, L'Aquila
Corsi Accademici di Musica Elettronica DCSL34
Analisi, Esecuzione e Interpretazione della Musica Elettroacustica

## Il PDF

Ultima versione compilata: <https://pietro00b.github.io/Analisi_Montaggio_I_am_sitting_in_a_room/latest.pdf>

Versione consegnata all'esame del 05/07/2025:
[`output/Lucier_Pietro_Barale.pdf`](output/Lucier_Pietro_Barale.pdf)


## Archivio Audio

I file audio sono disponibili nella cartella [audio](audio/Audio.md).

## Come funziona il repository

- `docs/sezioni/` — i capitoli in Markdown, concatenati in ordine alfabetico.
  `RIASSUNTO.md` finisce nell'abstract, gli altri nel corpo del testo.
- `docs/*.bib` — bibliografie. Le prime 3 lettere del nome del file danno
  l'id della sezione (`bibliografia.bib` → `#refs-bib`).
- `metadata.yaml` — titolo, autore, dati d'esame, impostazioni del documento.
- `styles/` — template LaTeX, `defaults.yml` di pandoc, filtro `multibib.lua`.
- `documento.md` — **generato dalla CI**, non modificarlo a mano.

Questo README è scritto a mano: la CI non lo tocca.

Ad ogni push su `main`, GitHub Actions assembla `documento.md`, compila il PDF
con pandoc + XeLaTeX e lo pubblica su GitHub Pages.
