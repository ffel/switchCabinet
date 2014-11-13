1.  Een *closure*.

    Voordeel van een closure:

    -   Je hoeft niet in de de code te springen om de functie te lezen

    -   Heeft in de body direct toegang tot de variabelen die in de
        omliggende functies bereikbaar zijn

    Neutrale eigenschappen

    -   Is geschikt voor toepassing op één plek in de code, nl. daar
        waar je deze definieert.

    Waarom dan alsnog een *functie*?

    -   Is als functie vooraf te gaan door `go`{.go}, dus concurrent.

    -   Kan gebruikt worden als argument voor een functie die een
        functie als een van zijn argumenten gebruikt.

2.  *unbuffered channel*

    Hier probeert de closure uit *1* data over de channel te sturen.

    Het lukt pas om data over een channel te sturen wanneer er aan de
    andere kant de data wordt ontvangen.

    > het fungeert een beetje als een met water gevulde leiding. Omdat
    > de inhoud niet comprimeerbaar is, kan er pas iets in als er op een
    > andere plek net zoveel water uit de leiding verdwijnt.

    Dus, stel dat er 20 te downloaden blokken zijn, dan kan op deze plek
    pas data verstuurd worden wanneer er aan de andere kant een worker
    vrij komt om die data te gebruiken.

    Kortom, gedurende de loop van het download proces blijft de closure
    op dit punt zo nu en dan nieuwe data op de channel verzenden.

3.  *close the channel*

    Wanneer de closure alle data heeft verzonden sluit deze closure de
    channel. Dit is belangrijk!

    Dave Cheney beschrijft in [Curious
    Channels](http://dave.cheney.net/2013/04/30/curious-channels) en
    [Channel Axioms](http://dave.cheney.net/2014/03/19/channel-axioms)
    het gedrag van channels.

    Dave schrijft: **A receive from a closed channel returns the zero
    value immediately**.

    Dit is relevant voor *4*.

    > Controleer het nog eens een keer: het komt noh niet helemaal
    > overeenstemming met wat ik verwacht had. Ik hoop dat een `range`
    > op een closed channel er voor zorgt dat de iteratie stopt op
    > dezelfde manier als een range die die laatste element van een
    > slice heeft bereikt.

    Het is idd zo dat doordat de channel closed is, de range terminates,
    zie [go by example](https://gobyexample.com/range-over-channels).

4.	*range over channel*


