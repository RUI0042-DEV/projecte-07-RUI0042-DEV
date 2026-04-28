# Informe Tècnic d'Implementació: Servidor d'Impressió amb Printer Pooling

## Índex
1. [Resum Executiu](#1-resum-executiu)
2. [Preparació de l'Entorn](#2-preparaci-de-lentorn)
3. [Instal·lació del Rol i Configuració del Pool](#3-instalació-del-rol-i-configuració-del-pool)
4. [Desplegament Automatitzat mitjançant GPO](#4-desplegament-automatitzat-mitjançant-gpo)
5. [Prova de Càrrega i Seguretat](#5-prova-de-càrrega-i-seguretat)

---

## 1. Resum Executiu

**Context:** L'empresa disposa d'un magatzem on la impressió d'albarans és crítica per a la cadena de fred. Una fallada o col·lapse d'impressora implica retards en la sortida de camions.

**Objectiu:** Implementar un servidor d'impressió resilient a Windows Server mitjançant **Printer Pooling** (cua compartida) per balancejar la càrrega entre dos dispositius virtuals PDF24.

**Tecnologies utilitzades:**
- Windows Server (AD DS + Print and Document Services)
- PDF24 (simulació de maquinari d'impressió)
- Group Policy Objects (GPO) per desplegament automatitzat
- Windows 11 Client (domini)

---

## 2. Preparació de l'Entorn

### 2.1 Verificació de l'Entorn

| Component | Requisit | Estat |
|-----------|----------|-------|
| Windows Server (2019/2022) | Amb rol AD DS instal·lat | ✅ Verificar |
| Windows 11 Client | Unit al domini | ✅ Verificar |
| Xarxa | Connectivitat servidor-client | ✅ Verificar |
| PDF24 | Instal·lat al servidor | ⬜ Instal·lar |

> **Justificació:** És imprescindible que el servidor ja tingui Active Directory configurat, ja que la GPO requereix un domini funcional. El client ha d'estar unit al domini per rebre les directives de grup.

### 2.2 Instal·lació de PDF24 al Servidor

**Pas 1:** Descarregar PDF24 des de [https://tools.pdf24.org/en/creator](https://tools.pdf24.org/en/creator)

**Pas 2:** Executar l'instal·lador amb permisos d'administrador.

**Pas 3:** Durant la instal·lació, seleccionar:
- ✅ "PDF Printer" (imprescindible per crear impressores virtuals)
- ✅ "PDF24 PDF Converter" (opcional, per gestió de documents)

![alt text](<pics/Captura de pantalla 2026-04-28 152726.png>)

> **Nota tècnica:** PDF24 crea per defecte una impressora anomenada "PDF24". Aquesta serà la base per crear les nostres dues instàncies corporatives.

### 2.3 Configuració de les Dues Instàncies d'Impressora

#### Instància A: IMP_MAGATZEM_A

1. Obrir **Settings → Bluetooth & devices → Printers & scanners**
2. Clicar **Add device → Add manually**
3. Seleccionar **"Add a local printer or network printer with manual settings"**

![alt text](<pics/Captura de pantalla 2026-04-28 153025.png>)

4. Triar **"Use an existing port"** → seleccionar `PDF24:` (o crear port `PDF24_A:`)
5. Seleccionar el driver **PDF24** de la llista
6. Al nom de la impressora, introduir: **`IMP_MAGATZEM_A`**

![alt text](<pics/Captura de pantalla 2026-04-28 154651.png>)

7. **NO** compartir encara (ho farem des de Print Management)
8. Finalitzar

#### Instància B: IMP_MAGATZEM_B

Repetir el procés anterior amb els següents canvis:
- Port: `PDF24_B:` (o un segon port virtual)
- Nom: **`IMP_MAGATZEM_B`**

![alt text](<pics/Captura de pantalla 2026-04-28 154904.png>)

> **Justificació de la nomenclatura:** El prefix `IMP_` identifica el recurs com a impressora. `MAGATZEM` especifica la ubicació/departament. El sufix `_A` i `_B` diferencia les dues unitats del pool. Aquesta convenció facilita la gestió i el troubleshooting en entorns corporatius.

---

## 3. Instal·lació del Rol i Configuració del Pool

### 3.1 Instal·lació del Rol "Print and Document Services"

**Mètode 1: Via Server Manager (GUI)**

1. Obrir **Server Manager → Manage → Add Roles and Features**
2. Clicar **Next** fins a "Server Roles"
3. Marcar **"Print and Document Services"**
4. Al popup, clicar **"Add Features"**
5. Clicar **Next** fins a "Role Services"
6. Verificar que està marcat:
   - ✅ **Print Server**
   - ✅ **Internet Printing** (opcional, per accés web)
   - ✅ **LPD Service** (opcional, per compatibilitat Linux/Unix)
7. Clicar **Install** i esperar la finalització
8. **Close**

### 3.2 Accés a la Consola Print Management

1. Obrir Server Manager → Tools → Print Management

O bé executar: printmanagement.msc

2. Expandir Print Servers → [NOM_DEL_SERVIDOR]

### 3.3 Configuració del Printer Pooling

Aquest és el nucli tècnic de l'activitat. El Printer Pooling permet que múltiples impressores físiques (o virtuals) treballin sota un sol nom lògic, distribuint automàticament les peticions.

Pas a pas:

1. A la consola Print Management, localitzar IMP_MAGATZEM_A

2. Clicar dret → Properties (o doble clic)

3. Anar a la pestanya "Ports"

4. Marcar la casella "Enable printer pooling"

Aquesta opció permet seleccionar múltiples ports per a una mateixa impressora lògica.

5. Seleccionar BOTH ports:

- ✅ Port de IMP_MAGATZEM_A (ex: PDF24_A:)
- ✅ Port de IMP_MAGATZEM_B (ex: PDF24_B:)

Els dos ports han de quedar marcats simultàniament

6. Clicar Apply → OK

![alt text](<pics/Captura de pantalla 2026-04-28 155309.png>)

Justificació tècnica: Quan un usuari envia un document a IMP_MAGATZEM_A, el Print Spooler de Windows assigna la feina al primer port disponible. Si IMP_MAGATZEM_A està ocupada, la petició va automàticament a IMP_MAGATZEM_B. Això elimina els colls d'ampolla sense intervenció de l'usuari.

## 4. Desplegament Automatitzat mitjançant GPO

### 4.1 Creació de la Unitat Organitzativa (OU)
**Recomanació:** Crear una OU específica per als usuaris o equips del magatzem per facilitar la gestió de polítiques.

1. Obrir **Active Directory Users and Computers** (`dsa.msc`).
2. Fer clic dret sobre el domini → **New** → **Organizational Unit**.
3. **Nom:** `OU_Magatzem`.
4. Moure l'usuari de proves (o els equips de xarxa) a aquesta OU.

![alt text](<pics/Captura de pantalla 2026-04-28 155630.png>)

### 4.2 Creació de la GPO
1. Obrir **Group Policy Management** (`gpmc.msc`).
2. Fer clic dret sobre `OU_Magatzem` → **"Create a GPO in this domain, and Link it here..."**.
3. **Nom de la GPO:** `GPO_Impressores_Magatzem`.
4. Fer clic dret sobre la nova GPO → **Edit**.

![alt text](<pics/Captura de pantalla 2026-04-28 155921.png>)

### 4.3 Desplegament de la Impressora des de Print Management
Aquest és el mètode recomanat, ja que és més intuïtiu i redueix el marge d'error en la configuració de rutes.

1. Obrir **Print Management** (`printmanagement.msc`).
2. Localitzar **IMP_MAGATZEM_A** (la impressora configurada al pool).
3. Fer clic dret sobre la impressora → **"Deploy with Group Policy..."**.
4. Al diàleg que apareix:
    * Clicar **Browse...** al camp "GPO".
    * Seleccionar la GPO creada anteriorment: `GPO_Impressores_Magatzem`.
    * Marcar **"The computers that this GPO applies to (per machine)"**: Això garanteix que qualsevol usuari que iniciï sessió en aquest equip vegi la impressora.
    * *Opcionalment:* Marcar **"The users that this GPO applies to (per user)"** per a què la impressora segueixi l'usuari a altres equips.
5. Clicar **Add** → **OK**.

![alt text](<pics/Captura de pantalla 2026-04-28 162031.png>)


> **Justificació:** El desplegament **"per machine"** és ideal per a entorns compartits com un magatzem, on diversos empleats fan servir el mateix terminal. El desplegament **"per user"** és més adequat per a oficines on l'usuari és qui es mou entre diferents llocs de treball.

### 4.4 Verificació de la Configuració de la GPO
Dins de l'editor de la GPO, cal verificar que s'ha creat la configuració correctament en una d'aquestes rutes:

``` plain
User Configuration (or Computer Configuration)
  → Policies
    → Windows Settings
      → Deployed Printers
        → \\NOM_SERVIDOR\IMP_MAGATZEM_A
```

![alt text](<pics/Captura de pantalla 2026-04-28 162136.png>)

## 4.5 Prova al Client Windows 11

Pas 1: Al client, obrir un Command Prompt com a Administrador

Pas 2: Forçar l'actualització de les directives:

``` cmd
gpupdate /force
```

> *Nota tècnica: gpupdate /force descarta la memòria cau de les GPO i les descarrega de nou des del controlador de domini. És essencial després de qualsevol canvi en GPO.*

Pas 3: Si es demana, tancar la sessió i tornar a iniciar-la

Pas 4: Verificar la impressora:

- Settings → Bluetooth & devices → Printers & scanners
- Ha d'aparèixer IMP_MAGATZEM_A amb estat "Ready"

![alt text](<pics/Captura de pantalla 2026-04-28 163005.png>)

Pas 5: Prova d'impressió:

Obrir qualsevol document (Notepad, Word, etc.)

Ctrl + P → seleccionar IMP_MAGATZEM_A

Imprimir → verificar que es genera un PDF a la carpeta de PDF24

![alt text](<pics/Captura de pantalla 2026-04-28 163556.png>)

## 5. Prova de Càrrega i Seguretat

### 5.1 Simulació de Balanceig de Càrrega
**Objectiu:** Verificar que la funció de *Printer Pooling* distribueix correctament les peticions d'impressió entre les dues instàncies configurades.

#### Mètode de prova:
1. Obrir **Print Management** al servidor.
2. Navegar fins a **Print Servers** → `[Nom_del_Servidor]` → **Printers**.
3. Fer clic dret a `IMP_MAGATZEM_A` → **"Open Printer Queue"** (o "See what's printing").
4. Des de l'equip client (o des del propi servidor), enviar **10 documents** de forma consecutiva:
    * Obrir 10 fitxers de text diferents amb el **Notepad**.
    * Enviar-los a imprimir ràpidament un darrere l'altre seleccionant la impressora compartida.

#### Observació del comportament:

| Escenari esperat | Resultat |
| :--- | :---: |
| Els documents 1-5 s'assignen a `IMP_MAGATZEM_A` | ✅ |
| Els documents 6-10 es desvien a `IMP_MAGATZEM_B` | ✅ |
| No es produeix acumulació de cua d'espera (bottleneck) | ✅ |

![alt text](<pics/Captura de pantalla 2026-04-28 163610.png>)

> **Nota tècnica:** En una configuració de *pool*, el servei de *spooler* assigna la feina al primer port disponible. Si ambdues impressores estan lliures, la primera petició va al primer port de la llista, la segona al segon, i així successivament. Aquest mecanisme garanteix la màxima distribució de càrrega i eficiència en el servei.

### 5.2 Configuració de Restriccions d'Horari (Available Times)

**Objectiu:** Limitar l'ús de la impressora exclusivament a l'horari laboral (**06:00 - 22:00**) per evitar impressions nocturnes no autoritzades, estalviar energia i optimitzar els recursos del magatzem.

#### Pas a pas:
1. Dins de **Print Management**, fer clic dret sobre la impressora `IMP_MAGATZEM_A` → **Properties**.
2. Seleccionar la pestanya **Advanced**.
3. Canviar l'opció per defecte de "Always available" a **"Available from"**.
4. Configurar el rang horari permès:
    * **From:** `6:00 AM`
    * **To:** `10:00 PM`
5. Clicar a **Apply** i després a **OK** per confirmar els canvis.


![alt text](<pics/Captura de pantalla 2026-04-28 163723.png>)

> **Nota:** Si un usuari intenta enviar un document fora d'aquest horari, el treball d'impressió quedarà retingut a la cua del servidor (Spooler) i no s'imprimirà fins que s'arribi a l'hora d'inici configurada (les 06:00 AM de l'endemà).

## Conclusió

Amb aquesta implementació, l'empresa disposa d'un servidor d'impressió **resilient, escalable i fàcilment gestionable**:

* **Resiliència:** Gràcies al *Printer Pooling*, si una impressora falla o es queda sense paper, l'altra continua processant els documents de la cua sense necessitat d'intervenció manual ni reconfiguracions per part de l'usuari.
* **Escalabilitat:** El sistema permet créixer segons les necessitats del magatzem; es poden afegir més impressores físiques al pool simplement seleccionant ports addicionals a la configuració existent.
* **Gestió centralitzada:** L'ús de **GPO** permet desplegar la configuració de forma automàtica i restringir l'accés de manera global, eliminant la necessitat de visitar físicament cada equip de la xarxa.
* **Seguretat:** Les restriccions d'horari i la gestió via Unitats Organitzatives (OU) limiten l'ús de la infraestructura estrictament a l'àmbit laboral i al personal autoritzat.

> **Recomanació final:** Es suggereix documentar en un **inventari d'infraestructura** la configuració específica del pool, els noms de les GPO i les OU afectades. Això facilitarà enormement les futures auditories de sistemes i les tasques de manteniment preventiu.

[Tornar enrere](README.md)
