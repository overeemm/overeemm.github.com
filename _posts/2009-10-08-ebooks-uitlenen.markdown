---
layout: posts
title: ebooks uitlenen (dutch)
categories: [reading, drm, ebooks]
---
Zoals de meeste die mij volgen op [twitter][1] wel weten lees ik nogal veel boeken (zie bijvoorbeeld [mijn pagina][2] op [@deboekenlijst][3]).
 
Wat mij betreft zijn ebooks dan ook helemaal geweldig. Ik koop mijn boeken toch al, dus waarom niet op een formaat dat ik ook makkelijk mee kan nemen. In plaats van 10 keer een boek van 3 a 4 centimeter dik heb ik nu alleen mijn reader nodig op vakantie. Maar, het moet natuurlijk wel net zo gemakkelijk zijn als een boek. Als het even kan wil ik mijn boeken kunnen uitlenen en wil ik boeken van vrienden en familie lenen. En dat laatste bleek toch iets lastiger...
 
Ik heb [een reader van bol.com][4] en heb daar inmiddels ook een aantal boeken gekocht. Bol.com gebruikt het [epub][5] formaat, wat betekent dat je je boeken moet registreren met [Adobe Digital Editions][6]. Je autoriseert je computer, je reader en daarna het ebook. Zolang het 1 reader betreft is er niks aan de hand. Maar nu wil ik mijn ebook uitlenen. Dus ik stuur het .acsm bestand op dat je krijgt van [bol.com][7]. Ze adverteren immers met het feit dat je boeken kan uitlenen (Het lijkt erop dat dit terug gedraaid is? Ik kan dit niet meer op de website vinden). Helaas kan het ebook bestand niet met een ander Adobe ID geregistreerd worden. Dus dan maar proberen het boek met mijn ID registreren voor een andere reader. En weer helaas: een reader kan maar aan 1 ID gelinkt zijn! Oftewel: wil je boeken (uit)lenen? Dan moet je 1 Adobe ID gebruiken. Dat is natuurlijk niet handig.
 
Daarom ben ik maar op zoek gegaan om de [DRM][8] van de epub files te slopen. Let op, niet met het doel om al mijn boeken te verspreiden. Ik verspreid ze alleen maar naar vrienden en familie, met daarbij natuurlijk de voorwaarde dat zij het niet weer verder verspreiden. Lijkt mij geen echt probleem, ik koop immers al mijn boeken nog steeds... (overigens zijn er via Google en Projet Gutenberg ook gratis boeken te downloaden).
 
Het verwijderen van DRM is eigenlijk vrij simpel (Dit is alleen voor Windows, ik heb niet verder gezocht naar een MacOSX handleiding). Even googlen levert al snel 2 links op:
[http://rapidfind.org/upload/showthread.php?p=1035319][9]
[http://klungvik.com/index.php/2008/how-to-remove-drm-from-ebooks/][10]
 
Wat heb je nodig? 
+ [Python voor Windows (versie 2.6)][11]
+ [De library Pycrypto][12]
+ [Adobe Digital Editions][13]
+ Een tweetal scripts: ineptkey.pyw en ineptepub.pyw - deze zet ik niet ter download hier, maar even [googlen][14] levert ze al snel op.
 
Nu begint het echte werk:
+ Eerst moet je een ebook bij [bol.com kopen][7]. Dit levert een .acsm file op.
+ Deze open je met Adobe Digital Editions, unlock hem met je Adobe ID.
+ Zoek nu de map 'My Digital Editions' op. Hierin staat het echte .epub bestand.
+ Kopieer het .epub bestand naar de map met python scripts.
+ Run eerst het script ineptkey.pyw. Dit levert een adeptkey.der bestand op.
+ Run het script ineptepub.pyw. Je krijgt een schermpje dat vraagt naar het key bestand (zie vorige stap) en het .epub bestand. Specifieer het output bestand en runnen maar.
 
Het bestand zelf kan nu met bijvoorbeeld [Calibre][15] omgezet worden naar andere formaten, of op de reader geplaatst worden.

[1]: http://twitter.com/michielovereem
[2]: http://deboekenlijst.ixmucane.nl/michielovereem.html
[3]: http://twitter.com/deboekenlijst
[4]: http://www.bol.com/nl/p/elektronica/sony-reader-pocket-edition-zilver/9000000006931121/index.html
[5]: http://en.wikipedia.org/wiki/EPUB
[6]: http://en.wikipedia.org/wiki/Adobe_Digital_Editions
[7]: http://nl.bol.com/
[8]: http://nl.wikipedia.org/wiki/Digital_Rights_Management
[9]: http://rapidfind.org/upload/showthread.php?p=1035319
[10]: http://klungvik.com/index.php/2008/how-to-remove-drm-from-ebooks/
[11]: http://www.python.org/download/releases/2.6.3/
[12]: http://www.voidspace.org.uk/python/modules.shtml#pycrypto
[13]: http://www.adobe.com/products/digitaleditions/
[14]: http://www.google.nl/search?hl=nl&rlz=1C1GGLS_nlNL343NL343&q=ineptkey.pyw+ineptepub.pyw&btnG=Zoeken&meta=
[15]: http://calibre.kovidgoyal.net/