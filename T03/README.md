# T03: Servidor de Fitxers - FoodLogistic 🚛📦

## 📝 Introducció
A mesura que **FoodLogistic** ha crescut, també ho ha fet el seu volum de dades. Per evitar la compartimentació de la informació i millorar l'eficiència, s'ha dissenyat una infraestructura de fitxers centralitzada i segura. 

L'objectiu d'aquest projecte és implementar un entorn de fitxers organitzat mitjançant permisos **NTFS/SMB**, **quotes** i **filtratge de fitxers (FSRM)**, utilitzant les tres vies principals d'administració: Explorador de fitxers, Server Manager i PowerShell.

---

## 🛠️ Descripció de l'Activitat

### 1. Preparació i Seguretat de Grups (AD)
S'ha configurat el domini `foodlogistic.test` amb una estructura d'Unitats Organitzatives (OU) justificada i els següents grups de seguretat:
* **Administracio**: Gestió de factures i albarans.
* **Transport**: Xofers i caps de flota.
* **Direccio**: Gerència.

### 2. Implementació de Recursos Compartits
S'han creat els següents recursos mitjançant metodologies específiques per demostrar el domini de l'entorn Windows Server:

| Carpeta | Mètode | Accés / Restricció | Configuració Especial |
| :--- | :--- | :--- | :--- |
| **Public** | Explorador d'arxius | Tothom | NTFS: Modificació / SMB: Lectura |
| **Operacions** | Server Manager (FSSM) | Grup Transport | Access-Based Enumeration (ABE) |
| **Confidencial** | PowerShell Avançat | Grup Direccio | Recurs ocult (`Direccio$`), ABE actiu i Mapatge Unitat Z: |

> **Nota sobre el Mètode D:** S'ha utilitzat el cmdlet `New-SmbShare` i s'ha habilitat l'ABE mitjançant PowerShell per a una gestió avançada.

### 3. Control d'Emmagatzematge
Per optimitzar l'espai en disc i evitar l'ús inadequat (fotos personals, executables), s'han aplicat:

* **Quotes NTFS (Volum):** Límit de **500 MB** per defecte per a nous usuaris.
* **FSRM - Quota de Carpeta:** A la carpeta `Public`, s'aplica una **Hard Quota de 200 MB** amb avís personalitzat al 90%.
* **FSRM - Filtratge de Fitxers:** A la carpeta `Operacions`, es prohibeixen fitxers executables (`.exe`, `.msi`) i fitxers multimèdia (àudio/vídeo).

---

## 🔍 Verificació i Auditoria
Es realitzen proves des de clients Windows 10/11 per confirmar:
1.  **Visibilitat selectiva:** Els usuaris només veuen els recursos per als quals tenen permisos (ABE).
2.  **Eficàcia del filtre:** Verificació del filtratge actiu (fins i tot canviant extensions a `.txt`).
3.  **GPO:** Comprovació que la Unitat Z: apareix automàticament als usuaris de Direcció.

---

## 📁 Contingut del Lliurament
L'informe tècnic detallat en format MarkDown inclou:
* **Resum de Configuració:** Taula amb noms de carpeta, camins UNC i permisos.
* **Evidències:** Captures de pantalla comentades de cada configuració (Server Manager, PowerShell, FSRM).
* **Proves de funcionament:** Validació real des del costat del client.


---
*Material de suport: 0224 SOX. Material UD8: AA2.*

[Guia](Guia.md)