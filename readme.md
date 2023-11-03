# Hitobito Notfallkontakte

## Beschreibung
In Hitobito soll jede Person Notfallkontakte erfassen können, damit diese im Notfall kontaktiert werden können. Notfallkontakte bestehen aus Namen, Beziehung, Email und Telefonnummer
## Motivation Notfallkontakte
In Hitobito können aktuell keine Notfallpersonen erfasst werden, der SAC braucht dieses Feature für die Tourenverwaltung und Durchführung der Touren. Diese Notfallkontakte sollen dem Tourenleiter eine Möglichkeit geben, Verwandte oder andere Kotankte einer Verunglückten Person zu informieren.


## Erfassung von Notfallkontakten
Eine Person sollte Notfallkontakte ganz einfach im Formular hinzufügen oder entfernen können. Ein Benutzer kann keinen bis zwei Notfallkontakte erfassen. Das Limit von zwei Notfallkontakten ist auch das Limit beim davor verwendeten Tool. Im aktuellen Personenformular verwenden wir bereits für Angaben wie Telefonnummern, Social Media oder Familienmitgliedern eine Felderart, die es ermöglicht, keine bis zu mehrere Einträge zu erfassen. Das gleiche würden wir auch für die Notfallkontakte verwenden und das sähe wie folgt aus:

![Mobile Ansicht von Formular](/images/MobileAnsicht.png)

Auf dem Bild ist die Mobile Ansicht zu sehen, bei der die vier Felder untereinander angeordnet sind. Das erste Feld ist ein normales Textfeld für Name/vorname, das Feld für die Email soll auf eine Email validiert werden und die Telefonnummer soll auch einer Telefonnummer entsprechen. Das letzte Feld für die Beziehung ist kein Pflichtfeld und ist ein normales Textfeld, ohne Validierung.

![Desktop Ansicht von Formular](/images/DesktopAnsicht.png)

Hier ist noch ein Mockup für die Desktopansicht, hier sind wie auch bei den anderen bereits vorhandenen Feldern, für die Telefonnummern oder Familienmitglieder, die Felder nebeneinander angeordnet.

## Einsehen von Notfallkontakten

Auf der Personenübersicht werden die Notfallkontakte aus Datenschutzgründen nicht angezeigt.

![Notfallkontakt exportieren](/images/Export.png)

#### Export

Die Kontaktdaten können über eine neue Exportart exportiert werden, diese Liste enthält den Vor- und Nachnamen der an der Tour Teilnehmenden Person, die Adresse und die Kontaktpersonen. Falls die Person keinen oder nur einen Notfallkontakt erfassst hat, werden die Spalten leer exportiert.

Die Spalten für den Export sehen wie folgt aus:



| Name                | Adresse                         | Kontaktperson 1    | Kontaktperson 2    |
| ------------------- | ------------------------------- | ------------------ |------------------ |
| Vorname + Nachname  | Strasse + HausNr, PLZ + Wohnort | Name (Beziehung), Telefon, Email |Name (Beziehung), Telefon, Email |

Bedeutet der Export sollte ungefähr so aussehen

| Name                | Adresse                         | Kontaktperson 1    | Kontaktperson 2    |
| ------------------- | ------------------------------- | ------------------ |------------------ |
| Max Müller  | Kirschengasse 12a, 2731 Nirgendwoburg | Tim Teller (Bruder), 079 999 99 99, tim-teller@example.com |Tanja Tasse (Mutter), 079 777 77 77, tanja.teller@example.com |

Die Möglichkeit Notfallkontaktlisten zu exportieren wird nur auf der Teilnehmendenübersicht einer Tour benötigt und kann somit auch nur über diese aufgerufen werden.

#### Datenschutz
Der Export sollte pro Tour nur für den Tourenleiter möglich sein. Alle anderen haben keine Möglichkeit die Notfallkontaktdaten der Teilnehmenden zu exportieren. Die Notfallkontakte sind in keiner anderen Exportoption vorhanden, auch nicht bei "Alle Angaben".


## Technisch
#### Datenstruktur

![Notfallkontakt exportieren](/images/Datenbank.png)

Der Notfallkontakt ist eine neue Tabelle in der Datenbank, da erwartet wird das die aller meisten Notfallkontakte keine SAC Mitglieder sind. Auf dem UML ist die Person nur minimal gehalten. Eine Person kann mehrere Notfallkontakte angeben, und ein Notfallkontakt gehört zu genau einer Person. 

#### Weitere Infos

Für die Felder im Formular kann ein [labeled_inline_fields_for](https://github.com/hitobito/hitobito/blob/master/app/helpers/standard_form_builder.rb#L251) genutzt werden, die ganzen Views zu den Feldern selbst kommen in einen Unterornder mit dem Namen "emergency_contacts" in den Ordner [Person](https://github.com/hitobito/hitobito/tree/master/app/views/person)

Der Endpoint fürs Bearbeiten von Personendaten, bleibt der selbe. Den Export kann man hier hinzufügen https://github.com/hitobito/hitobito/tree/master/app/domain/export

Auf dem neu erstellten Model sollen die Attribute noch wie folgt validiert werden:
- Name: keine Validierung benötigt
- Email: email regex
- Telefonnummer: telefonnummer regex
- Beziehung: keine Validierung
