# Widerruf-Referenz — Rechtsprechung, DSK-Beschlüsse & Prüfmuster

Lade diese Datei nur wenn Detailwissen zu Rechtsgrundlagen oder Muster-Texten benötigt wird.

---

## Kernrechtliche Grundlagen

### Art. 7(3) DSGVO — Widerruf der Einwilligung (vollständiger Wortlaut)

> „Die betroffene Person hat das Recht, ihre Einwilligung jederzeit zu widerrufen. Durch den Widerruf der Einwilligung wird die Rechtmäßigkeit der aufgrund der Einwilligung bis zum Widerruf erfolgten Verarbeitung nicht berührt. Die betroffene Person wird vor Abgabe der Einwilligung hiervon in Kenntnis gesetzt. Der Widerruf der Einwilligung muss so einfach wie die Erteilung der Einwilligung sein."

**Schlüsselsatz:** „Der Widerruf der Einwilligung muss **so einfach** wie die Erteilung der Einwilligung sein."

### TTDSG § 25 — Schutz der Privatsphäre bei Endeinrichtungen

§ 25 Abs. 1: Einwilligung erforderlich für das Speichern/Auslesen nicht unbedingt erforderlicher Informationen auf Endgeräten.
§ 25 Abs. 2: Ausnahmen (technisch notwendige Cookies).

**Widerruf-Implikation:** Die für das Setzen optionaler Cookies erteilte Einwilligung muss nach TTDSG i.V.m. Art. 7(3) DSGVO jederzeit widerrufbar sein.

---

## DSK-Orientierungshilfen (NICHT im MCP — manuell prüfen)

