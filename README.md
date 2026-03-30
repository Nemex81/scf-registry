# SCF Registry

Indice centralizzato dei pacchetti **SPARK Code Framework (SCF)**.

Questo repository contiene `registry.json` — il catalogo di tutti i pacchetti SCF
pubblici disponibili per l'installazione tramite il server MCP `spark-framework-engine`.

---

## Come funziona

Il server MCP `spark-framework-engine` legge questo file via HTTP GET:

```
https://raw.githubusercontent.com/Nemex81/scf-registry/main/registry.json
```

Quando un utente esegue il tool `scf_install_package`, il motore:
1. Scarica `registry.json` da questo repo
2. Trova il pacchetto richiesto e il suo `repo_url`
3. Scarica i file `.github/` dal repo del pacchetto
4. Li copia nel workspace locale dell'utente
5. Aggiorna `.github/.scf-manifest.json` con gli hash SHA-256

---

## Schema registry.json

```json
{
  "schema_version": "1.0",
  "updated_at": "2026-03-30T12:00:00Z",
  "packages": [
    {
      "id": "scf-pack-gamedev",
      "repo_url": "https://github.com/Nemex81/scf-pack-gamedev",
      "latest_version": "1.0.0",
      "description": "Pacchetto dominio per sviluppo videogiochi",
      "engine_min_version": "1.0.0",
      "status": "active",
      "tags": ["gamedev", "pygame", "accessibilità"]
    }
  ]
}
```

### Campi pacchetto

| Campo | Tipo | Obbligatorio | Descrizione |
|---|---|---|---|
| `id` | string | sì | Identificatore univoco del pacchetto |
| `repo_url` | string | sì | URL GitHub del repo `scf-pack-*` |
| `latest_version` | string semver | sì | Versione più recente disponibile |
| `description` | string | sì | Descrizione breve del dominio |
| `engine_min_version` | string semver | sì | Versione minima del motore richiesta |
| `status` | string | sì | `active` o `deprecated` |
| `tags` | array string | no | Tag di categoria per ricerca |

---

## Pacchetti disponibili

Nessun pacchetto disponibile al momento. I primi pacchetti `scf-pack-*` sono in sviluppo.

---

## Aggiungere un pacchetto

Per aggiungere un pacchetto al registry:
1. Crea un repo `scf-pack-{dominio}` con la struttura `.github/` completa
2. Apri una PR su questo repo aggiungendo la voce in `packages[]` di `registry.json`
3. Aggiorna `updated_at` con la data corrente in formato ISO 8601 UTC

---

*SPARK Code Framework — Livello 1 infrastruttura*
