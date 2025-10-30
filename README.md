# derechos-reservados
LICENSE OS_CLEF OZCLEF



1) Si NO quieres que nadie use tu repo sin permiso

Opciones (de más seguro a menos seguro):

Haz el repo privado (más seguro): nadie puede verlo ni forkearlo salvo las personas que invites.

Si tiene que ser público pero no quieres reuso sin permiso:

Añade un archivo LICENSE (o LICENSE.md) con un texto claro tipo “All rights reserved — No reuse without written permission”.

Añade CONTRIBUTING.md y TERMS.md explicando que para usar/redistribuir deben pedir permiso.

Activa branch protection en main y usa CODEOWNERS para exigir revisiones.

Nota: en GitHub público no puedes impedir totalmente que otros forkeen el repo (los forks son permitidos). Si realmente no quieres forks, pon el repo privado.

Si tu prioridad es bloquear uso no autorizado → haz el repo privado. Si quieres mostrar trabajo pero restringir uso → publica y agrega licencia restrictiva + avisos legales.

2) Ejemplo de archivo LICENSE (All Rights Reserved — texto simple)

Copia esto en la raíz del repo como LICENSE o LICENSE.md:

Copyright (c) 2025 Oscar Cruz (ozclef)

All rights reserved.

This repository and its contents (code, assets, documentation) are private intellectual property of the author.
You may NOT copy, redistribute, publish, modify, fork, or use any part of this repository in any public or private project
without express written permission from the author.

To request permission, contact: tu-email@example.com


Explicación: es un aviso de derechos reservados. Legalmente es válido para dejar claro que no autorizas el reuso; sin embargo, si vas a perseguir legalmente una infracción, conviene asesoría legal formal y pruebas.

3) Alternativa: LICENSE.md con permiso condicionado (más informativo)

Si quieres dar permisos solo bajo ciertas condiciones (por ejemplo: uso educativo no comercial), usa algo como:

Copyright (c) 2025 Oscar Cruz (ozclef)

Permitted use:
- This repository may be used for personal and educational purposes only.
- Commercial use, redistribution, or public forks are prohibited without prior written permission.

Contact: tu-email@example.com for licensing requests.

4) Consejos prácticos relacionados con licencias y visibilidad

Si no quieres sorpresas, simplemente crea repos privados y comparte acceso a quien quieras.

Las licencias open-source (MIT, Apache, GPL) permiten reuso; no uses esas si tu intención es bloquear reuso.

Incluye un README con: “Este proyecto no está licenciado para uso público. Contacto: …” — ayuda a dejarlo claro en la página principal del repo.

Pon un pequeño header en archivos importantes (por ejemplo en index.html o archivos .js):

<!-- Copyright (c) 2025 Ozclef. All rights reserved. Unauthorized reuse prohibited. -->

5) quick-create.sh — script para clonar un repo template y reemplazar variables

Guarda el siguiente script como quick-create.sh, dale permisos chmod +x quick-create.sh y ejecútalo con ./quick-create.sh nombre-proyecto "Nombre de Cliente".

#!/usr/bin/env bash
# quick-create.sh - clona desde un template local o remoto y reemplaza placeholders básicos
# Uso: ./quick-create.sh my-new-site "El Rincón de la Hada"

set -e

TEMPLATE_REPO="https://github.com/ozclef/donation-landing-template.git"  # REEMPLAZA si usas otro template
NEW_NAME="$1"
CLIENT_NAME="$2"

if [ -z "$NEW_NAME" ] || [ -z "$CLIENT_NAME" ]; then
  echo "Uso: $0 <nombre-repo-nuevo> \"Nombre Cliente\""
  exit 1
fi

echo "Creando proyecto: $NEW_NAME para cliente: $CLIENT_NAME"

# Clona el template en una carpeta temporal
git clone --depth 1 "$TEMPLATE_REPO" "$NEW_NAME"
cd "$NEW_NAME"

