# NL-straatnaamvarianten

Deze repository bevat spellingsvarianten, afkortingen en alternatieve namen van Nederlandse straten, gekoppeld aan Wikidata items van die straten.

Historische teksten komen in steeds grotere aantallen beschikbaar. Vrijwilligers transcriberen in talloze crowdsourceprojecten archiefstukken en de ontwikkelingen in de 'handwritten text recognition' staan verre van stil - het digitaal beschikbaar maken van hele archieven komt binnen bereik. De computer leert lezen, nu zou het mooi zijn als ze het gelezene ook leert 'begrijpen', bijvoorbeeld door 'named entities' (persoonsnamen, plaatsnamen, etc.) te herkennen. 

Dit soort artificiële intelligentie is afhankelijk van data. Enerzijds kan AI dan patronen leren herkennen, bijvoorbeeld als het `laan` steeds afgekort ziet worden tot `l.`. Anderzijds heeft AI data nodig om `Noorder Amstell.` thuis te kunnen brengen als de in 1946 hernoemde `Churchilllaan`.

## Wikidata

Vrijwel alle Nederlandse straten, uitgezonderd de zeer recent aangelegde straten en de straten waaraan geen adressen liggen, zijn te vinden op Wikidata. Daar kan je ook allerlei gegevens kwijt, zoals periode van bestaan, waar straten naar vernoemd zijn, coördinaten, indien hernoemd alle officiële namen, alternatieve namen,  etc. Maar Wikidata lijkt niet bedoeld om elke afkorting en (historische) spellingswijze op te slaan - vandaar de lijsten in deze repository.

Straatnamen zijn in Wikidata te vinden bij de volgende labels en properties:

- Label (`skos:prefLabel`), gecombineerd met taal
- Altlabel of 'Ook bekend als'(`skos:altLabel`), gecombineerd met taal
- Officiële naam (`P1448`), bijvoorbeeld te gebruiken voor namen die bij raadsbesluit zijn vastgelegd
- Naam (`P2561`), voor minder formeel vastgelegde namen (anders dan bij altlabels kan je hier bijvoorbeeld qualifiers als 'begindatum' en 'einddatum' kwijt)

Via de [Wikidata sparql endpoint](https://query.wikidata.org/) kan je straatnamen, bijvoorbeeld per gemeente, opvragen en downloaden. De onderstaande query vraagt alle straatnamen binnen de gemeente Haarlem op, met coördinaten en eventuele begin- en eindjaren:

```
SELECT ?item ?itemLabel ?altlabel ?officiallabel ?namelabel ?coords ?startjaar ?eindjaar WHERE {
  ?item wdt:P31 wd:Q79007 .
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

Meer weten over het editen van (historische) gegevens over straten op Wikidata? [Hier](https://github.com/mmmenno/linked-elo/tree/master/straten) leg ik uit hoe je dateringen, vernoemingen en relaties als 'opgegaan in', 'vervangen door' en 'onderdeel van' in Wikidata kunt noteren.

## Straatnamen per plaats

### Haarlem

Voor het bestand [haarlem.csv](haarlem.csv) zijn o.a. de volgende bronnen gebruikt:

- de [kaart van Nautz](https://github.com/mmmenno/nautz) uit 1829
- de [straatnamenindex op de Puiboeken 1760-1813](https://geneaknowhow.net/script/dewit/haarlem-puiboeken.htm) van Willem Blok



