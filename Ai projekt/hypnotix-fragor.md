# Frågor om Hypnotix – bra och dålig fråga

Kodbas: [linuxmint/hypnotix](https://github.com/linuxmint/hypnotix) – en IPTV-spelare skriven i Python (GTK + mpv).

## Dålig fråga

> Hur funkar Hypnotix?

**Varför den är dålig:**

- Den är för bred och vag och pekar inte ut någon del av koden.
- Den visar inte vad jag själv redan har undersökt.
- Den säger inte vad jag vill få ut av svaret.
- Svaret blir därför allmänt och svårt att använda.

## Bra fråga

> I Hypnotix läses M3U-spellistor in och delas upp i kanaler, filmer och serier. Jag har tittat i `usr/lib/hypnotix/` och ser att tolkningen verkar ske i `common.py`. Hur avgör koden om en post i spellistan är en livekanal, en film eller en serie, och vad händer om en post saknar `group-title`?

**Varför den är bra:**

- Den är specifik: den gäller en funktion (M3U-tolkningen) och en fil.
- Den visar att jag har gjort förarbete och vet var jag har letat.
- Den går att besvara konkret genom att peka på kod.
- Den innehåller ett kantfall (saknad `group-title`), vilket visar kritiskt tänkande.

### Svar på den bra frågan (från analys av `common.py`)

**1. Utifrån gruppnamnet** (klassen `Group`, rad 63–70)
Värdet i `group-title` delas upp i ord:

- innehåller ordet `VOD` → film
- innehåller ordet `SERIES` → serie
- annars → livekanal

Jämförelsen är skiftlägeskänslig och kräver hela ord. En grupp som heter "Movies", "vod" eller "Series-2024" hamnar därför under livekanaler.

**2. Utifrån kanalnamnet** (rad 254–276)
Ett reguljärt uttryck (rad 12) letar efter mönstret `Namn S01 E02`. Vid träff sorteras posten in som serie → säsong → avsnitt, oberoende av gruppen. En post kan alltså räknas både som livekanal och som seriavsnitt.

**Om `group-title` saknas** (rad 293–294)
Posten läggs direkt i listan över livekanaler utan grupp. Programmet kraschar inte, utan faller tillbaka på livekanal som standard.

## Tumregel

En bra fråga om en kodbas säger **var** du har tittat, **vad** du har förstått och **exakt vad** du inte förstår.
