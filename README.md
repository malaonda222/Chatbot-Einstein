# Chatbot Barberia — Prenotazione Appuntamenti via Chat

Prototipo di chatbot conversazionale per una barberia, che permette agli utenti di prenotare appuntamenti direttamente da una chat senza dover chiamare o visitare un sito web.

---

## Panoramica

Il chatbot guida l'utente nella scelta del servizio, raccoglie le informazioni necessarie e genera dinamicamente un link di prenotazione personalizzato tramite le API di Calendly.

---

## Stack Tecnologico

### Botpress
Piattaforma scelta per costruire il chatbot. Permette di creare flussi conversazionali visivi senza dover scrivere tutto il codice da zero. E' stata utilizzata la versione gratuita cloud, che include un emulatore per testare il bot in tempo reale e la possibilita' di collegare canali esterni come Telegram.

### Calendly
Servizio di prenotazione integrato nel chatbot. Gestisce automaticamente la disponibilita' del barbiere, gli slot orari, i promemoria e le email di conferma sia per il cliente che per l'host.

Sono stati configurati 4 Event Types distinti:

| Servizio |
|---|
| Shampoo e Taglio |
| Barba Modellata |
| Shampoo Taglio + Barba Modellata |
| Taglio Bambino |

Tramite le API di Calendly, il bot genera dinamicamente un link di prenotazione personalizzato per il servizio scelto dall'utente, invece di utilizzare un link generico.

### Telegram
Canale di messaggistica finale scelto per il prototipo. L'integrazione con Botpress e' nativa, gratuita e non richiede verifiche aziendali. Il bot e' accessibile pubblicamente tramite username e chiunque puo' testarlo cercandolo direttamente su Telegram.

---

## Scelta del Canale: da Twilio a Telegram

Inizialmente è stato scelto **Twilio Sandbox** per simulare WhatsApp, in quanto WhatsApp Business API reale richiede un account Meta Business verificato — un processo lungo e non adatto a un prototipo accademico. Twilio offre un ambiente di test gratuito che simula WhatsApp senza questa verifica.

Durante i vari test è emerso un problema tecnico bloccante: l'integrazione Twilio di Botpress v1.3.1 invia automaticamente un parametro `"typing": true` in ogni messaggio. Questa funzionalita' e' in Public Beta su Twilio e non è supportata dal Sandbox gratuito, che risponde con un errore **HTTP 400** bloccando l'invio di tutti i messaggi. Il problema è stato confermato come bug noto nella repository ufficiale di Twilio su GitHub e non era risolvibile lato configurazione.

Si è quindi deciso di passare a **Telegram**, che ha risolto tutti i problemi:

- Integrazione con Botpress immediata e funzionante
- Completamente gratuita
- Nessun limite giornaliero di messaggi
- Nessuna verifica aziendale richiesta

---

## Funzionalità del Chatbot

Il chatbot è strutturato in un unico workflow principale con tre percorsi:

**Menu principale**
Accoglie l'utente e propone due opzioni: prenotare un appuntamento oppure visualizzare informazioni su prezzi e orari.

**Sezione Informazioni**
Mostra i servizi disponibili con prezzi e orari della barberia, con possibilità di procedere direttamente alla prenotazione, tornare al Menù principale oppure terminare il flusso (tramite selezione 'Esci'). 

**Flusso di Prenotazione**
1. L'utente sceglie il servizio desiderato
2. Il bot raccoglie nome ed email
3. Viene effettuata una chiamata alle API di Calendly per generare un link personalizzato
4. Il link viene inviato all'utente in chat
5. L'utente sceglie data e ora su Calendly e riceve una email di conferma automatica

Dopo la prenotazione l'utente può scegliere se fare un'altra prenotazione (ritorna al nodo Prenotazione), tornare al Menù principale (ritorna al nodo Menu) oppure cliccare su Esci (per terminare il flusso). 

In tutti i nodi è stata implementata la gestione degli errori per input non validi: il bot avvisa l'utente e ripropone la domanda.

---

## Risultato

Il prototipo funziona su Telegram con integrazione reale delle API di Calendly. I link generati sono reali e le prenotazioni vengono effettivamente salvate nel calendario del barbiere.

Puoi testare il bot direttamente su Telegram: [Einstein] [(https://cdn.botpress.cloud/webchat/v3.6/shareable.html?configUrl=https://files.bpcontent.cloud/2026/03/17/15/20260317154148-BV65UTC4.json)]
---

## Tecnologie Utilizzate

- [Botpress](https://botpress.com/) — piattaforma per la costruzione del chatbot
- [Calendly API](https://developer.calendly.com/) — gestione prenotazioni e generazione link dinamici
- [Telegram Bot API](https://core.telegram.org/bots/api) — canale di messaggistica
