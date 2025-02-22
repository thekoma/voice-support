![Image](https://github.com/music-assistant/voice-support/blob/main/assets/music-assistant.png?raw=true)

# Riproduzione multimediale tramite Music Assistant con supporto LLM

[![Apri la tua istanza di Home Assistant e mostra la finestra di importazione blueprint con questo blueprint pre-compilato.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fvoice-support%2Fblob%2Fmain%2Fllm-enhanced-local-assist-blueprint%2Fmass_llm_enhanced_assist_blueprint_en.yaml)

### Configurazione

1. È necessario installare un'integrazione LLM. Può essere un'integrazione LLM già esistente. Questa automazione è pensata per essere utilizzata con un LLM Conversation Agent che non ha il permesso di controllare la casa. L'utilizzo con un LLM che ha il controllo della casa può causare errori, come ad esempio il targeting di lettori multimediali che non sono lettori Music Assistant. Se il tuo LLM ha accesso al controllo della casa, [l'opzione 3](../README.md#option-3-script-which-can-be-used-as-a-tool-by-an-llm-integration-like-open-ai-conversation-chatgpt-or-google-generative-ai-gemini) è l'opzione consigliata.
2. Importa il blueprint usando il pulsante qui sopra.
3. Crea l'automazione usando il blueprint. Scegli l'agente LLM dal punto 1. Opzionalmente puoi modificare le impostazioni per la riproduzione di Music Assistant.
4. Modifica le impostazioni di trigger e risposta per tradurle. Le traduzioni sono fornite qui sotto.
5. Opzionalmente, modifica le impostazioni per il prompt e decidi se l'automazione può condividere i nomi delle aree e dei lettori multimediali con l'LLM.
6. Salva l'automazione e assegnale un nome.

#### Traduzioni
|Nome del campo|Suggerimento per la traduzione|
|---|---|
|Conversation trigger sentences|`(riproduci\|ascolta) {query}`|
|Combine word|`e`|
|No target response|`Non è stato possibile determinare dove riprodurre il contenuto e non è stato impostato alcun lettore predefinito`|
|Area response|`<media_info> verrà riprodotto in <area_info>`|
|Player response|`<media_info> verrà riprodotto su <player_info>`|
|Area and Player response|`<media_info> verrà riprodotto in <area_info> e su <player_info>`|

### Impostazioni del Blueprint

#### Obbligatorio

* Seleziona un LLM Conversation Agent da utilizzare con l'automazione. Il blueprint è pensato per essere utilizzato con un conversation agent che non ha accesso al controllo della casa.

#### Opzionale

* Imposta un `Lettore Predefinito` da utilizzare quando non viene menzionato un target nella richiesta e l'area del satellite vocale non può essere utilizzata.
* Modifica l'impostazione per la `Modalità Radio`. Per impostazione predefinita, viene utilizzata l'impostazione `Non fermare la musica` del lettore Music Assistant.
* Copia le traduzioni per i trigger e le risposte nei campi corrispondenti e aggiungi eventuali frasi di trigger aggiuntive.
* Modifica l'impostazione per condividere i nomi delle aree e dei lettori multimediali con l'LLM. Per impostazione predefinita questa opzione è attiva. Se la disattivi, la menzione di un'area o lettore in un comando funzionerà solo se corrisponde esattamente al nome in Home Assistant (non sensibile alle maiuscole/minuscole). Esempio: se in Home Assistant il nome dell'area è _Camera Sophia_, non funzionerà se dici _camera di Sophia_, o se la conversione Speech to Text usa _Sofia_ invece di _Sophia_. Purtroppo gli alias non possono essere utilizzati nell'automazione, quindi si può confrontare solo con i nomi.
* Modifica il prompt utilizzato per l'LLM conversation agent. Non è necessario tradurlo. Questo potrebbe essere necessario per modelli più piccoli, per fornire maggiori dettagli su alcuni punti.

### Utilizzo

Ogni frase deve:

* iniziare con le parole `Riproduci` o `Ascolta` seguite da una descrizione del contenuto che vuoi riprodurre

* opzionalmente essere seguita da un'area o un lettore Music Assistant su cui vuoi riprodurre il contenuto

### Come viene determinato il target per il contenuto

1. Se nel comando vengono menzionate una o più aree e/o uno o più lettori multimediali, e questi possono essere associati a un'area e/o lettore Music Assistant, verranno utilizzati questi. Se non invii i nomi all'LLM e menzioni un'area o lettore che non corrisponde esattamente, verrà data la risposta `No target response`.
2. Se nel comando non viene menzionata alcuna area o lettore, ma può essere determinata un'area in base al dispositivo da cui proviene il comando, verrà utilizzata quest'area se contiene anche un lettore Music Assistant.
3. Se non viene menzionata alcuna area o lettore multimediale, e non può essere determinata in base al dispositivo da cui proviene il comando, il contenuto verrà riprodotto sul `Lettore Predefinito`. Se questo non è impostato, verrà data la risposta `No target response`.

#### Esempi

```
Riproduci le migliori canzoni dei Pink Floyd in cucina
Ascolta Jagged Little Pill nel salotto
Ascolta l'album Greatest Hits di James Taylor in cucina
Riproduci la canzone New Years Day in camera da letto
Riproduci A Hard Days Night di Billy Joel in camera da letto
Ascolta la playlist Classic Rock nello studio
Ascolta BBC Radio 1 in camera da letto
Riproduci l'album Classical Nights sull'altoparlante Sonos della camera
Ascolta il disco Classical Nights sull'altoparlante Sonos della camera
Riproduci canzoni degli U2
Riproduci musica del compositore di Oppenheimer
Riproduci quell'album con il bambino nudo che nuota verso una banconota sulla copertina
``` 