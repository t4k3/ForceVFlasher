# ForceV Flasher

Aggiornamento firmware delle stazioni ForceV (pedane + encoder, ESP32-C6) **via
browser**, senza installare nulla — stesso sistema del BallgunFlasher.

**➡️ https://t4k3.github.io/ForceVFlasher/**

## Per i collaboratori

1. Apri il link con **Chrome** o **Edge** (Safari/Firefox non supportano Web Serial)
2. Collega la stazione col cavo USB-C e accendila
3. **Connetti** → scegli la porta ("USB JTAG/serial debug unit")
4. **Aggiorna firmware** → aspetta il 100% → la stazione riparte da sola
5. Ripeti per ogni stazione: **master e slave vanno aggiornate tutte**

L'aggiornamento **non tocca** ruolo, tipo, nome e calibrazioni (restano in NVS):
a differenza del BallgunFlasher qui non viene fatto l'erase totale, si scrivono
solo le partizioni firmware.

Se la connessione fallisce: tieni premuto il tasto **BOOT** mentre colleghi il
cavo, poi riprova Connetti.

## Contenuto

- `index.html` — la pagina flasher (esptool-js via Web Serial, flash a 921600 baud).
  Flasha la versione puntata da `FW_VERSION` (attualmente **5.1.0**).
- `5.1.0/` — **firmware corrente** di James, ESP32-C6, ESP-IDF **6.1**,
  protocollo **V5**: la pagina flasha questo. Riferimento usato da Force27.
  ⚠️ La linea 5.x cambia il protocollo radio interno rispetto alla 4.2.x:
  **aggiornare master e slave nella stessa sessione** (versioni miste non
  comunicano). Compatibile con Force27 e con ForceV ≥ 5.8.67.
- `5.0.0/`, `5.0.1/`, `5.0.2/`, `5.0.3/`, `5.0.6/`, `5.0.7/` — versioni precedenti della linea 5.x, tenute
  per poter ripuntare `FW_VERSION` in caso di regressione.
- `4.2.2/` — known-good congelata (collaudo di campo 18/07/2026), tenuta come
  scialuppa: per tornare indietro basta ripuntare `FW_VERSION` in index.html.
- Layout binari: `bootloader.bin` (0x0), `partition-table.bin` (0x8000), `forcev_fw.bin` (0x10000)

## Provenienza della 5.1.0

- Sorgenti originali di James: `forcedeck_fw_esp_ide`, ramo `main`,
  commit `1328f1f6b03e181eb2e51676d40e541a874310c8`.
- Compilazione pulita con ESP-IDF `v6.1`, configurazione `sdkconfig` del commit,
  flash 4 MB, DIO, 80 MHz. Nessuna modifica ai sorgenti firmware.
- Hash dei tre binari in `5.1.0/SHA256SUMS`.
- Verifica di versione incorporata, chip, checksum e tabella delle partizioni;
  NVS esclusa dalle scritture, `eraseAll: false`.
- Questi controlli verificano il pacchetto pubblicato; non costituiscono un nuovo
  collaudo fisico delle stazioni o del collegamento BLE.

## Per aggiornare il firmware pubblicato

1. Compila in una directory pulita dal [repo sorgente di James](https://gitlab.com/dobecorp/work/clients/takeoff/forcedeck/forcedeck_fw_esp_ide) (privato), usando la versione ESP-IDF del firmware
2. Crea la cartella della nuova versione con i 3 `.bin` (rinomina `app-template.bin` → `forcev_fw.bin`)
3. Aggiorna `FW_VERSION` e il badge in `index.html`
4. Registra e verifica gli hash dei binari, versione incorporata, chip e offset
5. Commit + push: GitHub Pages si aggiorna da solo (la pagina ha il cache-busting)
6. Verifica la versione sul sito pubblico e gli hash dei tre download prima di considerare completata la pubblicazione
