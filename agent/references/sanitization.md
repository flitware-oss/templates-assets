# Sanitization policy

Examples are blank forms: variables + static legal prose. They must
never carry data that identifies a real company, person, vehicle or
account — even "just as an example".

## Banned in files, READMEs and skill docs

- Company names, NITs, addresses, phones, emails, websites, logos.
- Person names, ID numbers, signatures.
- Plates, VIN/engine numbers, policy/account numbers, amounts tied to
  real cases, tracking codes, internal system names.
- Credentials, tokens, internal URLs.

## How to sanitize a source document

1. Replace every data point with its variable (`$collection.*`,
   `$tenant.*`, `$system.*`, `$input.*`) or drop it when decorative.
2. `$input` defaults must be generic procedures/places, never personal
   data (`"TRASPASO"` ok; a real bank account never).
3. Descriptions show the *shape* of an answer with clearly generic
   examples (`Ej.: "19 pasajeros"`; `"a término indefinido, …"`).
4. Static prose keeps only public content (laws, decree numbers,
   boilerplate). Normalize typos; generalize client-specific systems
   ("los sistemas de la compañía").
5. Filenames, template `name`/`description` and README tables use the
   document kind only ("Certificado de ingresos del propietario").

## Pre-publish grep

Before publishing, grep the new/changed files for the *source
document's own* tokens (company name and NIT, person names, plates,
phones, emails, internal codes): nothing may match. Keep that token
list private — never commit it. When in doubt, variable-ize it:
a template that renders nothing without data is always safe.
