# T09: Estimació temporal de projecte (Diagrama de Gantt professional)

## 📝 Descripció
Un dels errors més habituals en equips tècnics novells és confondre “fer feina” amb “gestionar un projecte”. Començar a configurar servidors, desenvolupar una web o desplegar serveis sense una planificació rigorosa acostuma a provocar retards, colls d’ampolla i solucions improvisades.

En aquest punt del projecte, la direcció de **FoodLogístics S.A.** no vol només una proposta tècnica: **vol garanties**. 

Vol saber:
* Quan estaran les solucions operatives.
* Quin ordre seguireu.
* Què passarà si alguna tasca es retarda.
* Si sou capaços de treballar com un equip professional.

Per tant, el vostre objectiu no és “fer un Gantt”, sinó demostrar que sabeu planificar un projecte real amb criteri tècnic i organitzatiu.

---

## 🎯 Objectius específics de la tasca
Treballareu com a equip de projecte per construir una planificació completa i realista del desplegament de la solució per a FoodLogístics S.A., utilitzant un diagrama de Gantt generat amb **PlantUML (UMLTree)**.

---

## 🏗️ Fases del projecte

### Fase 1: Anàlisi real del projecte (pensament estructural)
**1.1 Identificació de tasques i dependències**
A partir de les tasques reals del projecte (T01–T08), identifiqueu:
* Ordre lògic d’execució.
* Tasques que poden anar en paral·lel.
* Tasques bloquejants.

**1.2 Identificació del camí crític**
Determineu:
* Quines tasques, si es retarden, afecten tot el projecte.
* Quines tenen marge (*slack*).

### Fase 2: Estimació d’esforç amb criteri (ús d’IA guiat)
Heu d’estimar la durada de cada tasca en hores considerant:
* Temps de comprensió i recerca.
* Temps d’implementació tècnica i proves.
* Temps de documentació i coordinació.
* Temps de marge (imprevistos).
* **Ús d’IA:** Obligatori per assistir en l'estimació, però validat amb el vostre criteri.

### Fase 3: Assignació de recursos (treball en equip real)
Distribuïu les tasques entre els membres de l’equip:
* Eviteu la sobrecàrrega d’una persona.
* Eviteu els temps morts en altres membres.

### Fase 4: Construcció del diagrama de Gantt (UMLTree)
Utilitzant PlantUML, el diagrama ha de mostrar:
* Tasques (T01–T08).
* Durada i dependències.
* Execució en paral·lel.
* Visió temporal de les **3 setmanes**.

### Fase 5: Pla de contingència (pensament professional)
Identifiqueu com a mínim **2 riscos crítics reals** i definiu l'impacte i l'estratègia de mitigació.

---

## 📦 Què cal lliurar
El lliurament s’haurà de fer dins del repositori, a la carpeta: `/P01/Planificacio/`.

### 1. Document de planificació (`README.md`)
Informe professional que inclogui:
* Introducció i context.
* Justificació de les estimacions i dependències.
* Explicació setmana a setmana de la distribució del treball.
* Reflexió sobre colls d'ampolla.
* Captures del diagrama.

### 2. Codi PlantUML i imatge del diagrama
* Arxiu amb el codi font per generar el Gantt.
* Imatge exportada (PNG/SVG).

### 3. Matriu d’assignació de responsabilitats (RACI)
Taula que indiqui qui és:
* **R** (*Responsible*): Qui executa.
* **A** (*Accountable*): Qui valida.
* **C** (*Consulted*): Qui assessora.
* **I** (*Informed*): Qui rep informació.

### 4. Taula de riscos i contingències
Taula amb: problema, part afectada, impacte i mesura de resposta.

---

## ❓ Preguntes clau (Obligatòries al README)
1. Quina és la tasca més crítica del projecte i per què?
2. On heu detectat el principal coll d’ampolla?
3. Quina decisió de planificació ha estat més difícil?
4. Heu hagut de modificar alguna estimació inicial? Per què?
5. Quin risc podria fer fracassar el projecte?
6. Si tinguéssiu una setmana més, què canviaríeu?

---

## ✅ Criteris de qualitat
* **Coherència** del Gantt amb el projecte real.
* **Realisme** de les estimacions (no arbitràries).
* **Qualitat del README** (professional, estructurat, justificat).
* **Ús crític de la IA** (no superficial).

[Planificacio](Planificacio.md)