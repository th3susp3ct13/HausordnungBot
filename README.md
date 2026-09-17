# Hausordnung-Bot

Ein Rasa-Chatbot, der Fragen zur **Hausordnung der Schulen des BFI Wien** beantwortet – gedacht zum Einbinden auf der Schulwebsite. Aufgebaut nach dem Muster von [ChatbotMakerspace](https://github.com/weberi/ChatbotMakerspace).

Alle Antworten stammen direkt aus `Hausordnung.pdf` und sind in [`domain.yml`](domain.yml) hinterlegt. Die Trainingsbeispiele (Fragen, wie sie echte Nutzer:innen stellen könnten) liegen in [`data/nlu.yml`](data/nlu.yml), die Zuordnung Frage → Antwort in [`data/rules.yml`](data/rules.yml).

Abgedeckte Themen: Verhalten/Höflichkeit, gemeinsame Sprache, Anwesenheitspflicht, Entschuldigungsgründe, Pünktlichkeit, Schulgebäude verlassen, Handys/verbotene Gegenstände, Rauchen, Inventarschäden, Verschmutzung, Feueralarm, Funktionsräume, Computerraumordnung, Essen im Unterricht, Turnsaal, Klassenzimmer aufräumen, Wertgegenstände, Konsequenzen und Wiedergutmachung.

## 1. Bot starten (empfohlen: GitHub Codespaces)

1. Repo auf GitHub anlegen und diesen Ordner pushen (siehe unten).
2. Auf GitHub **Code → Codespaces → Create codespace on main** klicken. Der Codespace richtet automatisch Python 3.10 + Rasa 3.6.20 + Node-RED ein (siehe `.devcontainer/devcontainer.json`).
3. Im Terminal des Codespaces:

   ```bash
   rasa train
   rasa run --port 5005 --cors "*"
   ```

4. Codespaces macht Port 5005 automatisch öffentlich erreichbar (Tab **Ports**). Die dort angezeigte URL brauchst du für `index.html`.

### Lokal starten (Alternative)

Rasa 3.6 benötigt **Python 3.8–3.10** (dein System hat 3.11 – dafür z. B. [pyenv](https://github.com/pyenv-win/pyenv-win) oder Docker mit Python 3.10 verwenden):

```bash
python -m venv .venv
.venv\Scripts\activate
pip install rasa==3.6.20
rasa train
rasa run --port 5005 --cors "*"
```

## 2. Bot testen

```bash
rasa shell
```

oder zum Nachtrainieren/Ausprobieren einzelner NLU-Beispiele: `rasa interactive`.

## 3. Auf der Schulwebsite einbinden

[`index.html`](index.html) enthält ein fertiges Chat-Widget (Chatroom.js). Trage dort deine echte Server-URL ein:

```js
host: "https://DEINE-CODESPACE-URL-5005.preview.app.github.dev",
```

Danach kannst du entweder:
- den Inhalt von `index.html` in eine bestehende Seite der Schulwebsite kopieren, oder
- die Datei als eigene Unterseite (z. B. `/chatbot.html`) hochladen und dort verlinken.

**Wichtig:** Ein GitHub-Codespace pausiert nach Inaktivität – für den dauerhaften Betrieb auf der echten Schulwebsite solltet ihr den Rasa-Server später auf einem echten Server/Hosting laufen lassen (z. B. via Docker), nicht dauerhaft in Codespaces.

## 4. Antworten erweitern/ändern

- Neue Frage-Themen: Intent in `domain.yml` (unter `intents` und `responses`) ergänzen, Beispielsätze in `data/nlu.yml`, Regel in `data/rules.yml`.
- Danach neu trainieren: `rasa train`.

## 5. Node-RED (optional)

`package.json` enthält Node-RED mit dem Rasa-Actionserver-Node, falls ihr später dynamische Antworten (z. B. Live-Daten) über Custom Actions einbauen wollt. Für den reinen FAQ-Bot ist das **nicht nötig** – `rasa run` allein reicht aus.
