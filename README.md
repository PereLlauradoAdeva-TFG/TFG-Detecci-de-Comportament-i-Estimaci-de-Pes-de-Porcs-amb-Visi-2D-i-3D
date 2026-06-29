# Detecció de Comportament i Estimació de Pes de Porcs amb Visió 2D i 3D

**Repositori oficial:** [https://github.com/PereLlauradoAdeva-TFG/TFG-Detecci-de-Comportament-i-Estimaci-de-Pes-de-Porcs-amb-Visi-2D-i-3D.git](https://github.com/PereLlauradoAdeva-TFG/TFG-Detecci-de-Comportament-i-Estimaci-de-Pes-de-Porcs-amb-Visi-2D-i-3D.git)

## Resum del Projecte (TFG)

L’augment de prop d’un 20% de la població porcina a la Península Ibèrica durant els últims 10 anys fa inviable que els ramaders, sense els recursos tecnològics adequats, puguin controlar el benestar, l’activitat i el pes de cadascun dels animals. Aquest projecte busca entrenar models d’IA capaces d’automatitzar el monitoratge de l’activitat i el benestar del bestiar, amb l’objectiu d'apropar aquesta tecnologia a les petites explotacions per millorar-ne la competitivitat, alhora que permet a les grans instal·lacions optimitzar l'eficiència i garantir el benestar dels porcs. 

A més, a causa de les dificultats en l’obtenció de dades necessàries per a predir el pes, s’ha investigat el camp dels Espais de Dades i se n’ha creat una representació a petita escala. Aquesta iniciativa de la Unió Europea permet la col·laboració entre diferents empreses per entrenar models d’intel·ligència artificial sense necessitat de compartir explícitament les seves dades, ni dependre de les dades d’una sola empresa. Gràcies a la recerca duta a terme en aquest projecte, s’ha desenvolupat un model altament precís i robust, capaç d’operar en diferents granges, condicions de lluminositat i genètica de porcs.

---

## ⚠️ Configuració Prèvia (Rutes)

Per garantir la portabilitat del codi, els camins dins dels scripts s'han parametritzat utilitzant rutes relatives `os.path.join(RUTA_BASE, ...)`. **És imprescindible** modificar la variable `RUTA_BASE` a l'inici de cada Notebook perquè apunti al directori local on hàgiu clonat aquest repositori. 
> **Excepció:** El document `05_processament_3D_homografia.ipynb` no requereix aquest canvi de ruta.

---

## 📂 1. Arxius de Codi Principals (Notebooks)

Tot el codi principal està estructurat en Notebooks de Jupyter (`.ipynb`) ordenats de forma seqüencial segons el flux de treball:

* **`01_processament_dades_2D.ipynb`:** S'encarrega del preprocessament inicial. Conté els bucles d'extracció i retall d'imatges (*crops*) a partir dels vídeos originals per preparar la base de dades visual abans de l'entrenament.
* **`02_experiments_seleccio_model_2D.ipynb`:** Dedicat a l'experimentació. S'hi fan proves de concepte i es validen els diferents models de detecció per seleccionar-ne el candidat òptim.
* **`03_entrenament_2D_DEFINITIU.ipynb`:** Script principal (*core*). Entrena i executa l'arquitectura 2D combinant el model YOLOv8m-seg (segmentació d'instàncies), el *tracker* BoT-SORT, i un mòdul Re-ID amb un Autoencoder de PyTorch (que extreu un espai latent de 512 dimensions) i l'Algorisme Hongarès. A més, genera un *dashboard* en vídeo amb mètriques en temps real.
* **`04_dataspace_federated.ipynb`:** Orquestra la simulació d'aprenentatge federat (*Federated Learning*) utilitzant les llibreries `flwr` (Flower) i Ray per entrenar models de forma descentralitzada entre diferents nodes.
* **`05_processament_3D_homografia.ipynb`:** Mòdul espacial 3D. Rep dades de profunditat i núvols de punts (*Point Clouds*) de càmeres ToF i calcula matrius d'homografia per projectar els punts 3D a una vista d'ocell (*Bird's-Eye View*), alineant mapes RGB amb coordenades físiques reals del terra de la granja.
* **`06_Diagrama_de_gantt.ipynb`:** Script secundari de gestió enfocat a la planificació temporal que genera gràfiques (Diagrames de Gantt) per avaluar el calendari de tasques del TFG.

---

## 📂 2. Carpetes de Dades i Recursos

Aquest repositori conté carpetes amb dades d'exemple per poder provar els models i l'arquitectura sense haver de descarregar conjunts de dades massius:

* **`3D_images/`:** Conté les dades espacials capturades per càmeres ToF. Inclou arxius de núvols de punts (`.ply`) i mapes de profunditat (`.tiff`) necessaris per executar el Notebook 05.
* **`Exemple_dataset_dataspaces/`:** Simula els espais de dades descentralitzats per al funcionament de l'aprenentatge federat. Es divideix en dues subcarpetes principals que actuen com a nodes aïllats:
    * `Granja Antiga/`
    * `Granja_Nova/`

### 📌 Estructura i limitacions del Dataset d'Exemple
A dins de cadascuna d'aquestes carpetes s'hi reprodueix l'estructura de directoris estàndard necessària per entrenar models YOLO. L'arbre de fitxers de cada granja és el següent:

```text
Granja_Nova/ (i Granja Antiga/)
│── data.yaml
│── images/
│   ├── train/ (16 imatges d'entrenament)
│   └── val/   (4 imatges de validació)
│── labels/
│   ├── train/ (16 arxius d'etiquetes)
│   └── val/   (4 arxius d'etiquetes)
└── videos/    (1 vídeo original d'exemple per granja)

---
**Autor:** Pere Llauradó Adeva (NIA: 1669502)
