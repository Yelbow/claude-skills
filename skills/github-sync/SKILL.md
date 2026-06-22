---
name: github-sync
description: "Sync alle bestanden uit het huidige Claude Project naar de GitHub repo Yelbow/claude-skills. Gebruik deze skill altijd wanneer de gebruiker zegt 'sync naar github', 'push context files', 'sync project', 'zet in github', of vergelijkbaar. Trigger ook aan het einde van een sessie waarin context files zijn aangemaakt of gewijzigd."
---

# GitHub Sync Skill

Pusht alle relevante bestanden uit het huidige project naar de centrale GitHub repo.

---

## Stap 1: Haal GitHub config op

Fetch Google Drive file ID `1lYU--uCLynzvru52H-7gWB70dGRDZX01` via de Google Drive MCP tool (`read_file_content`). Extraheer:
- `github_token` 
- `github_repo` (is `Yelbow/claude-skills`)

---

## Stap 2: Bepaal de projectnaam

Kijk in de systeemprompt of projectinstructies naar de projectnaam. Gebruik die als mapnaam onder `/context/`. Voorbeelden:
- Studio Zonder Meer → `szm`
- Modern Men Thongs → `modernmenthongs`
- WordPress → `wordpress`

Als de projectnaam onduidelijk is, vraag dan één keer: "Welke mapnaam gebruik ik in de repo?"

---

## Stap 3: Verzamel bestanden

Controleer twee bronnen:

**A. Project files in context**
Kijk in de huidige context naar een `project_files` blok of bestanden die zichtbaar zijn in de systeemprompt. Elk bestand dat beschikbaar is in de projectcontext is een kandidaat.

**B. Geüploade bestanden in deze sessie**
Controleer `/mnt/user-data/uploads/` via bash_tool:
```bash
ls /mnt/user-data/uploads/ 2>/dev/null
```

**Filter: push alleen bestanden die voldoen aan minimaal één criterium:**
- Bestandsnaam begint met `context_`
- Bestandsnaam eindigt op `.md`
- Bestandsnaam eindigt op `.txt`
- Bestandsnaam bevat `skill`, `prompt`, `strategie`, of `systeem`

Sla over: afbeeldingen, .zip, .php, .js, .css, binary bestanden.

---

## Stap 4: Push elk bestand naar GitHub

Gebruik bash_tool voor elke push. Herhaal dit patroon per bestand:

```bash
TOKEN="[token uit stap 1]"
REPO="Yelbow/claude-skills"
PROJECT="[projectnaam uit stap 2]"
FILENAME="[bestandsnaam]"
CONTENT="[bestandsinhoud]"

# Controleer of bestand al bestaat (SHA ophalen)
RESPONSE=$(curl -s -H "Authorization: token $TOKEN" \
  "https://api.github.com/repos/$REPO/contents/context/$PROJECT/$FILENAME")

SHA=$(echo "$RESPONSE" | python3 -c "
import sys, json, re
raw = sys.stdin.buffer.read()
cleaned = re.sub(rb'[\x00-\x08\x0b\x0c\x0e-\x1f]', b'', raw)
try:
    d = json.loads(cleaned)
    print(d.get('sha', ''))
except:
    print('')
")

# Bouw payload
python3 - << 'EOF'
import json, base64

content = """[BESTANDSINHOUD HIER]"""
encoded = base64.b64encode(content.encode('utf-8')).decode('ascii')

payload = {
    "message": f"sync: context/$PROJECT/$FILENAME",
    "content": encoded
}

sha = "$SHA"
if sha:
    payload["sha"] = sha

with open('/tmp/payload.json', 'w') as f:
    json.dump(payload, f)
print("payload klaar")
EOF

# Push
curl -s -X PUT \
  -H "Authorization: token $TOKEN" \
  -H "Content-Type: application/json" \
  "https://api.github.com/repos/$REPO/contents/context/$PROJECT/$FILENAME" \
  --data @/tmp/payload.json | python3 -c "
import sys, json
d = json.load(sys.stdin)
if 'content' in d:
    print('OK:', d['content']['name'])
else:
    print('FOUT:', d.get('message', 'onbekend'))
"
```

Verwerk bestanden één voor één. Niet alles tegelijk.

---

## Stap 5: Rapporteer

Na afloop:

```
GitHub sync klaar.

Gepusht (context/[project]/):
- bestand1.md ✓
- bestand2.md ✓

Overgeslagen:
- afbeelding.png (geen tekst)
```

Als er niets te pushen was: "Geen relevante bestanden gevonden in dit project."

---

## Foutafhandeling

- **401 Unauthorized:** token verlopen of ongeldig. Meld dit aan de gebruiker.
- **404 op config fetch:** Drive file niet bereikbaar. Vraag de gebruiker de config te checken.
- **Bestand te groot (>5MB):** overslaan, melden.
- **SHA conflict:** altijd eerst SHA ophalen voor een update. Nooit blind overschrijven.

---

## Naamgeving in repo

| Type | Pad in repo |
|---|---|
| Context files | `/context/[project]/[bestandsnaam]` |
| Skills | `/skills/[skill-naam]/SKILL.md` |
| Prompts | `/context/[project]/[naam]-prompt.md` |

