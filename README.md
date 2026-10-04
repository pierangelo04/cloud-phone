# Cloud Phone su GitHub Actions

Telefono Android **rootato** on-demand, gratis, senza VPS.

## Come funziona

1. Vai su **Actions → Cloud Phone → Run workflow** (puoi scegliere i minuti e la versione Android).
2. Il workflow installa l'SDK Android, crea l'emulatore, lo avvia con KVM, lo rootta (`adb root`)
   e pubblica lo schermo via browser (noVNC + tunnel Cloudflare).
3. L'URL compare nel **riepilogo del job** (e nei log come `notice`). Aprilo e premi **Connetti**.
4. Il telefono resta acceso per i minuti scelti, poi il job termina (limite GitHub: ~6 ore).

La **cache di GitHub** conserva SDK e AVD tra un avvio e l'altro: la prima volta scarica ~3 GB
e il boot richiede 10-20 minuti, le volte dopo è molto più veloce.

## Setup

1. Crea un repo GitHub e carica questi file (mantieni il percorso `.github/workflows/`).
2. (Opzionale) Per raggiungere il runner anche via Tailscale: aggiungi il secret
   `TAILSCALE_AUTHKEY` (Settings → Secrets → Actions) con una chiave di autenticazione
   Tailscale. Altrimenti lo step viene saltato in automatico.
3. Lancia il workflow da Actions.

## Limiti onesti

- **Max ~6 ore** per sessione, poi devi rilanciarlo a mano.
- **Minuti**: con repo **privato** e piano gratuito hai 2000 minuti/mese di runner Linux
  (~6 sessioni complete da 5,5 ore). Con repo **pubblico** i minuti sono gratis, ma attenzione:
  i log del workflow sono pubblici e quindi anche l'URL del telefono lo è — chiunque potrebbe
  aprirlo mentre è acceso. Per un telefono personale, meglio repo privato.
- **KVM**: i runner Ubuntu lo espongono dal 2023, ma non è garantito ufficialmente.
  Il workflow verifica `/dev/kvm` e si ferma con un errore chiaro se manca.
- **Prestazioni**: i runner standard hanno pochi core — non aspettarti un fulmine,
  ma per un telefono di test/emergenza va bene.
- **Termini di servizio**: GitHub Actions nasce per CI/CD. Un telefono interattivo
  sempre acceso è un uso fuori etichetta: il mining è esplicitamente vietato, questo no,
  ma un abuso evidente potrebbe far flaggare l'account. Usalo con misura.
- **Nessuna persistenza garantita**: a ogni run il telefono riparte (la cache aiuta,
  ma non è uno storage affidabile al 100%).
- Il VNC **non ha password**: la protezione è l'URL casuale del tunnel. Non farci cose sensibili.
