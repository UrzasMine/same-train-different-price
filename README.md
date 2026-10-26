# Same train, different price

**Daily fare comparison of European railway ticket sellers – open data (CC BY 4.0)**

🌐 Results per route: **https://sametrain.eu** · 📄 Citable dataset: [10.5281/zenodo.23669337](https://doi.org/10.5281/zenodo.23669337)
· Mirrors: [GitHub](https://github.com/UrzasMine/same-train-different-price) · [Kaggle](https://www.kaggle.com/datasets/ferdinandfrey/european-train-fares-same-train)

Many European trains can be booked on several websites: the railway of the departure country, the railway of the
destination country, sometimes a transit railway or a reseller. This dataset records every day whether they charge
the same for the same train – and how that depends on how far ahead you book.

**Current version 2026-10-18:** 3,114 rows · 1,542 train offers compared ·
76 routes · 12 sellers · checks from 2026-10-13 to 2026-10-18
(travel dates 2026-10-13 – 2026-11-29).

## Data

[`data/vergleich.csv`](data/vergleich.csv) – one row per train and seller, **semicolon-separated, decimal comma**, UTF-8.

```python
import pandas as pd
df = pd.read_csv("data/vergleich.csv", sep=";", decimal=",")
```

| Column | Meaning |
|---|---|
| reise_datum | travel date |
| nr, strecke | route number and name (German city names, e.g. "München - Wien") |
| abfahrt, ankunft | departure / arrival of the matched train |
| zuege | train numbers, if shown |
| verkaeufer | seller website (e.g. bahn.de, oebb.at) |
| rolle | seller's role: Start/Ziel = railway of origin/destination country, Durchgang = transit country, kein Bezug = unrelated, Vermittler = reseller (Omio) |
| abfahrt_verkaeufer, ankunft_verkaeufer | times as shown by this seller |
| preis, waehrung | fare in local currency |
| preis_eur | fare in EUR (ECB reference rate of the check date) |
| tarif | fare name or note, if any |
| billigster_eur | cheapest fare among all sellers for this train |
| aufschlag_prozent | markup over the cheapest seller, in % |
| verdacht | "ja" if the highest fare is at least 3× the lowest (worth checking) |
| vorlauf_tage | booking lead time: days between check and travel |
| paarung | how the train was matched: "gleiche Zeiten" (same times) or "gleiche Zugnummer" (same train number) |

The file contains only checks that are at least 7 days old; current fares are shown on the website.

## Sellers

| Seller | Country |
|---|---|
| SNCB International (b-europe.com) | BE |
| Deutsche Bahn (bahn.de) | DE |
| České dráhy | CZ |
| Elron | EE |
| Eurostar | FR/BE/NL/DE/UK |
| PKP Intercity | PL |
| LTG Link | LT |
| MÁV | HU |
| ÖBB | AT |
| Omio (reseller) | – |
| SBB | CH |
| Trenitalia | IT |

Not included: SNCF Connect, Trainline and Rail Europe (captcha or access block), NS International, SŽ, ZSSK and
others (no suitable online search or blocked automated access). Bot protection is never bypassed.

## Method

- Every day an automated browser opens each seller's website and searches the same connections, as a person would.
- Travel dates: 1, 7, 21 and 42 days ahead; trains from 8 a.m.
- Stored: the lowest fare shown for 1 adult, 2nd class, no discount card.
- Same train = same departure and arrival time, or same train number with times at most 15–20 min apart.
- Only fares checked on the same day are compared.
- CHF, CZK, PLN → EUR at the ECB reference rate of the check date.
- Excluded: fares valid only for part of the route, connections without a fare.
- Omio: an estimated service fee (about 6.5 % of the fare, at least €2) is added.
- Sellers unrelated to a route that were never cheaper than the origin/destination railway in 6 checks are dropped
  from that route; routes where all sellers charged the same on 6 travel days are checked on Mondays only.

Full description: https://sametrain.eu/en/methodik.html

## Limitations

The lowest fare can be a saver fare at one seller and a flexible fare at another. Fares change constantly; each
row is a snapshot. Booking fees charged at payment are not included for railways. Discount cards are not taken into
account. No guarantee of accuracy.

## License and citation

Data: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Please credit
“Train Price Comparison (sametrain.eu)” with a link to https://sametrain.eu, or cite:

> Ferdinand Frey (2026). *Same train, different price: daily fare comparison of European railway sellers* (Version 2026-10-18) [Data set]. Zenodo. https://doi.org/10.5281/zenodo.23669337

University project, Data Analysis course, Vilnius University, 2026. Feedback: https://sametrain.eu/en/feedback.html

---

**Deutsch:** Täglicher Vergleich der Preise verschiedener Bahn-Verkäufer für denselben Zug am selben Abfragetag
(1 Erwachsener, 2. Klasse, ohne Ermäßigung). Auswertung pro Strecke auf https://sametrain.eu/de/index.html,
Daten unter CC BY 4.0 mit Quellenangabe „Zugpreis-Vergleich (sametrain.eu)“ und Link.
