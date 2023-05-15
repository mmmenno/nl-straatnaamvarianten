# NL-straatnaamvarianten

Bij het thuisbrengen van (bijvoorbeeld met Named Entity Recognition gevonden) straatnamen zijn lijsten met naamvarianten en schrijfwijzes heel handig. Deze repository bevat (links naar) spellingsvarianten, afkortingen en alternatieve namen van Nederlandse straten, gekoppeld aan Wikidata items van die straten.

## Wikidata

Vrijwel alle Nederlandse straten, uitgezonderd de zeer recent aangelegde straten en de straten waaraan geen adressen liggen, zijn te vinden op Wikidata. Daar kan je ook allerlei gegevens kwijt, zoals periode van bestaan, waar straten naar vernoemd zijn, coördinaten, indien hernoemd alle officiële namen, alternatieve namen,  etc. Maar Wikidata lijkt niet bedoeld om elke afkorting en (historische) spellingswijze op te slaan - vandaar de lijsten in deze repository.

Straatnamen zijn in Wikidata te vinden bij de volgende labels en properties:

- Label (`skos:prefLabel`), gecombineerd met taal
- Altlabel of 'Ook bekend als'(`skos:altLabel`), gecombineerd met taal
- Officiële naam (`P1448`), bijvoorbeeld te gebruiken voor namen die bij raadsbesluit zijn vastgelegd
- Naam (`P2561`), voor minder formeel vastgelegde namen (anders dan bij altlabels kan je hier bijvoorbeeld qualifiers als 'begindatum' en 'einddatum' kwijt)

Via de [Wikidata sparql endpoint](https://query.wikidata.org/) kan je straatnamen, bijvoorbeeld per gemeente, opvragen en downloaden. De onderstaande query vraagt alle straatnamen binnen de gemeente Haarlem op, met coördinaten en eventuele begin- en eindjaren:

```
SELECT ?item ?itemLabel ?altlabel ?officiallabel ?namelabel ?coords ?startjaar ?eindjaar ?openbruimteLabel WHERE {
  VALUES ?openbruimte { wd:Q79007 wd:Q174782 wd:Q574990 } # straat, plein
  ?item wdt:P31 ?openbruimte .
  ?item wdt:P131 wd:Q9920 .
  ?item wdt:P625 ?coords .
  OPTIONAL{
    ?item wdt:P571 ?start .
    bind(year(?start) as ?startjaar) .
  }
  OPTIONAL{
    ?item wdt:P576 ?eind .
    bind(year(?eind) as ?eindjaar) .
  }
  OPTIONAL{
    ?item skos:altLabel ?altlabel .
  }
  OPTIONAL{
    ?item wdt:P1448 ?officiallabel .
  }
  OPTIONAL{
    ?item wdt:P2561 ?namelabel .
  }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "nl,en". }
}

```

Meer weten over het editen van (historische) gegevens over straten op Wikidata? [Hier](https://github.com/mmmenno/linked-elo/tree/master/straten) leg ik uit hoe je dateringen, vernoemingen en relaties als 'opgegaan in', 'vervangen door' en 'onderdeel van' in Wikidata kunt noteren. In de meer algemene blog [Van Oude Nieuwstraat tot Onbekendegracht - straatnamen in collectiedata](http://islandsofmeaning.nl/straten-in-collecties/) lees je meer over naamvarianten, maar ook over bijvoorbeeld geometrieën.

## Straatnamen per plaats

### Amsterdam

Amsterdam is de vreemde eend in de bijt, want het heeft met [Adamlink](https://adamlink.nl/geo/streets/list) een eigen online stratenregister. Op de [data-pagina](https://adamlink.nl/data) is de hele dataset, inclusief naamvarianten, te downloaden als ttl.

Die varianten (iig de meest recente weergave daarvan) zijn ook via [data.create.humanities.uva.nl](https://data.create.humanities.uva.nl/) op te vragen. De onderstaande query levert meer dan 12.000 naamvarianten op (even de offset zelf ophogen om ook de volgende 2.000+ schrijfwijzes te verkrijgen):

```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX hg: <http://rdf.histograph.io/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
SELECT ?straat ?pref ?alt WHERE {
  ?straat a hg:Street .
  ?straat skos:prefLabel ?pref .
  ?straat skos:altLabel ?alt .
  FILTER (?pref != ?alt)
} 
OFFSET 0 LIMIT 10000
```

### Haarlem

Voor het bestand [haarlem.csv](haarlem.csv) zijn o.a. de volgende bronnen gebruikt:

- de [kaart van Nautz](https://github.com/mmmenno/nautz) uit 1829
- de [straatnamenindex op de Puiboeken 1760-1813](https://geneaknowhow.net/script/dewit/haarlem-puiboeken.htm) van Willem Blok

### Den Haag

De varianten in [den-haag.csv](den-haag.csv) komen uit [Leidse politiedossiers](https://dossier071.hicsuntleones.nl/)

### Leiden

Een bestand met meer dan 2.000 Leidse straatnaamvarianten is [hier](https://github.com/mmmenno/linked-elo/tree/master/straten) te vinden.

### 's-Hertogenbosch

In het [Stratenregister 's-Hertogenbosch](https://github.com/mmmenno/stratenregister-den-bosch) is een [bestand](https://github.com/mmmenno/stratenregister-den-bosch/blob/main/schrijfwijzes/naamvarianten.csv) met zo'n 150 varianten opgenomen)


