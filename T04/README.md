# T04: Implementació del Servidor d’Impressió - FoodLogistic S.A.

## 📝 Descripció del Projecte
En el sector logístic, la impressió de documents (albarans, fulls de transport) és un procés crític per garantir la sortida dels camions i mantenir la cadena de fred. Aquest projecte documenta la configuració d'un **Servidor d'Impressió centralitzat** sota Windows Server per gestionar el volum de treball del magatzem de FoodLogistic S.A.

L'objectiu principal és garantir l'alta disponibilitat mitjançant el **Printer Pooling** i l'automatització del desplegament als equips clients.

---

## ⚙️ Fases Tècniques de la Implementació

### 1. Preparació de l'Entorn i Simulació
* **Entorn:** Windows Server amb Active Directory (AD) operatiu.
* **Maquinari Simulat:** Instal·lació i configuració de dues instàncies d'impressora virtual (PDF24).
* **Nomenclatura Corporativa:** * `IMP_MAGATZEM_A`
  * `IMP_MAGATZEM_B`

### 2. Configuració del Rol i Printer Pooling
* **Rol de Servidor:** Instal·lació del servei *Print and Document Services*.
* **Gestió de Càrrega:** Activació de la funció **Enable printer pooling** a la consola de *Print Management*.
* **Objectiu:** Agrupar les dues impressores sota un mateix nom de xarxa per balancejar el treball de forma automàtica.

### 3. Desplegament Automatitzat (GPO)
* **Estratègia:** Creació d'una GPO anomenada `GPO_Impressores_Magatzem`.
* **Mètode:** Ús de la funció "Deploy with Group Policy" per vincular el recurs a la Unitat Organitzativa (OU) corresponent.
* **Experiència d'usuari:** Instal·lació transparent per als operaris del magatzem en iniciar la sessió al client Windows 11.

### 4. Seguretat i Proves de Càrrega
* **Optimització de Recursos:** Configuració de restriccions horàries (disponibilitat de 06:00 a 22:00).
* **Verificació del Balanceig:** Simulació de cua d'impressió amb enviament massiu de documents per validar el repartiment entre ports.

---

## 🚀 Solució Realitzada

### 🔗 Documentació de la Configuració
* **Nom del Domini:** [El teu domini .local]
* **GPO Aplicada:** `GPO_Impressores_Magatzem`
* **Port Virtual Pooling:** [Llista dels ports configurats]

### 📸 Evidències de la Implementació
> En aquest apartat s'inclouen les captures de pantalla que certifiquen el funcionament del sistema.

1. **Consola Print Management:** (Captura mostrant les dues impressores i el pooling actiu).
2. **GPO de Desplegament:** (Captura de l'editor de polítiques de grup).
3. **Client Windows 11:** (Captura del tauler de control del client amb la impressora ja instal·lada).
4. **Cua d'Impressió:** (Captura de la prova de càrrega de 10 documents).

---

[Guia](guia.md)