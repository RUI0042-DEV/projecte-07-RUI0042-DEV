# Explicació de la solució – Web Corporativa FoodLogistic S.A.

## 1. Desplegament (GitHub Pages)

La web s'ha publicat correctament a GitHub Pages i és accessible sota una URL pública del tipus `https://github.com/RUI0042-DEV/web-corporativa`. La pàgina és totalment operativa i inclou totes les seccions demanades: **Inici, Serveis, Sobre nosaltres i Contacte**, amb el formulari de contacte amb tots els camps obligatoris (nom, correu, telèfon i missatge). El disseny és responsiu i funciona correctament tant en escriptori com en dispositius mòbils.

---

## 2. Estructura del Repositori

El repositori segueix estrictament l'estructura exigida per GitHub Pages:

```
docs/
├── index.html
├── styles.css
README.md
```

Tot el codi de la web es troba dins la carpeta `/docs`, amb el fitxer principal anomenat `index.html` tal com requereix GitHub Pages per reconèixer-lo com a pàgina d'inici. Els estils estan separats en un fitxer `styles.css`.

---

## 3. Documentació (README.md)

El fitxer `README.md` a l'arrel del repositori inclou:

- **Descripció** del projecte i de l'empresa fictícia
- **URL pública** de la web desplegada
- **Estructura de fitxers** del projecte
- **Tecnologies utilitzades**, citant explícitament l'eina d'IA emprada (Claude, Anthropic)
- **Informació sobre normativa** complerta (LOPDGDD i LSSI-CE)
- **Secció d'analítica** reservada per afegir les captures de Statcounter
- **Autor** i context del mòdul

---

## 4. Control i Analítica (StatCounter)

S'ha integrat el comptador invisible de Statcounter just abans del tancament de la etiqueta `</body>` a l'`index.html`. Es va triar la modalitat **Invisible Counter** per no mostrar cap element visible als visitants. Un cop la web ha rebut visites, s'han capturat evidències del tauler de Statcounter que mostren dades com el nombre de visitants únics, el país d'origen, el dispositiu utilitzat i les hores de més activitat. Aquestes captures s'han inclòs al README.md i a la documentació de la carpeta T02.

![alt text](<pics/Captura de pantalla 2026-04-13 162925.png>)

![alt text](<pics/Captura de pantalla 2026-04-13 202915.png>)

---

## 5. Workflow i Versionat

El desenvolupament s'ha fet seguint una estratègia de **treball local primer, publicació després**. S'han anat fent commits regulars al llarg del procés amb missatges descriptius.

![alt text](<pics/Captura de pantalla 2026-04-13 204439.png>)