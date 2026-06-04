Deze PR lost een kritieke synchronisatiebug op in de Eczeem Smeerapp.

Probleem:
De app gebruikte localStorage direct als initiële bron van waarheid. Op een nieuw device of in een nieuwe browser was die lokale data leeg. Daardoor kon de app het setup-scherm tonen en vervolgens met DOC.set(next) het volledige Firestore-document overschrijven. Hierdoor konden bestaande log- en plekken-gegevens verloren gaan wanneer iemand met dezelfde familiecode inlogde op een nieuw apparaat.

Oplossing:

De app initialiseert data nu met null en wacht eerst op Firestore.
Een niet-bestaand Firestore-document wordt niet meer automatisch gevuld met lokale data.
Firebase lees- en schrijffouten worden zichtbaar gemaakt met console errors en alerts.
Er is een updatedAt-veld toegevoegd zodat zichtbaar is wanneer data voor het laatst is opgeslagen.
De familiecode wordt opgeslagen en gebruikt om het Firestore-document te bepalen.

Waarom:
Hiermee wordt voorkomen dat een nieuw device of een lege browsercache bestaande centrale data overschrijft met lege lokale state.

Test:

App geopend in bestaande browser met data.
App geopend in nieuwe browser zonder lokale data.
Gecontroleerd dat nieuwe browser niet automatisch lege lokale data naar Firebase pusht.
Gecontroleerd dat data pas wordt geschreven na geldige Firebase-load en gebruikersactie.
