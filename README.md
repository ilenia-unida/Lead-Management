# 📧 Lead Scoring e Follow-up Automatico basato su AI 

**Video di Spiegazione Dettagliata:** [https://youtu.be/EESTTAZ31pI](https://youtu.be/EESTTAZ31pI)

Questo workflow di n8n implementa un sistema avanzato di **Marketing Automation** per la gestione e classificazione automatica dei lead basata sulle risposte email in ingresso.

Il cuore del sistema è l'utilizzo di **Google Gemini** per eseguire un'analisi e uno scoring immediato della risposta ricevuta, identificando i contatti che richiedono un follow-up prioritario.

---

## ⚙️ Architettura del Flusso

Il flusso è interamente dedicato alla gestione di un lead in entrata, dalla classificazione all'azione operativa.

### 1. Innesco e Classificazione AI

| Nodo | Funzione Principale |
| :--- | :--- |
| **`Gmail Trigger`** | Innesca il flusso all'arrivo di una nuova email in una casella di posta monitorata (es. una risposta a una campagna di follow-up). |
| **`Google Gemini Chat Model3`** | Il Large Language Model di Google Gemini (LLM) per l'analisi del testo. |
| **`AI Agent3`** | Agisce da "Scoring Agent": riceve il contenuto dell'email e un System Prompt per classificarlo (es. interessato, non interessato, necessita di approfondimento). |
| **`Structured Output Parser3`** | Si assicura che l'output dell'Agente AI sia un JSON strutturato e affidabile, con il flag di classificazione (es. `is_interessante: true/false`). |
| **`Merge1` / `If1`** | Combina l'analisi e dirama il flusso in base alla classificazione ottenuta (`is_interessante: true/false`). |

### 2. Gestione del Follow-up e Operazioni

Il flusso si divide in due rami principali dopo la classificazione:

#### A. Ramo Lead Interessante (`is_interessante: true`)

Questo ramo gestisce i contatti classificati come prioritari:

* **`Prepara mail2` (Set):** Prepara i dati necessari per l'invio della mail di follow-up (es. template di conferma appuntamento o richiesta di contatto).
* **`Follow-up1` (Gmail):** Invia l'email di follow-up automatica.
* **`Create a ticket` (Zendesk):** Crea immediatamente un ticket di alta priorità nel sistema CRM/Supporto per il tracciamento manuale.
* **`Send message to channel` (Slack):** Invia una notifica in tempo reale su un canale Slack dedicato (`#nuovo-canale`) per avvisare il team commerciale del nuovo lead caldo.

#### B. Ramo Lead Non Interessante (`is_interessante: false`)

* **`Prepara email1` (Set):** Prepara i dati per un'azione diversa (es. log della transazione su un DB a bassa priorità o un'email di disiscrizione/mantenimento).

---

## 🔑 Credenziali Necessarie

Questo workflow richiede la configurazione delle seguenti credenziali in n8n per operare:

1.  **Gmail account** (Gmail Trigger e Follow-up)
2.  **Google Gemini (PaLM) Api account**
3.  **Zendesk account**
4.  **Slack API account**

---

