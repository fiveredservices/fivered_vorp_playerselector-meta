Meta-Repository für den **FIVERED PlayerSelector**.

Dieses Repo enthält **keinen Script-Code**, sondern dient ausschließlich dazu,
Versionen und Changelogs bereitzustellen, damit der **VORP Version Checker**
für die Resource `fivered_playerselector` korrekt funktioniert.

# FIVERED.SERVICES PlayerSelector

Bildschirm‑Playerselector für **RedM** / **VORP Inventory**.

`FIVERED PlayerSelector` blendet Marker über nahe Spielern ein und erlaubt es, Zielspieler direkt
auf dem Bildschirm auszuwählen – ideal für „Give“‑/Handel‑Flows im `vorp_inventory`.

Die Resource wurde von **FIVERED.SERVICES** **https://fivered.services** speziell für die Integration in `vorp_inventory`
entwickelt, ist aber auch in anderen Client‑Scripts nutzbar.

---

## Features

- Visuelle Spielerwahl direkt am Charakter (Marker über den Spielern).
- Nahtlose Integration in `vorp_inventory` (Give‑Flow mit Screen‑Selector).
- Fokus‑Handling zwischen Inventory‑NUI und Selector‑NUI.
- Konfigurierbare Reichweite (`Config.PlayerSelectorRange`).
- Event‑API, die auch von anderen Ressourcen genutzt werden kann.

---

## Anforderungen & Kompatibilität

- RedM mit `rdr3` (fxserver).
- VORP Core / VORP Inventory (getestet mit v4.3).
- Resource‑Pfad:
  - `resources/[fivered]/fivered_playerselector`
  - `resources/[VORP]/vorp_inventory`

Der Selector ist als reine **Client‑Resource** gedacht und arbeitet mit den bestehenden
Server‑Events des Inventars zusammen.

---

## Kurzer Überblick über die API

Die API wird über ein Event exportiert:

```lua
TriggerEvent('fr_playerselector:load', function (Selector)
    -- Selector == FR.PlayerSelector
end)
```

Wichtige Methoden von `FR.PlayerSelector`:

- `setRange(val)` – Reichweite in Metern.
- `getRange()` – aktuelle Reichweite.
- `onPlayerSelected(fn)` – Callback, wenn ein Spieler gewählt wurde (`data.id` = Server‑ID).
- `activate()` / `deactivate()` – Selector starten / stoppen.

Wenn der Selector ohne Auswahl geschlossen wird (z. B. ESC), wird zusätzlich das Event
`fr_playerselector:closed` ausgelöst.

---

## Beispiel: Standalone‑Nutzung

```lua
local Selector = nil

TriggerEvent('fr_playerselector:load', function (data)
    Selector = data -- == FR.PlayerSelector
end)

if not Selector then
    return print("FR.PlayerSelector nicht verfügbar")
end

Selector:onPlayerSelected(function (data)
    print("Gewählter Spieler (Server-ID):", data.id)
    Selector:deactivate()
end)

Selector:setRange(10.0)
Selector:activate()
```

---

## Integration in VORP Inventory

Für eine komplette Integration in `resources/[VORP]/vorp_inventory` (inkl. Anpassungen an
`NUIService.lua`, Config‑Werten und Event‑Handling) gibt es eine **separate Installations‑
und Integrationsanleitung** in der Resource selbst:

- Siehe: `install.md`

Die `install.md` enthält:

- genaue Änderungen, die in `NUIService.lua` vorzunehmen sind,
- empfohlene `config.lua`‑Settings,
- Hinweise zur `server.cfg`‑Reihenfolge,
- sowie ein Test‑HowTo für die Give‑Funktion.

---

## Credits

- Entwicklung & Integration: **FIVERED.SERVICES** (https://fivered.services)