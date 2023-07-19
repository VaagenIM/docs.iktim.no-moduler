---
title: Oppgave mkdocs
aliases: 
  - Oppgave: Dokumentasjonspublisering
lang: nb-NO
authors:
  - Sondre Grønås
tags:
  - Oppgave
  - mkdocs
  - readthedocs
created: 2023-05-28 11:07:53
updated: 2023-06-05 17:54:19
---
# Oppgave: Dokumentasjonspublisering
> [!IMPORTANT] Uferdig, TODO:
> - [ ] Bryt opp til flere sider?
> - [ ] Opprett utgangspunktrepo
 
I denne oppgaven skal du bruke prinsippene fra CI/CD for å opprette en GitHub workflow som publiserer dokumentasjon til et programmeringsprosjekt opp mot GitHub pages. Også kjent som Continuous Documentation Deployment.

God dokumentasjon pleier å inneholde følgende:
- [ ] Oversikt over prosjektet
- [ ] Installasjon
- [ ] Bruksinstrukser (kodereferanser / schema)
- [ ] _Endringslogg (Changelog)_
- [ ] _Lisens_
- [ ] _Credits_

Du kan ta utgangspunkt i følgende GitHub repository: (TBD, mangler link)

> [!CODE]+ OBS: GitHub Classroom
> Sjekk om du finner en GitHub Classroom link til oppgaven i Teams, eller be om å få det. Ved å besvare oppgaven i GitHub Classroom kan du publisere til GitHub Pages uten å måtte publisere repositoriet.

## Hvorfor dokumentasjon?
<iframe width="560" height="315" src="https://www.youtube.com/embed/L7Ry-Fiij-M" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Fremgangsmåte
For å få mest utbytte av oppgaven anbefales det å følge stegene under.

Det anbefales å bruke `mkdocs` og Markdown sider for å gjennomføre oppgaven, men det finnes alltid alternativer!
- https://squidfunk.github.io/mkdocs-material/
- https://docs.readthedocs.io/en/stable/index.html

> [!HINT] Sjekk ut lenkene, de er i seg selv gode eksempler på dokumentasjon.

### 1. Forstå koden
Før man kan skrive dokumentasjon, må man forstå hva formålet bak koden er. Tenk gjennom når du leser koden: Hva, hvorfor og hvordan? Hva gjør koden? Hvorfor ble den skrevet, til hvilket formål? Hvordan kan man ta den i bruk?

### 2. Konfigurer dokumentasjonstjenesten
Før du skriver dokumentasjon, så kan det være lurt å teste ut noen andre sin dokumentasjon først! Bruk dokumentasjonen til å finne ut hvordan du konfigurerer tjenesten.

Dersom du ønsker å bruke mkdocs, bør du følge dokumentasjonen: https://squidfunk.github.io/mkdocs-material/creating-your-site/

Eller så kan du se den i form av en YouTube tutorial (start fra 3:26, du finner tilsvarende funksjonalitet i PyCharm):
<iframe width="560" height="315" src="https://www.youtube.com/embed/Q-YA_dA8C20?start=206" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Merk at en YouTube video er ikke alltid like oppdatert, men er ofte enklere å fordøye.

### 3. Skriv dokumentasjonen
Det kan være lurt å begynne med å skrive tekst i et tekstbehandlingsprogram, eller direkte som en Markdown fil (`.md`) i utviklingsmiljøet i en egen `docs` mappe. Dersom dokumentasjonen angår kodens funksjoner eller klasser, vil det være lurt å skrive denne direkte i kodefilene i form av docstrings (se under), eventuelt kommentarer.

Din dokumentasjon bør inneholde følgende:
- `index` - Oversikt over prosjektet, hva er det?
- `Installation` - Hvordan installerer man? (Kan være en del av index)
- `Usage` - Hvordan bruker man? (Kan være en del av index)
- `Code Reference` - Hvordan fungerer koden? Hva gjør de ulike funksjonene? Se også: https://mkdocstrings.github.io/