> ⚠️ Die DSK-Dokumente sind nicht über das `rechtsinformationen-bund-de` MCP abrufbar. 
> Aktuelle Versionen: [datenschutzkonferenz-online.de](https://www.datenschutzkonferenz-online.de/)

**Relevante DSK-Dokumente:**

| Dokument | Inhalt | Widerruf-Relevanz |
|---|---|---|
| DSK Orientierungshilfe Telemedien 2.0 (2021) | Cookie-Einwilligung, Banner-Gestaltung | Ablehnen = gleich einfach wie Akzeptieren |
| DSK Beschluss "Anforderungen an Einwilligungen" (2022) | Symmetrie-Anforderungen für Banner | Gleiche Klickzahl, gleiche Prominenz |
| DSK Beschluss "Pay or OK" (2024) | Bedingte Zugänglichkeit | Widerruf darf nicht zu Zugangsentzug führen ohne Ausweichoption |

**DSK-Kernprinzipien (aus Trainingsdaten — auf datenschutzkonferenz-online.de verifizieren):**

1. **Klick-Symmetrie:** Widerruf ≤ gleich viele Klicks wie ursprüngliche Einwilligung
2. **Visuelle Gleichwertigkeit:** "Alle ablehnen" Button darf optisch nicht schwächer sein als "Alle akzeptieren"
3. **Keine Strafe:** Widerruf darf nicht zu Funktionsverlust führen (außer bei Pay-or-OK mit Alternativangebot)
4. **Sofortige Wirkung:** Nach Widerruf muss Tracking unverzüglich stoppen
5. **Dauerhaft zugänglich:** Widerrufsmöglichkeit muss immer auffindbar sein (nicht nur beim ersten Banner-Aufruf)

---

## Relevante Rechtsprechung

### Planet49 (EuGH, 01.10.2019, C-673/17)
**Sachverhalt:** Vorausgefüllte Checkboxen für Werbe-Einwilligung.
**Entscheidung:** Kein wirksames Opt-In bei vorangehakten Checkboxen — aktive Handlung erforderlich.
**Relevanz für Widerruf:** Bestätigt, dass passive "Einwilligung" nichtig ist. Widerruf muss aktiv ermöglicht werden.

### BGH "Cookie-Einwilligung II" (BGH, 28.05.2020, I ZR 7/16)
**Sachverhalt:** Cookie-Banner mit vorausgefüllten Checkboxen.
**Entscheidung:** Folgt Planet49. Nur aktives Opt-In = wirksame Einwilligung.
**Relevanz:** Definiert den Standard, gegen den Widerruf symmetrisch sein muss.

### LG München I (LG München I, 29.01.2024, 33 O 5198/22)
**Sachverhalt:** Cookie-Banner ohne gleichwertigen Ablehnen-Button.
**Entscheidung:** Fehlender gleichwertiger Ablehnen-Button = Verstoß gegen TTDSG/DSGVO.
**Relevanz:** Direkte Pflicht zur visuellen Gleichwertigkeit des Widerruf-Buttons.

### OLG Frankfurt (OLG Frankfurt, 27.06.2024, 6 U 192/23)
**Sachverhalt:** Cookie-Banner mit "X" = Akzeptieren.
**Entscheidung:** Schließen-Button darf nicht als Einwilligung gewertet werden.
**Relevanz:** Widerruf durch Banner-Schließen ist umgekehrt auch nicht ausreichend.

---

## Bußgeld-Übersicht (Art. 83 DSGVO)

| Verstoß | Bußgeldrahmen | Beispiele (DE) |
|---|---|---|
| Kein Widerruf möglich (Art. 7(3)) | bis 20 Mio. € oder 4% Jahresumsatz | Meta IE (2023): 1,2 Mrd. € |
| Kein gleichwertiger Ablehnen-Button | bis 10 Mio. € oder 2% Jahresumsatz | — |
| Tracking ohne/vor Einwilligung | bis 20 Mio. € oder 4% Jahresumsatz | Google Analytics (AT, 2022) |
| Datenschutzseite ohne Widerruf-Info | bis 10 Mio. € oder 2% Jahresumsatz | — |

---

## Muster-Texte für Datenschutzerklärungen

### Widerruf-Abschnitt (Art. 7(3) DSGVO Pflichtangabe)

```markdown
### Recht auf Widerruf Ihrer Einwilligung

Soweit die Verarbeitung Ihrer personenbezogenen Daten auf Ihrer Einwilligung beruht 
(Art. 6 Abs. 1 lit. a) DSGVO bzw. Art. 9 Abs. 2 lit. a) DSGVO), haben Sie das Recht, 
Ihre Einwilligung jederzeit zu widerrufen.

**Wie Sie widerrufen können:**
- **Cookies und Tracking:** Klicken Sie jederzeit auf den Link „Cookie-Einstellungen" 
  in der Fußzeile dieser Website und deaktivieren Sie die jeweiligen Kategorien.
- **Newsletter:** Klicken Sie auf den Abmelde-Link am Ende jeder Newsletter-E-Mail 
  oder schreiben Sie uns an [E-Mail-Adresse].
- **Weitere Einwilligungen:** Kontaktieren Sie uns unter [Kontaktdaten].

Durch den Widerruf wird die Rechtmäßigkeit der bis zum Widerruf erfolgten Verarbeitung 
nicht berührt.
```

### Cookie-Banner Ablehntext

```markdown
Wir nutzen Cookies und ähnliche Technologien. Mit Klick auf „Alle akzeptieren" 
stimmen Sie der Verwendung aller Cookies zu. Mit „Alle ablehnen" stimmen Sie nur 
technisch notwendigen Cookies zu. Ihre Einwilligung können Sie jederzeit unter 
„Cookie-Einstellungen" (Link in der Fußzeile) widerrufen.
```

---

## Implementierungs-Checkliste für Entwickler

### CMP-Auswahl und -Konfiguration

- [ ] CMP (z.B. Usercentrics, Cookiebot, Consentmanager) DSGVO-konform konfiguriert
- [ ] "Alle ablehnen" Button als gleichrangiger Primär-Button konfiguriert
- [ ] Ablehnen nicht auf zweite Ebene verschoben
- [ ] Consent-Zustand im localStorage/Cookie korrekt gespeichert
- [ ] Widerruf-Handler an alle Tracking-Scripts übergeben
- [ ] Footer-Link zu Cookie-Einstellungen dauerhaft vorhanden

### Code-Muster für Consent-Gate (Next.js / React Beispiel)

```typescript
// Tracking nur nach Einwilligung initialisieren
const initAnalytics = () => {
  const consent = localStorage.getItem('cookie_consent');
  if (consent && JSON.parse(consent).analytics === true) {
    // Analytics initialisieren
    gtag('consent', 'update', { analytics_storage: 'granted' });
  }
};

// Widerruf-Handler
const revokeConsent = (category: string) => {
  const consent = JSON.parse(localStorage.getItem('cookie_consent') || '{}');
  consent[category] = false;
  localStorage.setItem('cookie_consent', JSON.stringify(consent));
  
  if (category === 'analytics') {
    gtag('consent', 'update', { analytics_storage: 'denied' });
  }
};
```

### Typescript-Typen für Consent-Status

```typescript
interface ConsentState {
  necessary: true;           // immer true, nicht widerrufbar
  analytics: boolean;
  marketing: boolean;
  preferences: boolean;
  timestamp: string;         // ISO 8601, für Nachweispflicht Art. 7(1)
  version: string;           // Datenschutzrichtlinien-Version
}
```
