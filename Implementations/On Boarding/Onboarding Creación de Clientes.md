Creación de clientes dentro y fuera de Aurora

> [!IMPORTANT]
> - feat/client-product-accounts
> - feat/client-product-accounts-dev
> - feat/client-product-accounts-qa


> [!TODO] Cosas que hacer
> - Esos rangos de depositos y retiros
> - Quitar tarjeta de debito (priductos) que el la pueda reclamar donde quiera => SOLO EN LA INFORMATIVA
> - downloadDocuments

Entonces la recomendación para cuando mergees a master:

1. Avisar a quien genera el dist que hay una dependencia nueva.
2. Que antes de buildear corran npm ci (usa exactamente lo que dice package-lock.json, más seguro que npm install para producción) o al menos npm install.
3. Si hacen eso, el dist sale limpio y sin el problema que viste en dev.

Si simplemente copian una carpeta dist que ya tenían generada de antes (sin rebuildear tras el merge), tampoco hay riesgo — ese dist viejo ni siquiera tiene el código de la firma todavía.

![[Pasted image 20260827095257.png]]

```json
  {
    "taxCode": "08011990123456",
    "country": "HN",
    "secondaryTaxCode": "",
    "legalType": "2",
    "firstName": "Carlos",
    "secondaryName": "Alberto",
    "lastName": "Martinez",
    "secondaryLastName": "Lopez",
    "shortName": "Carlos Martinez",
    "status": "ACTIVE",
    "birthDate": "1990-05-15",
    "longName": "Carlos Alberto Martinez Lopez",
    "address": "Colonia Palmira, Tegucigalpa, Francisco Morazan",
    "email": "carlos.martinez@example.com",
    "officer": "1001",
    "sex": "M",
    "channel": "BRANCH",
    "pep": {
      "charge": "",
      "period": "",
      "organization": ""
    },
    "fatca": {
      "country": "HN",
      "taxCode": "08011990123456"
    },
    "products": [
      {
        "id": 1,
        "product": "AH01",
        "subProduct": "AH01",
        "currency": "HNL",
        "creditCount": 15,
        "creditAmount": 85000.5,
        "debitsCount": 10,
        "debitsAmount": 62350.75
      }
    ],
    "beneficiaries": [
      {
        "name": "Maria Fernanda Martinez",
        "taxcode": "08011995123457",
        "birthDate": "1995-08-20",
        "phone": "99991234",
        "adrress": "Colonia Palmira, Tegucigalpa",
        "citizenship": "HN",
        "country": "HN",
        "sex": "F",
        "percent": 100,
        "relatedProduct": 1
      }
    ],
    "services": {
      "debitCard": true,
      "banhcafeOnline": true
    }
  }
 
```