#### Eksempel docstring i kode
Det finnes ulike standarder for docstrings, noen av de mest vanlige er [Google](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings) og [Sphinx](https://sphinx-rtd-tutorial.readthedocs.io/en/latest/docstrings.html) sin standard, eller NumPy for mer matematiske prosjekter.

I PyCharm er Sphinx brukt som standard docstringformat, som er brukt i eksemplene under.

Se for deg at vi har følgende kode:

```python
def calculate_price(cost: float) -> float:
    total = cost * 1.25

    if total < 500:
        total += 79

    return total
```

og tenk, hvor enkelt er det å forstå koden?

Her er samme kode med en docstring:

```python
def calculate_price(cost: float) -> float:
    """
    Calculate the final price of a product, including taxes and shipping costs.
    If the final price is less than 500, a shipping cost of 79 is added.
    
    :param cost: The net price of the product (without taxes).
    :return: The final price of the product (including taxes and shipping costs).
    """
    total = price * 1.25

    if total < 500:
        total += 79

    return total
```

Lettere å forstå formålet til koden nå, ikke sant? Merk at koden ville egentlig hatt mer nytte av å brytes opp i mindre funksjoner for å slippe å måtte forklares grundig gjennom en docstring.

> [!INFO]+ Type hinting og variabelnavn er også en form for "dokumentasjon"
> I eksemplene er det brukt type hinting i funksjonens parametre, som forteller utviklingsmiljøet hvilke verdier som forventes av funksjonen. Ved å definere tydelige klasser blir man også mindre avhengig av å lese dokumentasjon; koden i seg selv er en del av kommentarene.
> 
> Eksempel:
> ```python
> def heal(hp, amnt):
>     hp += amnt
> ``` 
>vs.
>```python
>def heal(character: Character, amount_to_heal: int) -> None:
>	character.hp += amount_to_heal
> ```

### 4. Opprett en GitHub Workflow
Ved å opprette en Workflow fil (under GitHub Actions) kan man lage instrukser til en [GitHub Runner](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners), som spinner opp en virtuell maskin og utfører oppgaver for oss. Workflowene må lagres i en egen mappe i repoet: `.github/workflows/` og være i `.yml` format.

> [!IMPORTANT] Dersom du allerede har laget en workflow fil via videoen trenger du ikke forholde deg til koden under. Så lenge den blir publisert, så er alt OK! 😄

GitHub Actions kan være skremmende til å starte med, men heldigvis kan du benytte deg av forhåndsdefinerte maler, eller ta utgangspunkt i denne filen, det er lagt inn kommentarer der du kan fritt gjøre endringer. Merk at runneren tar utgangspunkt i `mkdocs`:

```yaml title=".github/workflows/CD.yml"
name: Documentation

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    permissions:
      contents: write
      pages: write
      id-token: write
    steps:
    - uses: actions/checkout@v3

    - name: Set up Python 3.10
      uses: actions/setup-python@v3
      with:
        python-version: "3.10"

	# HER KAN DU LEGGE TIL EKSTRA LINJER MED
	# LINUX KOMMANDOER, F.EKS. FLYTTE ELLER
	# OPPRETTE NYE FILER (F.EKS.) REDIGERE
	# "Sist oppdatert" TEKST.
    - name: Install Python Packages
      run: |
        python -m pip install --upgrade pip
        pip install setuptools wheel
        pip install mkdocs
        echo Hello World!

    - name: Build site
      run: mkdocs build --clean

    - name: Upload artifact
      uses: actions/upload-pages-artifact@v1
      with:
        name: github-pages
        path: ./site

    - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v1
```

> [!TECH]+ Om GitHub Workflows
> GitHub Workflows kan være skremmende, og ofte krever de mye prøving og feiling. Det er vanskelig å tyde hva de ulike linjene egentlig gjør, eller hvilket formål de egentlig har, som f.eks. `${{ steps.deployment.outputs.page_url }}`.
> 
> Det finnes dog bare en måte å bli klokere på dette på, og det er å lese dokumentasjonen 😜

### 5. Verifiser GitHub Pages
Inne på GitHub repoet skal du nå finne en `Environment` fane i menyen, dersom steg 3 var vellykket.

![[gh-pages-screenshot.png]]

Trykk deg inn på denne for å finne linken til dokumentasjonen din. Fungerer den? Ser noe rart ut?

Dersom alt fungerer, kan du oppdatere repoet til å inkludere en link. Vis derretter dokumentasjonen til en annen og be om tilbakemelding. Er det noe som kan formuleres enklere?

### 6. Forbedre dokumentasjonen
Dersom du har mer tid, kan du prøve å forbedre dokumentasjonen din etter tilbakemeldingene du har fått.

Du kan også prøve å legge til illustrasjoner, bilder eller annet innhold, the ☁ is the limit!