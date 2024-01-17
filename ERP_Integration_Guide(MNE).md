
# Vodič za Integraciju Perper.net sa ERP Sistemom

Ovaj vodič pruža detaljna uputstva za integraciju sistema za plaćanje stabilnom valutom Perper u vaš ERP sistem. Slijedite ove korake za efikasno postavljanje i upravljanje transakcijama.

## Početak

Da biste započeli proces integracije, potrebno je da obezbijedite sljedeće informacije za kreiranje projekta na Perper.net - https://sdk.perper.net:

1. **Ime Projekta**: Naziv vašeg projekta.
2. **Redirect URL**: URL na koji će korisnici biti preusmjereni nakon transakcije.
3. **API Endpoint**: URL vašeg backend-a na koji će rezultati transakcija biti poslati.

Nakon kreiranja projekta, dobićete:

- **ID Projekta**
- **Security Code Projekta**

Ovi podaci su ključni za proces integracije i verifikaciju transakcija.

## Zahtjevi za Integraciju sa ERP Sistemom

Za uspješnu integraciju sa vašim ERP sistemom, potrebno je:

- **API Endpoint**: URL vašeg backend-a.
- **ID Projekta**: Pruža Perper.net.
- **Security Code Projekta**: Pruža Perper.net.

## Obrada Transakcija

### Primanje Rezultata Transakcija

Vaš API Endpoint će primati rezultate transakcija u sljedećem formatu:

- **Za neuspješnu transakciju**:
  ```json
  {
    "status": "fail",
    "data": "string",
    "hash": "string"
  }
  ```

- **Za uspješnu transakciju**:
  ```json
  {
    "status": "success",
    "data": "string",
    "hash": "string",
    "transactionId": "string"
  }
  ```
  - `transactionId` je naš interni ID za izvršenu transakciju.

### Kreiranje QR Koda za Perper Plaćanja

Da biste kreirali QR kod za Perper plaćanja, koristite JSON string u ovom formatu:

```json
{
  "amount": number,
  "projectId": "string",
  "data": "string"
}
```

- `amount`: Količina Perpera koja treba biti poslata.
- `projectId`: Vaš ID Projekta.
- `data`: String (može biti nasumično generisan ili vaš interni ID transakcije) koji će kasnije biti korišćen za verifikaciju.

### Proces Verifikacije

Polje `data` će biti poslato nazad na vaš API Endpoint, zajedno sa potpisanim JWT koji sadrži polje `hash`, koristeći vaš Security Code Projekta. Koristite ova polja (`data` i `hash`) da potvrdite da li je zahtjev zaista od nas ili se radi o pokušaju prevare.

## Sigurnost i Validacija

- Osigurajte sigurnost vašeg ID-a Projekta i Security Code Projekta.
- Validirajte svaku transakciju poređenjem polja `data` i `hash` sa vašim Security Code Projekta.

## Podrška

Za pomoć ili više informacija, kontaktirajte [support@perper.net].