# Borra el remoto del template
git remote remove origin || true

# Reemplaza algunos placeholders en archivos comunes
# (añade más patrones si tu template usa otros)
grep -RIl "__REPLACE_PROJECT_NAME__" . || true
# Usamos perl para reemplazos in-place compatibles
perl -pi -e "s/__REPLACE_PROJECT_NAME__/$NEW_NAME/g" $(git ls-files) 2>/dev/null || true
perl -pi -e "s/__REPLACE_CLIENT_NAME__/$CLIENT_NAME/g" $(git ls-files) 2>/dev/null || true

# Inicializa nuevo repo local
git init
git add .
git commit -m "Init: proyecto desde template para $CLIENT_NAME"

echo "Proyecto creado localmente en $(pwd). Ahora crea el repo en GitHub y agrega origin si quieres:"
echo "  gh repo create $NEW_NAME --public --source=. --remote=origin" 
echo "  git push -u origin main"

echo "Listo. Revisa archivos y ajusta CONFIG si es necesario."

Qué hace cada parte (explicación):

TEMPLATE_REPO: URL del template que clonas. Cámbiala a tu template.

git clone --depth 1: clona sólo la última versión para no traer todo el historial.

perl -pi -e ...: reemplaza en todos los archivos los placeholders __REPLACE_PROJECT_NAME__ y __REPLACE_CLIENT_NAME__. Puedes añadir más patrones.

Al final te recuerda crear el repo remoto (usa la CLI gh si la tienes).

6) ¿Qué es CONFIG en el HTML que te pasé?

El bloque CONFIG en el index.html es una configuración central en JavaScript para que no tengas que buscar por todo el código cuando quieras cambiar valores. Ejemplo:

const CONFIG = {
  whatsappNumber: "+521XXXXXXXXXXX",
  email: "tu@ejemplo.com",
  gofundme: "https://...",
  stripeLink: "...",
  restaurantName: "El Rincón de la Hada",
  currentAmount: 12450,
  goalAmount: 50000
};


Significado de campos:

whatsappNumber: número al que se abren los enlaces wa.me.

email: mail para mailto:.

gofundme, stripeLink: URLs de campaña o pago.

restaurantName: nombre del negocio que aparece en el mensaje.

currentAmount, goalAmount: cifras para la barra de progreso.

Beneficio: para personalizar el sitio solo editas esos valores y listo.

7) Otros términos rápidos y cómo te afectan

Branch protection: evita que main se mezcle sin PR aprobado. Recomendado para producción.

CODEOWNERS: archivo que obliga a ciertas personas a revisar PRs en determinadas rutas (útil si trabajas con otros).

Secrets: claves (Stripe API, tokens). No las metas en el repo; configúralas en GitHub Actions / Vercel.

Fork: copia pública que otros hacen. Si no quieres forks, haz repos privados.

README, CONTRIBUTING, TERMS: documentos que aclaran uso, contribuciones y condiciones legales. Úsalos para dejar claro que NO permites reuso.

8) ¿Toda esta info la puede estudiar alguien en un día? 🤯

Depende:

Si alguien ya sabe Git básico y HTML/CSS/JS: sí, puede leer y entender la mayoría en 1 día (80–90% de los conceptos prácticos).

Si es alguien nuevo total: sería mucho para un solo día — aprender los fundamentos de Git, deploy, licencias y scripting lleva varios días/semanas practicando (diría 20–30% asimilado en un día, 70–80% con práctica en 2–4 semanas).

Mi recomendación: divide en pasos prácticos — por ejemplo hoy: personalizar CONFIG y subir index.html. Mañana: hacer el LICENSE y probar quick-create.sh. Poco a poco aprendes todo sin saturarte.

C) Te genero un README corto que diga expresamente “No reuse sin permiso” y lo dejo listo.

D) Hago una mini-checklist para que publiques en GitHub Pages (paso a paso).
