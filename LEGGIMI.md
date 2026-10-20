# Casa Libra · sito

Landing page del B&B Casa Libra (Valdicastello Carducci, Pietrasanta), con calendario delle disponibilità sincronizzato da Airbnb e richiesta di prenotazione diretta via email.

## Cosa c'è nella cartella

| File | A cosa serve |
|---|---|
| `index.html` | Il sito (una sola pagina) |
| `assets/img/` | Foto di Serena Rossi, ridimensionate per il web |
| `assets/pittogramma.png` | Logo Libra |
| `data/availability.json` | Date occupate delle 3 camere (si aggiorna da solo) |
| `scripts/sync-ical.mjs` | Legge i calendari Airbnb e aggiorna `availability.json` |
| `.github/workflows/sync-calendar.yml` | Fa girare lo script ogni ora su GitHub |

## Pubblicare su GitHub Pages (una volta sola)

1. Su GitHub, account **libraterracreativa**, crea un nuovo repository pubblico chiamato **`casalibra`**.
2. Carica **tutto il contenuto** di questa cartella, compresa la cartella nascosta `.github` (con "Add file → Upload files" trascina tutto; se `.github` non compare, creala a mano con "Add file → Create new file" scrivendo il percorso `.github/workflows/sync-calendar.yml` e incollando il contenuto).
3. **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main`, cartella `/ (root)` → Save.
4. Dopo un paio di minuti il sito è su **https://libraterracreativa.github.io/casalibra/**

## Collegare i calendari Airbnb (una volta sola)

I link "Esporta calendario" di Airbnb contengono un codice privato: **non vanno mai incollati nei file del sito**, ma solo nei *secrets* di GitHub, che restano nascosti.

1. Nel repository: **Settings → Secrets and variables → Actions → New repository secret**.
2. Crea questi tre secrets, incollando in ognuno il link `.ics` della camera:
   - `ICAL_MARINE` → link della Camera Marine
   - `ICAL_FORESTA` → link della Suite Foresta
   - `ICAL_RELAX` → link della Suite Relax
3. Vai su **Actions → "Sincronizza calendario Airbnb" → Run workflow** per la prima sincronizzazione.
4. Da quel momento il calendario si aggiorna da solo **ogni ora**. Nel sito, sotto il calendario, c'è scritto quando è stato aggiornato l'ultima volta.

Se cambi un link su Airbnb (es. "reimposta link"), aggiorna il secret corrispondente.

## Come funzionano le prenotazioni

- L'ospite sceglie camera e date. Il calendario non lascia selezionare notti già occupate. Il giorno di partenza può coincidere con l'arrivo di un altro ospite.
- La richiesta arriva via email a **librapietrasanta@gmail.com** (servizio Web3Forms, lo stesso del sito eventi), con oggetto tipo *"Richiesta Casa Libra · Suite Foresta · 12 nov – 14 nov 2026"*.
- **La data non viene bloccata in automatico.** Prima di rispondere controlla su Airbnb che sia ancora libera, poi **blocca le date su Airbnb**: così spariscono anche dal calendario del sito alla sincronizzazione successiva. Infine rispondi all'ospite con conferma e link di pagamento.

## Modifiche frequenti

- **Prezzi "da"**: in `index.html` cerca `ROOMS=` (prezzi del calendario) e `room-price` (prezzi mostrati nelle camere).
- **Link agli eventi del mese di Libra**: in `index.html` cambia `LIBRA_EVENTI_URL` in alto (es. `.../Novembre26/`).
- **Numero massimo di ospiti**: in `ROOMS=` (`max`). Ora: suite 4, Marine 2.
