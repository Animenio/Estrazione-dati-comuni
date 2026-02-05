# Prompt chiaro per ChatGPT — piattaforma estrazione dati comuni

Agisci come **Senior Software Engineer + Data Engineer + AI Engineer** esperto in:
- scraping/crawling siti PA italiani
- Google Drive API (OAuth)
- ETL e data quality
- estrazione dati da PDF (testo + OCR locale)
- integrazione API ISTAT
- progettazione pipeline riutilizzabili per team non tecnici

## Obiettivo
Voglio progettare una piattaforma **riutilizzabile dai colleghi** che estragga automaticamente dati sui comuni, partendo da template CSV presenti su Google Drive.

## Fase attuale (obbligatoria)
Per ora devo fare **solo un test sul Comune di Vigone**:
`https://www.comune.vigone.to.it/`

## Input e sorgenti
1. Il programma deve avere accesso al mio Google Drive via **OAuth**.
2. Cartella sorgente corretta: **`dataset_dati_comuni`**.
3. Dentro `dataset_dati_comuni` ci sono:
   - `01_governo.csv`
   - `02_territorio_popolazione.csv`
   - `03_risultati_pillole.csv`
   - `04_servizi_civici.csv`
   - `05_rifiuti.csv`
   - `06_progetti.csv`
   - `guida.md`
4. I CSV **non contengono dati**, ma sono **template/schemi di estrazione**:
   - 01 Governo
   - 02 Territorio e popolazione
   - 03 Risultati in pillole
   - 04 Servizi civici
   - 05 Rifiuti
   - 06 Progetti/PNRR
5. I CSV contengono solo il nome degli indicatori/campi (niente sinonimi, niente formato, niente unità).
   Quindi unità e formato devono essere **inferiti semanticamente**.
6. Fonti da usare:
   - sito del Comune
   - `servizipubblicaamministrazione.it`
   - ISTAT (anche via guida: `https://github.com/ondata/guida-api-istat`)
   - altre fonti istituzionali/autorevoli solo se utili

## Output richiesto
1. Il programma chiede in input:
   - URL del comune
   - anno di riferimento
2. Crea su Drive questa struttura:
   - `Comuni/<nome_comune>/<anno>/`
3. Scarica in quella cartella i documenti trovati (soprattutto PDF, ma considera anche altri formati utili).
4. Compila i 6 CSV mantenendo **la stessa forma originale**.
5. Facoltativo: mostra anche una tabella con bottone “download CSV”.
6. Per ogni campo estratto salva anche:
   - valore
   - link fonte
   - snippet breve di evidenza
   - confidence score (0-1)
7. Se dato non trovato: `null`.

## Regole di estrazione (fondamentali)
1. Cerca prima i dati **definitivi**; se non ci sono, cerca anche i **previsionali**.
2. Se mancano i definitivi, restituisci entrambe le cose quando possibile e segnala chiaramente.
3. Un solo valore finale per campo (best value), con punteggio di confidenza.
4. I documenti senza testo selezionabile:
   - prova OCR locale in modo efficiente
   - se ancora non leggibile, passa oltre e logga il motivo

## Ricerca documenti (metodo desiderato)
1. Per ogni campo da estrarre, usa OpenAI per generare query mirate (Google/altri motori).
2. Le query devono puntare a trovare pagine/documenti pertinenti nelle fonti autorevoli.
3. Priorità: precisione e completezza, ma senza costi elevati.

## Vincoli economici e tecnici
1. Voglio una soluzione **a costo zero o quasi** (open source quando possibile).
2. Devo evitare spese API inutili.
3. PDF mediamente <500, peso contenuto.
4. Il sistema deve avere cache e resume:
   - non riscaricare file già presenti
   - non rianalizzare file già processati
   - riprendere da interruzioni
5. Stato pipeline su JSON in Google Drive.

## UX finale
- Target utenti: colleghi non tecnici.
- Modalità preferita: **Google Colab one-click**.
- Deve elaborare sempre tutti i 6 CSV.

## Cosa voglio da te adesso
Dammi una risposta in 8 sezioni, concrete e operative:

1. **Architettura end-to-end** (componenti, flusso, cartelle, stato, retry).
2. **Algoritmo dettagliato** per estrazione campo-per-campo dai 6 CSV.
3. **Strategia di ricerca fonti** (query generation + ranking + dedup).
4. **Parser documenti** (PDF testuale, OCR fallback, altri formati).
5. **Prompting strategy** per LLM (template prompt robusto + schema output JSON).
6. **Validazione qualità** (confidence, controlli numerici, coerenza anno, gestione null).
7. **Piano MVP su Vigone** (step pratici, checklist, criteri go/no-go).
8. **Stack consigliato low-cost/free**, incluse alternative a pagamento-zero, e librerie/progetti GitHub riusabili.

In più:
- proponi uno **schema dati JSON standard** per i risultati;
- proponi una **struttura repository** pronta per scalare dopo il test;
- includi un elenco di **rischi reali + mitigazioni**;
- chiudi con una **roadmap in 3 fasi**: test Vigone → pilot multi-comune → piattaforma completa.
