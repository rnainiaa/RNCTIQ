<div align="center">

# RNCTIQ
<p align="center">

<a href="https://github.com/rnainiaa/RNCTIQ/releases/tag/v1.0.0">
    <img src="https://img.shields.io/badge/Download-v1.0.0-success?style=for-the-badge&logo=github">
</a>

<img src="https://img.shields.io/badge/Platform-Windows-lightgrey?style=for-the-badge">

</p>
**Cyber Investigation & Intelligence Platform**

*Trace. Correlate. Investigate.*

Turn a lone IP address or domain name into a
**documented, timestamped and defensible investigation file** — without leaving a single interface.

![Version](https://img.shields.io/badge/version-1.0.0-00b4d8)
![Platform](https://img.shields.io/badge/platform-Windows%20x64-0078d6)
![Usage](https://img.shields.io/badge/usage-professional-lightgrey)
![No install](https://img.shields.io/badge/install-none-3fb950)
![No API key](https://img.shields.io/badge/API%20key-optional-3fb950)
![UI language](https://img.shields.io/badge/interface-French-e3b341)

**English** · [Français](README.md)

</div>

> **⚠️ The application interface is in French.**
> RNCTIQ was built for French-speaking investigators: menus, labels, reports and
> the built-in manual are all in French. There is no language switch. This
> English guide explains every screen so a non-French speaker can follow along —
> each screenshot caption gives the French labels and their meaning. Command-line
> options and identifiers are in English.

---

## Table of contents

- [What it does](#what-it-does)
- [Who it is for](#who-it-is-for)
- [Getting started in 3 minutes](#getting-started-in-3-minutes)
- [Illustrated user guide](#illustrated-user-guide)
  - [1. Sign in](#1-sign-in)
  - [2. Dashboard](#2-dashboard)
  - [3. Analysing an IP address](#3-analysing-an-ip-address)
  - [4. Analysing a domain name](#4-analysing-a-domain-name)
  - [5. Fingerprints: finding the wider infrastructure](#5-fingerprints-finding-the-wider-infrastructure)
  - [6. The investigation graph](#6-the-investigation-graph)
  - [7. The case file](#7-the-case-file)
  - [8. Continuous monitoring](#8-continuous-monitoring)
  - [9. Indicators of compromise](#9-indicators-of-compromise)
  - [10. Reports](#10-reports)
  - [11. Settings](#11-settings)
- [How to read a risk score](#how-to-read-a-risk-score)
- [Command-line mode](#command-line-mode)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Legal framework and ethics](#legal-framework-and-ethics)
- [Frequently asked questions](#frequently-asked-questions)
- [French–English glossary](#frenchenglish-glossary)

---

## What it does

You receive a suspicious IP address or a phishing domain. Today, characterising
it means opening a dozen tabs — WHOIS here, DNS there, reputation somewhere
else, certificates on a fourth service — then copying the results by hand into a
report.

**RNCTIQ does all of that from a single input**, keeps every result timestamped,
links cases to one another and produces the final report.

| Without RNCTIQ | With RNCTIQ |
|---|---|
| Ten tabs, ten syntaxes, manual copying | **One input**, one reasoned verdict |
| "Source X says it's malicious" | **8 weighted dimensions**, every indicator shown with its weight |
| You block the reported domain, the rest survives | **Fingerprint pivots**: the full cluster appears |
| Undated screenshots | **Timestamped archiving** + SHA-256 chain of custody |
| Separate spreadsheets, accidental overlap | **Automatic detection** of shared infrastructure |
| Hours of writing | **PDF/HTML report in one second** |

---

## Who it is for

| Role | Primary use |
|---|---|
| **Cybercrime investigator** | Build a reproducible, dated case file that holds up in court |
| **SOC analyst** | Qualify an alert in one minute: background scanner, TOR exit node or real C2? |
| **Threat intelligence analyst** | Map adversary infrastructure beyond the initial indicator |
| **DFIR team** | Extract and qualify indicators from logs in bulk |

---

## Getting started in 3 minutes

RNCTIQ is a **standalone executable**. No Python, no dependencies, no
administrator rights required.

```
1. Download RNCTIQ.exe from the Releases section
2. Put it in a dedicated folder (e.g. D:\Investigations\RNCTIQ\)
3. Double-click it
```

> **Important** — place the `.exe` in a folder you can write to. On first launch,
> RNCTIQ creates its working folders next to itself: database, logs, generated
> reports. Avoid `C:\Program Files\` and OneDrive-synced Desktop folders.

**First-run credentials:**

```
Username: admin
Password: ChangeMe!2024#CI
```

⚠️ **Change this password immediately**: *Sécurité → Gérer les utilisateurs*
(Security → Manage users).

### Check that everything is fine

```powershell
RNCTIQ.exe --check
```

This prints the state of dependencies, the file locations in use and the
available sources. Run it first whenever something looks wrong.

### Do I need API keys?

**No.** RNCTIQ works out of the box using public sources: **RDAP** (official
registries), **DNS**, **TLS** (the certificate served by the host) and
**crt.sh** (Certificate Transparency).

Keys for VirusTotal, AbuseIPDB, Shodan, GreyNoise, OTX, Censys or abuse.ch
*enrich* reputation data — they are not required for the tool to work.

---

## Illustrated user guide

> Every screenshot below comes from the real application, run against harmless
> public targets (`8.8.8.8`, `iana.org`) and **with no API keys configured**.
> This is exactly what you will see on first launch.
>
> The interface is in French. Each section lists the French labels alongside
> their English meaning, and a full [glossary](#frenchenglish-glossary) is
> provided at the end.

### 1. Sign in

![RNCTIQ sign-in screen](screenshots/01-connexion.png)

**What you are looking at** — The authentication screen, with an optional
checkbox to unlock the API key vault at sign-in.
*French labels:* `Utilisateur` = username · `Mot de passe` = password ·
`Se connecter` = sign in · `Quitter` = quit.

**What this feature does** — Authentication is not cosmetic: it underpins the
**audit trail**. Every analysis, scan and export is logged with the identity of
its author and a timestamp.

**How to read it** — Four roles govern permissions:

| Role | Permissions |
|---|---|
| `admin` | Everything, including user accounts and settings |
| `investigator` | Cases, analyses, authorised scans, reports |
| `analyst` | Analyses and reports — **no active scanning** |
| `viewer` | Read-only |

**Why this matters** — If your work is challenged, the audit trail shows who did
what and when. It protects the investigator as much as it binds them. Sessions
lock automatically after inactivity (`Ctrl+L` to lock manually).

---

### 2. Dashboard

![Dashboard](screenshots/02-tableau-de-bord.png)

**What you are looking at** — Ten headline figures, three charts (threat level
breakdown, 14-day activity, target types) and a table of the latest analyses
with their verdicts.
*French labels:* `Analyses totales` = total analyses · `IOC critiques` = critical
IOCs · `Enquêtes ouvertes` = open cases · `Score moyen` = average score ·
`Recoupements` = cross-case overlaps · `Sources actives` = active sources.

**What this feature does** — It aggregates the state of your entire local
database: stored analyses, critical IOCs, open cases, pending monitoring alerts
and OSINT source availability.

**How to read it** — "Sources actives 2/11" means only the key-free sources are
responding. "Recoupements 0" means no shared infrastructure has been detected
between your cases yet.

**Why this matters** — This is your morning entry point: what changed overnight,
what needs a decision, and whether your tooling is healthy.

---

### 3. Analysing an IP address

**How to use it** — *Analyse IP* tab (`Ctrl+1`), type the address, click
**Analyser** (Analyse).

![IP analysis - summary](screenshots/03-analyse-ip.png)

**What you are looking at** — A "Verdict" banner: the target, the level
(`FAIBLE` = low), the classification, the score out of 100 with its gauge, and a
one-line summary. Below it, the full network identity sheet.
*French labels:* `Cible à analyser` = target to analyse · `Synthèse` = summary ·
`Fiabilité` = confidence · `Réseau d'entreprise` = corporate network.

**What this feature does** — It queries RDAP registries, reverse DNS, the local
GeoIP database and any configured reputation sources, then consolidates
everything.

**How to read it** — "1 source · fiabilité 41 %" is **as important as the
score**: only one source answered here. A low score with low confidence means
"nothing found", **not** "harmless".

**Why this matters** — The ASN and CIDR block identify the operator actually
responsible — the one to serve a legal request on. That is often the most
actionable item on the sheet.

#### VPN / proxy / TOR detection

![Infrastructure and anonymisation](screenshots/04-infrastructure-vpn-tor.png)

**What you are looking at** — Four cards (detected type, provider, confidence,
anonymisation) followed by the numbered list of **indicators that led to the
verdict**.
*French labels:* `Type détecté` = detected type · `Fournisseur` = provider ·
`Confiance` = confidence · `Anonymisation` = anonymisation · `NON` = no.

**What this feature does** — It classifies the address into one of 16
infrastructure categories: hosting provider, commercial VPN, proxy, TOR node,
bulletproof host, corporate network, mobile, academic, and so on.

**How to read it** — Confidence matters more than the label. At 65 % the
evidence is solid. Below 40 %, treat the classification as **a hypothesis to
verify**.

**Why this matters** — Behind a VPN, the address no longer identifies a person
but **a service shared by thousands of users**. Knowing this changes what comes
next: you will need a legal request, not an inference.

---

### 4. Analysing a domain name

**How to use it** — *Analyse de domaine* tab (`Ctrl+2`). Three checkboxes
control depth: use cache, enumerate subdomains, active resolution.

![Domain analysis - summary](screenshots/05-analyse-domaine.png)

**What you are looking at** — Domain age, registrar, A records, subdomain count,
email security posture and certificate validity — followed by findings and
recommendations.
*French labels:* `Âge du domaine` = domain age · `Sous-domaines` = subdomains ·
`Sécurité mail` = email security · `Certificat` = certificate · `Valide` = valid ·
`Constatations principales` = key findings · `Recommandations` = recommendations.

**What this feature does** — It cross-references WHOIS/RDAP, all DNS records,
the TLS certificate and Certificate Transparency in a single pass.

**How to read it** — **Age is the single most discriminating signal.** 11,391
days indicates an established institution. A 3-day-old domain impersonating a
bank is a major red flag.

**Why this matters** — The recommendations are methodological: here, keep a
timestamped copy of the records, because this data is volatile and constitutes
evidence.

#### DNS and email security

![DNS and email security](screenshots/06-dns-securite-mail.png)

**What you are looking at** — All records on the left (A, AAAA, MX, TXT, NS,
SOA, CAA). On the right, an assessment of SPF, DNSSEC, DMARC and DKIM with a
risk level per mechanism.
*French labels:* `Enregistrements DNS` = DNS records · `Sécurité de la
messagerie` = email security · `Mécanisme` = mechanism · `État` = status ·
`Évaluation` = assessment · `Moyen` = medium · `Faible` = low.

**What this feature does** — It collects the DNS zone then **assesses
anti-spoofing posture**: a domain with no SPF or DMARC can be impersonated by
anyone in an email.

**How to read it** — "DMARC en mode surveillance uniquement (p=none)" means the
policy exists but **blocks nothing**. Worth reporting, but do not confuse it
with a total absence.

**Why this matters** — For a SOC, this answers "could this email have been
legitimately spoofed?". For an investigator, an **unusually low TTL** betrays
infrastructure designed to move fast.

#### TLS certificate

![TLS certificate](screenshots/07-certificat-tls.png)

**What you are looking at** — Common name, organisation, issuing authority,
validity dates, duration, days remaining, expiry and self-signing flags.
*French labels:* `Autorité émettrice` = issuing CA · `Valide du/jusqu'au` = valid
from/until · `Jours restants` = days remaining · `Auto-signé` = self-signed.

**What this feature does** — It retrieves the certificate actually served by the
host and extracts its subject alternative names (SANs) and fingerprints.

**How to read it** — A certificate **issued hours before the incident**,
self-signed, or covering dozens of unrelated domains, is a strong signal.

**Why this matters** — The certificate is the **most reliable pivot** in the
whole investigation: publicly logged, timestamped by an independent third party,
and hard to backdate.

#### Subdomains

![Discovered subdomains](screenshots/08-sous-domaines.png)

**What you are looking at** — The list of subdomains, their resolved IP
addresses, their status (`Actif` = active / `Inactif` = inactive) and the
discovery source.

**What this feature does** — It queries Certificate Transparency and any
configured OSINT sources. **No connection to the target** is required: discovery
is entirely passive.

**How to read it** — The **inactive ones are valuable**: they reveal names
prepared then abandoned — or not yet activated. An inactive
`secure-payment.domain` announces the next phase of a campaign.

**Why this matters** — The real footprint of an infrastructure almost always
exceeds the reported domain. **Blocking only the known domain leaves the
campaign operational.**

---

### 5. Fingerprints: finding the wider infrastructure

A criminal changes IP in an hour and domain in a day. They change their
**certificate, name servers or TLS configuration** far less often. That is where
continuity lives.

![Fingerprints and pivots](screenshots/09-empreintes-pivots.png)

**What you are looking at** — A table of fingerprints (type, value, reliability,
source, what they can find) and, below it, **ready-to-copy queries** for Shodan,
Censys and crt.sh.
*French labels:* `Empreintes collectées` = collected fingerprints ·
`Fiabilité` = reliability · `Permet de retrouver` = can be used to find ·
`Exploitable` = actionable · `Requêtes de recherche inversée` = reverse-search
queries · `Rechercher les domaines liés` = search for linked domains.

**What this feature does** — It extracts reusable pivots: certificate
fingerprint, serial number, covered names, name servers, mail servers,
registrant email, JARM and favicon hash.

**How to read it** — The **reliability** column ranks them: "Très fiable" (very
reliable) for a dedicated name server, lower for a shared host used by millions
of sites — where the pivot proves nothing.

**Why this matters** — This is the step from **one indicator to an
infrastructure**. The "Rechercher les domaines liés" button queries crt.sh
**with no API key at all**.

---

### 6. The investigation graph

**How to use it** — *Graphe* tab, pick the source (last analysis, active case,
or all stored relations), then **Construire le graphe** (build the graph).

![Graph view](screenshots/10-graphe.png)

**What you are looking at** — The entity graph, a statistics panel (nodes,
relations, components, density), the **pivot nodes** table with their
interpretation, and a legend by type and risk level.
*French labels:* `Nœuds` = nodes · `Relations` = edges · `Densité` = density ·
`Nœuds pivots` = pivot nodes · `Légende` = legend · `Aperçu statique` = static
preview.

**What this feature does** — It links domains, subdomains, IPs, ASNs, network
blocks, certificates, DNS and mail servers, fingerprints and cases into a
navigable network.

**How to read it** — Pivot nodes are ranked **by investigative value**, not by
edge count: a certificate shared by 3 domains says more than an ASN shared by
300.

**Why this matters** — A 52-row table cannot be narrated; a graph can be shown.
It is **the exhibit that explains the infrastructure** to a judge or a board in
thirty seconds.

**Available exports** — PNG, PDF, interactive HTML, GraphML (Gephi), JSON.

![Graph PNG export](screenshots/11-graphe-export.png)

*PNG export ready to drop into a report: shapes and colours by entity type,
outline by risk level.*

---

### 7. The case file

**How to use it** — `Ctrl+N` to open a case. It becomes the active case:
**every subsequent analysis is attached to it automatically**.

![Cases view](screenshots/12-enquetes.png)

**What you are looking at** — The case list on the left; on the right the
auto-generated reference, status, priority, TLP classification, keywords,
statement of facts and five counters.
*French labels:* `Enquêtes` = cases · `Nouvelle` = new · `Référence` = reference ·
`Intitulé` = title · `Priorité` = priority · `Exposé des faits` = statement of
facts · `Preuves` = evidence · `Définir comme enquête active` = set as active case.

**What this feature does** — It aggregates everything related to the case:
indicators, analyses, notes, **evidence with SHA-256 hashes**, timeline and
cross-case overlaps.

**How to read it** — The **(ACTIVE)** marker in the title tells you where your
next analyses will be filed. The TLP classification governs report distribution.

**Why this matters** — Without a case, an analysis is a lost file. With one, it
becomes **an exhibit attached to a procedure**, timestamped and attributed.

#### Cross-case overlaps

![Cross-case overlaps](screenshots/13-recoupements.png)

**What you are looking at** — One case shares infrastructure with another:
common indicators, indirect links, **confidence level**, and the detail of the
shared elements.
*French labels:* `Enquêtes liées` = linked cases · `IOC communs` = shared IOCs ·
`Liens indirects` = indirect links · `Confiance` = confidence ·
`Détail du recoupement` = overlap detail · `Indicateurs identiques` = identical
indicators.

**What this feature does** — It compares the case against **all others** on two
levels: strictly identical indicators, and shared infrastructure (same
certificate, same registrant, same name server).

**How to read it** — The second level is the valuable one: it connects cases
**that no exact-value search would ever link**. Above 70 % confidence, the match
warrants a formal exchange.

**Why this matters** — This is the answer to siloing. Two investigators working
the same campaign without knowing it are put in touch. **A strong overlap can
justify joining two procedures.**

#### Target history

![Target history](screenshots/14-historique.png)

**What you are looking at** — How long the target has been tracked, the table of
**detected changes** with severity, then a timeline of each attribute: value,
status, validity period, number of observations.
*French labels:* `Changements détectés` = detected changes · `Ancienne/Nouvelle
valeur` = old/new value · `Gravité` = severity · `Chronologie des attributs` =
attribute timeline · `Depuis` = since · `Jusqu'au` = until.

**What this feature does** — On each analysis, the tool compares the observed
state with the previous one and **dates every value**. It also feeds the
"behavioural" dimension of the risk score.

**How to read it** — The *Depuis* and *Jusqu'au* columns answer the decisive
question: **"what was the state of this infrastructure at the time of the
offence?"** — not what it looks like today.

**Why this matters** — This data is volatile: **what is not captured is lost for
good**. A registrant who changes three days after the incident can only be
established through historisation.

---

### 8. Continuous monitoring

This moves the tool **from reactive to proactive**: instead of re-running an
analysis by hand, you place a target under watch and get alerted when something
moves.

![Continuous monitoring](screenshots/15-surveillance.png)

**What you are looking at** — Watched targets, their checks, frequency, status,
next check and alert count. Below, the alert log.
*French labels:* `Surveillance continue` = continuous monitoring ·
`Cibles surveillées` = watched targets · `Périodicité` = frequency ·
`Dernier/Prochain contrôle` = last/next check · `Alertes` = alerts ·
`Démarrer la surveillance` = start monitoring.

**What this feature does** — **Ten periodic checks**: A/AAAA resolution, name
servers, mail servers, registrant, certificate, page content, ports, reputation,
availability, reverse DNS.

**How to read it** — A change classified as **critical triggers a full automatic
re-analysis**. The case updates itself, even if nobody has opened it for weeks.

**Why this matters** — Without monitoring, you learn that a dormant domain went
live again **from the next victim**.

![Monitoring check selection](screenshots/16-surveillance-controles.png)

**Each check states what it detects**, not merely what it measures: "change of
DNS servers: domain takeover, judicial seizure or transfer". The alert arrives
already interpreted.

> **Passive by default** — checks grouped under `Contrôles passifs` (passive
> checks) make **no connection to the target**. Active checks (certificate, page
> content, ports) live in a separate section and **require explicit
> authorisation**.

---

### 9. Indicators of compromise

#### Automatic extraction from free text

**How to use it** — *Outils → Extraire les IOC d'un texte…* (Tools → Extract
IOCs from text).

![Automatic IOC extraction](screenshots/17-extraction-ioc.png)

**What you are looking at** — Free text pasted at the top (email, report, log);
at the bottom, the extracted and typed indicators: IPs, domains, URLs, emails,
hashes, ASNs.
*French labels:* `Extraire` = extract · `Extraire et ajouter à l'enquête active`
= extract and add to the active case.

**What this feature does** — It recognises the **defanged forms** used by CERTs —
`hxxp://`, `1[.]2[.]3[.]4`, `example(.)com` — and refangs them before extraction.

**How to read it** — Documentation ranges (RFC 5737) and private addresses are
**deliberately discarded**: they would pollute the database without ever
designating a real target.

**Why this matters** — This is the most immediate time saver: a three-page
report becomes a list of qualified IOCs **in one operation**, with no
transcription errors.

#### Indicator database

![Indicator database](screenshots/18-base-indicateurs.png)

**What you are looking at** — Each indicator with its type, value, threat level,
score, confidence, source, parent case and observation date.
*French labels:* `Base d'indicateurs` = indicator database · `Menace` = threat ·
`Confiance` = confidence · `Vu le` = seen on · `Importer` = import.

**What this feature does** — It centralises IOCs from **all** cases and exports
them as JSON, CSV, **MISP**, **STIX 2.1**, Suricata rules or a blocklist.

**How to read it** — The **Enquête** (case) column is the overlap signal: the
same indicator present in two distinct cases is a lead in itself.

**Why this matters** — This is the bridge to operations: the blocklist goes to
the firewall, the MISP export goes to the community, **with no re-typing**.
Double-clicking re-runs the analysis.

#### Global search

![Global search](screenshots/19-recherche-globale.png)

**What you are looking at** — A search box and three tabs: cases, archived
analyses, indicators.
*French labels:* `Recherche globale` = global search · `Analyses archivées` =
archived analyses · `Ouvrir la sélection` = open selection.

**What this feature does** — It searches references, targets, verdicts and even
the **body of stored analyses**. Double-clicking opens the result in the right
place.

**How to read it** — Opening an **archived analysis** shows it as it was: the
tool does not re-run it. Re-running would produce a different result and consume
API quota.

**Why this matters** — "Have we seen this IP before?" is answered **in three
seconds** — including on a case handled by a colleague six months ago.

---

### 10. Reports

**How to use it** — From an analysis or a case, click **Rapport PDF** or
**Rapport HTML**.

<table>
<tr>
<td width="50%"><img src="screenshots/21-rapport-pdf-garde.png" alt="PDF report cover page"></td>
<td width="50%"><img src="screenshots/22-rapport-pdf-corps.png" alt="PDF report body"></td>
</tr>
</table>

**What you are looking at** — A paginated document, header and footer on every
page, a **TLP** classification banner, numbered sections and standardised tables.

**What this feature does** — It composes the report from the database: case
sheet, statement of facts, IOCs, analyses, timeline, graph, justified risk level
and recommendations.

**How to read it** — The disclaimer on the cover page is not boilerplate: it
states that conclusions are **hypotheses supported by open sources**, to be
corroborated before any legal qualification.

**Why this matters** — This is the deliverable a judge, a CISO or a client will
read. It is produced **in one second** and is identical from one analyst to the
next: quality no longer depends on the writer.

---

### 11. Settings

![Settings](screenshots/23-configuration.png)

**What you are looking at** — The secret vault (to be created on first launch),
one field per OSINT source with a "get a key" link, and the **connector status**
table: key required, key configured, rate limit.
*French labels:* `Coffre-fort de secrets` = secret vault · `Créer le coffre-fort`
= create the vault · `Clé requise` = key required · `Configurée` = configured ·
`Limite de débit` = rate limit · `Enregistrer` = save.

**What this feature does** — It centralises configuration: sources, network,
scoring thresholds, security and scanning, local intelligence (GeoLite2
databases), plugins and the **audit trail**.

**How to read it** — `crt.sh` and RDAP show "clé requise : Non" (key required:
no) — these are the two sources that work **immediately, with no account**.

**Why this matters** — Keys are stored **AES-encrypted**, never in plaintext in
a config file. The displayed rate limit stops you burning a quota mid-incident.

### Built-in documentation

![Built-in documentation](screenshots/24-documentation.png)

The **Investigator's Manual** is embedded in the executable: clickable table of
contents, full-text search, adjustable text size. Reach it via *Aide → Manuel de
l'enquêteur* (Help → Investigator's manual). **This manual is in French.**

---

## How to read a risk score

The score out of 100 aggregates **eight weighted dimensions**:

| Dimension | Weight | What it measures |
|---|---|---|
| Reputation | **28 %** | VirusTotal, AbuseIPDB, OTX, GreyNoise, abuse.ch |
| Infrastructure | **18 %** | VPN, proxy, bulletproof, fast-flux |
| Exposure | **14 %** | Open ports, vulnerable services |
| DNS hygiene | **9 %** | SPF, DMARC, DNSSEC |
| Age / volatility | **9 %** | Recently created domain, low TTL, churn |
| TLS posture | **9 %** | Expired or self-signed certificate |
| Behaviour | **8 %** | Changes observed over time |
| Context | **5 %** | Tags and internal lists |

**Reading thresholds:**

| Score | Level (French label) |
|---|---|
| 0 – 25 | 🟢 Low — `FAIBLE` |
| 26 – 50 | 🟡 Medium — `MOYEN` |
| 51 – 75 | 🟠 High — `ÉLEVÉ` |
| 76 – 100 | 🔴 Critical — `CRITIQUE` |

These bounds are **adjustable** in *Configuration → Scoring*: a bank fraud unit
and a national CERT do not share the same tolerance.

> ⚠️ **A score of 92/100 means "converging and strongly unfavourable evidence",
> not "guilt established".** The tool does not conclude; it documents.

---

## Command-line mode

`RNCTIQ.exe` is scriptable — useful for automation and SIEM integration.
**Command-line options and output keys are in English.**

```powershell
# Analyse an IP address
RNCTIQ.exe --cli ip 45.33.32.156

# Analyse a domain and produce a PDF report
RNCTIQ.exe --cli domain example.com --report pdf

# Auto-detect the target type, JSON output, offline
RNCTIQ.exe --cli auto suspect.tk --json --offline

# Save the result to a file
RNCTIQ.exe --cli ip 45.33.32.156 --output result.json

# Environment diagnostics
RNCTIQ.exe --check

# Monitoring: run one cycle then exit (schedule it)
RNCTIQ.exe --monitor --no-gui

# List registered watches
RNCTIQ.exe --list-watches
```

### Exit codes

| Code | Meaning |
|---|---|
| `0` | Low or medium threat (score < 51) |
| `1` | Runtime error |
| `2` | High or critical threat (score ≥ 51) |

Example use in a script:

```powershell
RNCTIQ.exe --cli auto $target --quiet
if ($LASTEXITCODE -eq 2) { Write-Host "High-risk target: escalation required" }
```

### Main options

| Option | Effect |
|---|---|
| `--cli {ip,domain,auto}` | Analyse without the graphical interface |
| `--json` | Full JSON output |
| `--output FILE` | Write the JSON result to a file |
| `--report {pdf,html}` | Generate a report |
| `--offline` | No network requests, cache only |
| `--no-save` | Do not persist to the database |
| `--analyst NAME` | Name recorded in the audit trail |
| `--bruteforce` | Active resolution of common subdomains |
| `--quiet` | Hide progress output |
| `--check` | Verify the environment |
| `--version` | Print the version |

---

## Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+1` | Go to IP analysis |
| `Ctrl+2` | Go to domain analysis |
| `Ctrl+Shift+A` | Quick analysis (top bar) |
| `Ctrl+N` | New case |
| `Ctrl+F` | Global search |
| `Ctrl+G` | Build the graph |
| `Ctrl+I` | Import IOCs |
| `Ctrl+E` | Export IOCs (JSON) |
| `Ctrl+S` | Save settings |
| `Ctrl+L` | Lock the session |
| `Ctrl+Q` | Quit |

---

## Legal framework and ethics

RNCTIQ applies one simple rule: **any function that opens a connection to the
target is disabled by default** and requires explicit authorisation, recorded in
the audit trail.

![Active scan legal warning](screenshots/20-scan-actif-avertissement.png)

**What you are looking at** — A warning banner citing the applicable statutes, a
checkbox reading "Je certifie disposer d'une autorisation légale" (I certify
that I hold legal authorisation) and a red action button.

**What this feature does** — The scan stays **inoperative until the box is
ticked**. The authorisation, its stated reason and the operator's identity are
written to the audit trail.

**How to read it** — Red marks the actions that are **observable by the target**,
as opposed to passive collection, which is the bulk of the tool.

**Why this matters** — If challenged, the audit trail shows the act was
**authorised, justified and attributed**.

### Passive versus active

| Passive — default | Active — on authorisation |
|---|---|
| RDAP, DNS, Certificate Transparency, third-party sources | Port scanning, JARM, favicon, direct TLS, HTTP content |
| Leaves no trace on the target | Observable by the target |

### Three reminders shown inside the tool

- **IP geolocation** gives the *declared location of the infrastructure*, never
  a person's position.
- Behind a **VPN**, the address identifies a shared service, not an author.
- A **high score** is a body of evidence, not proof.

> Port scanning a third-party system without prior written authorisation may
> constitute a criminal offence (French Penal Code art. 323-1 et seq., the UK
> Computer Misuse Act, the US CFAA). Use this function only on systems you own,
> or under a judicial warrant or a penetration-testing contract.

---

## Frequently asked questions

<details>
<summary><b>Can I switch the interface to English?</b></summary>

<br>

No. RNCTIQ has no internationalisation layer: the interface, reports and
built-in manual are French-only, and there is no language setting.

This guide is designed to bridge that gap — every screen is explained with its
French labels — and the [glossary](#frenchenglish-glossary) covers the recurring
terms. Command-line options, JSON output keys and exported formats (MISP,
STIX 2.1, CSV) are in English, so **automation and data exchange are unaffected
by the interface language**.

</details>

<details>
<summary><b>Does my data leave my machine?</b></summary>

<br>

No. The database is **local** (SQLite by default), and API keys sit in an
**AES-encrypted vault** on your machine. The only outbound traffic consists of
the OSINT lookups **you trigger** (RDAP, DNS, crt.sh, plus any source you have
configured a key for).

The `--offline` mode disables all network requests and works from cache.

</details>

<details>
<summary><b>My antivirus flags the file — is that expected?</b></summary>

<br>

Yes, this is a classic **heuristic false positive** for PyInstaller
executables: the self-extraction at startup resembles, to a heuristic engine,
the behaviour of a malicious packer.

UPX compression is deliberately disabled because it makes the problem
noticeably worse.

</details>

<details>
<summary><b>The first launch is slow — why?</b></summary>

<br>

A standalone executable unpacks itself into memory at startup. Expect a few
seconds on the first run, faster afterwards. RNCTIQ also creates its working
folders and database on first execution.

</details>

<details>
<summary><b>Where are my cases stored?</b></summary>

<br>

In a folder created **next to the executable**. Run `RNCTIQ.exe --check` to
print the exact locations in use.

To back up your cases, copy the `data/` folder. To start fresh, delete it — the
application will recreate it.

</details>

<details>
<summary><b>Can I use it without API keys?</b></summary>

<br>

Yes, and that is the default. RDAP, DNS, TLS and crt.sh cover full
infrastructure analysis: network identity, ASN, WHOIS, DNS records,
certificates, subdomains and pivots.

Keys add **multi-source reputation** (VirusTotal, AbuseIPDB, GreyNoise, OTX,
Shodan, Censys, abuse.ch). All of them offer a free tier.

</details>

<details>
<summary><b>Can I change the risk thresholds?</b></summary>

<br>

Yes: *Configuration → Scoring*. You can adjust the low/medium/high bounds and
**the weighting of all eight dimensions**. Values are saved to a readable
configuration file.

</details>

<details>
<summary><b>Can a team use it together?</b></summary>

<br>

The executable is single-workstation by design. For team use, the database can
be switched from SQLite to **PostgreSQL** in the settings: cases, indicators and
overlaps then become shared, while each analyst keeps their own account and
named audit trail.

</details>

<details>
<summary><b>What should I do if an analysis fails?</b></summary>

<br>

1. Run `RNCTIQ.exe --check` to diagnose the environment and sources.
2. Check the log in the `logs/` folder.
3. Some public sources (notably `crt.sh`) return temporary errors under load —
   the tool retries automatically, but trying again a few minutes later resolves
   most cases.

</details>

---

## French–English glossary

Recurring interface terms, in the order you are likely to meet them.

| French | English |
|---|---|
| Tableau de bord | Dashboard |
| Analyse IP / de domaine | IP / domain analysis |
| Enquête · Enquêtes | Case · Cases |
| Cible | Target |
| Analyser | Analyse (run) |
| Synthèse | Summary |
| Verdict | Verdict |
| Menace | Threat |
| Faible · Moyen · Élevé · Critique | Low · Medium · High · Critical |
| Fiabilité · Confiance | Confidence |
| Fournisseur | Provider |
| Hébergeur | Hosting provider |
| Sous-domaines | Subdomains |
| Empreintes | Fingerprints |
| Certificat · Autorité émettrice | Certificate · Issuing CA |
| Serveur de noms | Name server |
| Serveur de messagerie | Mail server |
| Titulaire · Registrar | Registrant · Registrar |
| Nœud · Relations | Node · Edges |
| Recoupements | Cross-case overlaps |
| Liens indirects | Indirect links |
| Historique · Chronologie | History · Timeline |
| Changements détectés | Detected changes |
| Gravité | Severity |
| Surveillance continue | Continuous monitoring |
| Contrôles passifs / actifs | Passive / active checks |
| Périodicité | Frequency |
| Alertes en attente | Pending alerts |
| Indicateurs · IOC | Indicators · IOCs |
| Preuves | Evidence |
| Exposé des faits | Statement of facts |
| Piste d'audit | Audit trail |
| Coffre-fort | Vault |
| Clé requise | Key required |
| Scan actif | Active scan |
| Rapport | Report |
| Enregistrer | Save |
| Actualiser | Refresh |
| Rechercher | Search |
| Fermer · Annuler | Close · Cancel |

---

## Going further

This README covers using the executable. <br>
The **Investigator's Manual** is also embedded in the executable: 
*Aide → Manuel de l'enquêteur* (Help → Investigator's manual).<br>
A **Complete Investigation Example** is also embedded in the executable:
*Aide → Exemple d'enquête complete* (Help → Complete investigation example).<br>
 — **note that these documents are in French**:<br>

| Document | Contenu |
|---|---|
| *Aide → Manuel de l'enquêteur* (Help → Investigator's manual) | Methodology, score interpretation, investigative pivots, legal framework |
| *Aide → Exemple d'enquête complete* (Help → Complete investigation example) | An annotated end-to-end practical case study with all steps and details |

<p align="center">
  <a href="https://github.com/rnainiaa/RNCTIQ/releases/tag/v1.0.0">
    <img src="https://img.shields.io/badge/Download-RNCTIQ%20v1.0.0-blue?style=for-the-badge&logo=github" alt="Download RNCTIQ">
  </a>
</p>

---

<div align="center">
**RNCTIQ** · Cyber Investigation & Intelligence Platform · v1.0.0<br>
Creator of RNCTIQ: Rachid Nainiaa<br>
M. Sc. Cyber Security<br>
contact@rncyber.ca<br>
<br>
*Results are weighted technical indicators.<br>
They are hypotheses to be corroborated, not judicial findings.*

</div>
