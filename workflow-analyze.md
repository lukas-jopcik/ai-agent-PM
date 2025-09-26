# 🔄 Workflow s úpravami (Max Split + Epic Gate + Memory)

Tento dokument popisuje rozšírený proces, doplnený o:
- **Maximálne rozdelenie** PRD → epiky → stories → subtasks,
- **Epic Gate** – povinné zastavenie a testovanie po každom epiku,
- **Automatické logovanie a pamäť** – systém si vždy pamätá aktuálny stav projektu.

---

## 0. Analyst (pred PRD)
- **Používa systémy:** brainstorming, market research, competitor analysis, deep research, project brief.  
- **Výstup:** `docs/analysis/tema.md` (goals, context, constraints, risks, references).  
- **Log event:** `analysis.created`.

---

## 1. PO (Product Owner)
- **Úloha:** vytvoriť PRD.  
- **Upravené:** shardovanie na maximum epikov → epic = 1–3 stories.  
- **Výstup:** `docs/prd/epic-*`.  
- **Log events:** `prd.created`, `prd.sharded`, `epic.created`.

---

## 2. SM (Scrum Master)
- **Úloha:** vytvoriť user stories.  
- **Upravené:** každý epic → maximum krátkych stories (1–3h).  
- **Story obsahuje:** AC, subtasks (1–2h), mini Test Plan, rollback/flag.  
- **Výstup:** `docs/stories/*`.  
- **Log event:** `story.created`.

---

## 3. Dev (Developer)
- **Úloha:** implementovať stories.  
- **Detail:** kód + testy + dokumentácia (CHANGELOG, README).  
- **Výstup:** implementácia + PR.  
- **Log event:** `impl.merged`.

---

## 4. QA (Quality Assurance)
- **Úloha:** testovať každú story.  
- **Detail:** testy podľa mini Test Planu + regresia.  
- **Výstup:** QA results.  
- **Log event:** `qa.result`.

---

## 5. Epic Gate (PO + QA) – **STOP bod**
- **Povinné zastavenie po epiku:**  
  - Checklist: všetky AC splnené, testy (unit/integration/e2e + regresia), dokumentácia, rollback/flag.  
- **Výstup:** „Epic Gate: APPROVED“ alebo „FAIL → späť Dev“.  
- **Log event:** `epic.gate`.

---

## 6. Release
- **Dev/Ops:** nasadenie s feature flagom alebo rollout.  
- **QA:** monitoring po release.  
- **Výstup:** stabilná funkcionalita.  
- **Log event:** `release`.

---

## 7. Retro
- **Celý tím:** retro po každom epiku.  
- **Výstup:** lessons learned.  
- **Log event:** `retro.logged`.

---

# 🗂️ Memory & State Tracking

## Event Ledger
- Súbor: `docs/state/ledger.ndjson`  
- Každý krok workflowu zapisuje udalosť (`actor`, `event`, `ref`, `from`, `to`, `notes`).  
- Append-only, rotuje sa podľa veľkosti.

## Current State Snapshot
- Súbor: `docs/state/status.json` (strojovo čitateľný) + `docs/state/status.md` (prehľad pre ľudí).  
- Obsahuje: posledné stavy epikov a stories, % progres, ďalšiu akciu.  
- Generuje sa automaticky cez `scripts/rebuild_state.py`.

## Hooky do krokov
- Každý krok po zápise eventu spúšťa rebuild snapshotu (`status.json` + `status.md`).  
- Pri ďalšom dni / reštarte má systém vždy aktuálnu pamäť.

---
