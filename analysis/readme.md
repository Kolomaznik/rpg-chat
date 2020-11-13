# Analýza požadavků

Systém je analyzován po malých částech (procesech/scénářich).

Každý scénař je opsán v samostatném dokumentu a představuje jednu uzavřenou funkcionalitu.

Nad všemi scénaří je definován *overview* která lučije klíčové prvky každého scécnaře.

##### PlantUML diagrams
Digramd tatový tříd se vytvoří pomocí nástroje [PlantUMl](https://plantuml.com/).
V rámci [*overview ClassDiagram.adoc*] se všechny diagramy skládaji dobromady pro sískání ucelého přehledu.

## Obsah scénáre
Scénáře by měli mít co nejpodobnější strukturu, aby se v nich dalo snadno vyznat.

### Usecase
Nejprve se sepíše [Usecase diagram](https://plantuml.com/use-case-diagram). 

Pro přehled nad projektem se všechny digramy skládají do jedno [overview UseCase](overview/UseCaseDiagram.adoc).
Pro je potřeba oby obsaholi možnost `inlude::` přes ascidoctor.

```adoc
[plantuml, diagram/S01_usecase, svg]
----
left to right direction
' tag::diagram_usecase[]

diagram definition here

' end::diagram_usecase[]
----
```

Ten obsahuje rovněř textoví popis scénaře, jak by měl probívat.

U složitějšíc scénářů je možné nadeinovat i 
* Precodition
* Poscondition
* Scenarion
* [Activity diagram](https://plantuml.com/activity-diagram-beta)

### Design UI
Návrh uživatelského rozhranní slouží k představě jak bude scénář vypadat.

_Uživatelské rozhranní ja varhováno v nástoji [Figma](https://www.figma.com/).
V adresáři design je kromě vyexporvovaných svg návrhu uložen i celý [fig soubor](design/RPG-chat.fig)._

Pod design je textově definován *Model* což je víceurovnový seznam který popisuje data, který ze obrazují v modelu.

Jedná se v podstatě o derivát relevatních textů z návrhu spolu s některými skrytými daty a popisnými informacemi.

Másledující příklad z prvního scénaře:

- `name`: jméno uživatle
- `stories`: seznam jeho dobrodruští setízeni podle poslední aktivity
  - `image`: Motivační obrázek definovaný u příbehu
  - `title`: Název příbehu
  - `last_activity`: Posladní aktivita zaz
    - `Author`: Kdo je autorem aktivity
    - `timestamp`: datum a čas kdy byla událost vytvořena
    - `message`: Zpráva učivatele v chat. 
                 _Zatím se počítá pouze se zprávami do budoucna může přibýt další typy._
- `new_story`: Data ve formulári pro nový scénář
  - `title`: titulek příběhu
  - `image`: může být nahrán uživatelm
    - `predefine_images`: Uživatel má možnost si výbrat jeden z předefinovaných žánrových obrázků.
    
### Class diagram datových tříd
Z User case a Design UI odvodíme [class diagram](https://plantuml.com/class-diagram) pro datové třídy.
Cílem tohoto digramu je zahytit strukturu dat. 

v dalších fázích ze něj může být odvozen ERD diagram (pro lelační databáze) nebo dokumentový digram nebo graf.
Záleži ale na typu zolené databáze.

Class digramy jsou skládany do přehledového [overview ClassDigram](overview/ClassDigram.adoc). ,proto je potřebné aby toto umožnili pomocí `include::` příkazu. 
Musí mát tedy definovaný `tag` pro vkladaní.

```adoc
[plantuml, diagram/S01_class, svg]
----
' tag::diagram_class[]

diagram definition here

' end::diagram_class[]
----
```

### Modelování procesu
Dalším krokem je vytvoření [Sekvenční digramu](https://plantuml.com/sequence-diagram)
Učelem tohot digramu je navhnou servicní třídy které manipuluji s datovými třídami, případně metody datových tříd.

U techto digramů se *nepočítá* s vkládním do jednotného *overview*, přesto je dovbré mít možnost se na tento digram odkázat pomocí `tag`:

```adoc
[plantuml, diagram/S01_sequence, svg]
----
skinparam responseMessageBelowArrow true
' tag::diagram_sequence[]

diagram definition here

' tag::diagram_sequence[]
----
```

Po Sekvenčím digramu následuje stručný popis funkce navrhnutých metod.

## Document template

```adoc
= Scénář {id}: title
Jan Kolomazník <jan.kolomazník@gmail.com>
:id: Set id here (S01)
:toc: left
:icons: font

== Usecase Create Story
[plantuml, diagram/{id}_usecase, svg]
----
left to right direction
' tag::diagram_usecase[]

:User:

' end::diagram_usecase[]
----

Write text description for usecases here.

=== Precodition
definition:: description

=== Poscondition
definition:: description

=== Scenarion
Semistructuration text

=== Activity digram
[plantuml, diagram/{id}_activity, svg]
----
' tag::diagram_activity[]

start

' end::diagram_usecase[]
----

== Design UI
image::design/{id}_title.svg[{id} Design]

.Model
* `data`: description
** `subdata`: Motivační obrázek definovaný u příbehu

== Digram datových tříd
[plantuml, diagram/{id}_class, svg]
----
' tag::diagram_class[]

namespace cz.rpg.chat.name {

    class .Name {
    }

}

' end::diagram_class[]
----

Write text description for classes here.

== Modelování prcesu
[plantuml, diagram/{id}_sequence, svg]
----
skinparam responseMessageBelowArrow true
' tag::diagram_sequence[]

actor User

' tag::diagram_sequence[]
----

.Methods
- *metods*: description
```


Poté co založít nový scénár, aktualizauj digramy v *overview*:
* [ClassDiagram](overview/ClassDigram.adoc)
* [UseCaseDiagram](overview/UseCaseDiagram.adoc)