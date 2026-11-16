---
name: caveman
description: Caveman mode toggle — terse output, ~75% token savings. /caveman ON/OFF. OFF by default, session start always resets.
---

# Caveman Mode Toggle

Caveman mode = brutal brevity. Same technical accuracy, zero fluff. ~75% fewer output tokens.

## Commands

```
/caveman          → toggle (ON↔OFF)
/caveman on       → aktif et
/caveman off      → kapat
/caveman status   → mevcut durum
```

## Skill Tetiklenince Claude Şunu Yapar

1. `~/.claude/caveman-mode.json` dosyasını oku (mevcut durum)
2. Toggle işlemi yap (on→off veya off→on) — ya da açık komut (on/off)
3. State dosyasını güncelle
4. Kısa onay mesajı döndür

### State Dosyası Yaz

```bash
# ON:
node -e "const fs=require('fs'),os=require('os'),p=require('path');fs.writeFileSync(p.join(os.homedir(),'.claude','caveman-mode.json'),JSON.stringify({active:true,timestamp:new Date().toISOString()}))"

# OFF:
node -e "const fs=require('fs'),os=require('os'),p=require('path');fs.writeFileSync(p.join(os.homedir(),'.claude','caveman-mode.json'),JSON.stringify({active:false,timestamp:new Date().toISOString()}))"
```

### State Oku

```bash
node -e "const fs=require('fs'),os=require('os'),p=require('path');try{const s=JSON.parse(fs.readFileSync(p.join(os.homedir(),'.claude','caveman-mode.json'),'utf8'));console.log('Caveman:',s.active?'ON':'OFF')}catch{console.log('Caveman: OFF')}"
```

## Aktif Olduğunda Davranış

Hook (`caveman-hook.cjs`) her mesajda systemPrompt enjekte eder:

> CAVEMAN MODE ON. Rules:
> - No greetings, no preambles, no "I'll help you with", no "Great question"
> - No summaries at end ("In summary...", "To recap...")
> - No explaining what you're about to do — just do it
> - Terse sentences. Verb-first.
> - Cut all filler: "As you can see", "It's worth noting", "Please note"
> - Technical accuracy: 100%. Verbosity: 0%.
> - Code comments: only if non-obvious WHY
> - Bullet points > paragraphs when listing
> - Onay: "✓" | Red: "✗" | Info: short

## Notlar

- Session başlangıcında ALWAYS OFF
- İş/profesyonel context'te kullanma — `/caveman off` veya yeni session
- `/caveman-commit` ve `/caveman-review` sub-skilleri mevcut değil (ileride eklenebilir)
