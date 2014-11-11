Downloader
==========

De [applicatie](https://github.com/nf/dl)[^1] kan een bestand in
meerdere blokken tegelijkertijd downloaden. Dit is een belasting voor de
server, maar kan zorgen voor een veel snellere download bij de client.

Ondanks dat de bestudeerde applicatie flink wordt uitgebreid, relevante
vragen zijn:

1.  Wordt de applicatie beter te begrijpen?

    Feit is dat de applicatie kort is, maar zeker niet triviaal. Ook is
    het mogelijk dat de applicatie werkt, maar nog niet de meest heldere
    opzet heeft gekregen.

    > Een van de dingen die mij opvalt is dat er een error channel is
    > die niet altijd wordt gebruikt. Soms wordt de error channel
    > gebruikt, soms wordt een error teruggegeven. Ik heb het gevoel
    > (maar wellicht ten onrechte) dat dat voor verbetering vatbaar is.

    > Houdt er rekening mee dat je bezig bent onderzoek te doen naar de
    > "fosiele lagen van iemands gedachten": onduidelijkheden kunnen
    > zowel komen doordat ik het niet begrijp of doordat de schrijver
    > nog niet de meest heldere opzet heeft bedacht.

    Ook is het mogelijk dat de visuele representatie te weinig toevoegt
    aan het begrijpen van de applicatie.

    Een probleem is dat wellicht te weinig tot uiting komt dat
    communicatie via channels *blokt*, dus

    -   soms lijkt de levensduur van een channel beperkt, maar is van
        vanwege bloks niet het geval (het duurt een tijd voor de laatste
        waarde over de channel verstuurd wordt).

    -   blokken staat ook voor bewaren van *synchronteit*, het zou
        prettig zijn wanneer de communicatie door een channel een
        rechte, horizontale lijn is.

        Zo'n lijn is lang niet altijd mogelijk. Een voorbeeld is de
        communicatie over de error channel: dit zijn twee blokken die
        uiteindelijk door de `get()` worden geïnitieerd.

    Misschien moet ik juist stellen dat de oplossing van channels
    elegant is.

    Ik denk wel dat er voor een eerste overzicht *teveel* details in het
    schema zetten. Een voorbeeld is dat het op dit niveau nog niet
    uitmaakt wie channels maakt. Voor nu is het voldoende dat ze er
    zijn.

    Een aantal mogelijk relevante details is nu wellicht nog teveel
    verborgen: Bijvoorbeeld dat er een heel aantal *closures* tegelijk
    in de lucht zijn die elk tot drie pogingen doen om een blok te
    downloaden.

2.  Zou je eerst zo'n schema kunnen maken om uiteindelijk sneller of
    beter een dergelijke applicatie te schrijven?

3.  Wat vind ik belangrijk om over te brengen als eerste indruk hoe de
    applicatie werkt:

    -   De applicatie kan meerdere grote bestanden achter elkaar
        downloaden.

    -   Voor ieder bestand zijn maximaal 10 downloaders in de lucht die
        een blok van het bestand downloaden.

        Ieder downloader neemt een blok met maximale omvang voor zijn
        rekening.

        > Dit is een per blok ongeveer [10
        > MB](http://www.miniwebtool.com/bitwise-calculator/bit-shift/?data_type=10&number=10&place=20&operator=Shift+Left).

    -   Wanneer een downloader klaar is, neemt deze een volgend blok
        voor zijn rekening.

        Sommige blokken zullen sneller worden gedownload dan andere
        blokken, dus de volgorde waarin downloaders een nieuw blok voor
        hun rekening nemen is onduidelijk.

        Hier heb je rekening te houden met de verschillende
        downloadsnelheden

    Secundaire details:

    -   Iedere downloader doet maximaal drie pogingen.

    -   Fouten leiden tot afbreken van de download waarna met het
        volgende te downloaden bestand wordt begonnen.

Naarmate ik langer met deze applicatie bezig ben, krijg ik het idee dat
het patroon veel universeler is:

1.  Werkverdeler

2.  Een aantal uitvoerders

3.  Rapportage en afbreken in geval van problemen

[^1]: Ondertussen is al een nieuwere versie gecommit!
