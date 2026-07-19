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

- `index.html` — la pagina flasher (esptool-js via Web Serial, flash a 921600 baud)
- `5.0.0/` — **firmware corrente** (linea James): la pagina flasha questo.
  ⚠️ La 5.0.0 cambia il protocollo radio interno: **aggiornare master e slave
  nella stessa sessione** (versioni miste non comunicano). Richiede app ≥ 5.8.39.
- `4.2.2/` — known-good congelata (collaudo di campo 18/07/2026), tenuta come
  scialuppa: per tornare indietro basta ripuntare `FW_VERSION` in index.html.
- Layout binari: `bootloader.bin` (0x0), `partition-table.bin` (0x8000), `forcev_fw.bin` (0x10000)

## Per aggiornare il firmware pubblicato

1. Compila dal repo sorgente ([FORCEV_FW](https://github.com/t4k3/FORCEV_FW), privato)
2. Crea la cartella della nuova versione con i 3 `.bin` (rinomina `app-template.bin` → `forcev_fw.bin`)
3. Aggiorna `FW_VERSION` e il badge in `index.html`
4. Commit + push: GitHub Pages si aggiorna da solo (la pagina ha il cache-busting)
