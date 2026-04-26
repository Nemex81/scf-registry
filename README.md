# SCF Registry

Indice centralizzato dei pacchetti **SPARK Code Framework (SCF)**.

Questo repository contiene `registry.json` — il catalogo dei pacchetti SCF
pubblici disponibili per installazione e aggiornamento tramite il server MCP
`spark-framework-engine`.

---

## Come funziona

Il server MCP `spark-framework-engine` legge questo file via HTTP GET:

```text
https://raw.githubusercontent.com/Nemex81/scf-registry/main/registry.json
```

Quando un utente esegue `scf_install_package`, il motore:

1. Scarica `registry.json` da questo repo
2. Trova il pacchetto richiesto e il suo `repo_url`
3. Scarica i file `.github/` dal repo del pacchetto
4. Li copia nel workspace locale dell'utente
5. Aggiorna `.github/.scf-manifest.json` con gli hash SHA-256

Quando un utente esegue `scf_update_packages` o `scf_apply_updates`, il motore usa
lo stesso registry per costruire un piano di update coerente con le dipendenze
dichiarate nei `package-manifest.json` dei package installati.

---

## Schema registry.json

```json
{
  "schema_version": "2.0",
  "updated_at": "2026-04-26T00:00:00Z",
  "packages": [
    {
      "id": "spark-base",
      "repo_url": "https://github.com/Nemex81/spark-base",
      "latest_version": "1.5.0",
      "description": "Layer fondazionale SCF con agenti base, skill condivise, instruction comuni e prompt framework general-purpose.",
      "min_engine_version": "2.4.0",
      "status": "stable",
      "tags": ["base", "foundation", "agents", "skills", "prompts", "general-purpose"]
    },
    {
      "id": "scf-master-codecrafter",
      "repo_url": "https://github.com/Nemex81/scf-master-codecrafter",
      "latest_version": "2.3.0",
      "description": "Plugin CORE-CRAFT sopra spark-base per design, code routing, code UI e contesto MCP.",
      "min_engine_version": "2.4.0",
      "status": "stable",
      "tags": ["master", "core-craft", "dispatcher", "design", "spark-base"]
    },
    {
      "id": "scf-pycode-crafter",
      "repo_url": "https://github.com/Nemex81/scf-pycode-crafter",
      "latest_version": "2.1.0",
      "description": "Pacchetto SCF per progetti Python specializzato sopra scf-master-codecrafter.",
      "min_engine_version": "2.4.0",
      "status": "stable",
      "tags": ["python", "development", "copilot", "agenti"]
    }
  ]
}
```

### Campi pacchetto

| Campo | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| `id` | string | sì | Identificatore univoco del pacchetto |
| `repo_url` | string | sì | URL GitHub del repository del pacchetto |
| `latest_version` | string semver | sì | Versione più recente disponibile |
| `description` | string | sì | Descrizione breve del dominio |
| `min_engine_version` | string semver | sì | Versione minima del motore richiesta |
| `status` | string | sì | `active`, `stable` o `deprecated` |
| `tags` | array string | no | Tag di categoria per ricerca |

---

## Pacchetti disponibili

| Pacchetto | Versione | Descrizione | Status |
| --- | --- | --- | --- |
| [`spark-base`](https://github.com/Nemex81/spark-base) | `1.5.0` | Layer fondazionale SCF con agenti base, skill condivise, instruction e prompt framework | `stable` |
| [`scf-master-codecrafter`](https://github.com/Nemex81/scf-master-codecrafter) | `2.3.0` | Plugin CORE-CRAFT sopra spark-base per design, code routing, code UI e contesto MCP | `stable` |
| [`scf-pycode-crafter`](https://github.com/Nemex81/scf-pycode-crafter) | `2.1.0` | Plugin Python specializzato che richiede scf-master-codecrafter | `stable` |

---

## Schema Version

Il campo `schema_version` di `registry.json` è indipendente dal campo `schema_version`
presente nel file `.github/.scf-manifest.json` dei progetti consumer.
Sono artefatti con strutture e cicli di vita distinti:

| Artefatto | schema_version | Struttura | Scopo |
| --- | --- | --- | --- |
| `registry.json` (questo repo) | `"2.0"` | `packages[]` | Catalogo pubblico dei package SCF disponibili |
| `.github/.scf-manifest.json` (progetto consumer) | `"1.0"` | `entries[]` | Tracking locale dei package installati nel progetto |

I due numeri di versione **non sono confrontabili** e possono evolvere in modo indipendente.
Un bump di `schema_version` in uno schema non implica un cambio nell'altro.

> Il registro viene aggiornato automaticamente tramite il workflow `registry-sync-gateway.yml`
> nel motore `spark-framework-engine` quando un pacchetto pubblica una nuova versione.
> Il workflow aggiorna le versioni e valida lo schema del registry prima di aprire la PR.

---

## Aggiungere un pacchetto

Per aggiungere un pacchetto al registry:

1. Crea un repo package con `package-manifest.json` coerente e struttura `.github/` completa
2. Apri una PR su questo repo aggiungendo la voce in `packages[]` di `registry.json`
3. Aggiorna `updated_at` con la data corrente in formato ISO 8601 UTC

---

## Nota

SPARK Code Framework — Livello 1 infrastruttura.
