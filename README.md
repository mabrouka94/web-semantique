Ce dépôt contient un ensemble de données RDF représentant un hôpital générique, développé dans le cadre d’un projet de cours de troisième cycle sur le Web Sémantique.
L’objectif principal de ce projet est de modéliser les entités, les relations et les informations médicales d’un hôpital de manière structurée et sémantique, en utilisant les technologies du Web sémantique telles que RDF (Resource Description Framework) et RDFS (RDF Schema). L'ontologie ainsi développée permet de représenter les différentes composantes d’un hôpital — patients, médecins, chambres, départements, dossiers médicaux, etc. — de manière formelle et interopérable.
Grâce à cette ontologie, il est possible d'effectuer des requêtes SPARQL afin d'extraire des informations sémantiques précises et pertinentes à partir des données RDF. Cela permet d'exploiter pleinement le potentiel du Web sémantique pour améliorer l'accès aux données, la recherche d'informations, et la déduction logique au sein d’un système hospitalier.
Domaine choisi :
Le domaine de cette ontologie est la gestion hospitalière, avec un focus sur la structuration des informations médicales, des patients, docteurs, chambres, lits, mises à jour médicales, et dossiers. L'objectif est de représenter de façon sémantique les interactions complexes entre les entités du système hospitalier pour faciliter l'interopérabilité, la recherche, la mise à jour et l’inférence de nouvelles connaissances.

Modèle utilisé :
Nous avons utilisé un modèle orienté objets, adapté en RDF/RDFS/OWL, représentant les entités clés comme des classes, et les relations entre elles via des propriétés. Ce modèle est enrichi par des règles SWRL (Semantic Web Rule Language) pour exprimer des inférences logiques non gérables directement par OWL.

Justification des Namespaces utilisés:
xmlns:h="http://example.org/hospital#" : Namespace principal, utilisé pour toutes les classes et propriétés spécifiques au domaine hospitalier.
rdf, rdfs, xsd, owl : Standards du Web sémantique permettant la définition de classes (rdfs:Class), de propriétés (rdf:Property), de types de données (xsd:string, etc.) et de l'ontologie (owl:Ontology).
swrl, swrlb : Pour écrire des règles logiques SWRL.
xsp : Namespace pour les fonctions supplémentaires, parfois utilisé pour les extensions (non obligatoire ici).

Description des Classes,  Propriétés:
Classe :	Description
Hospital:	Entité racine représentant l’hôpital
Wing :	Unité géographique ou bâtiment
Ward:	Service hospitalier
Room :	Chambre dans une unité
Bed :	Lit dans une chambre
Patient :	Personne hospitalisée
Doctor :	Personnel médical
Department :	Département médical (ex: cardiologie)
Record :	Dossier médical d’un patient
HistoryUpdate :	Mises à jour médicales associées à un dossier.

Propriétés d'objet:
Propriété 	    Domaine 	             Portée 	                        Description
hasWing 	     Hospital	              Wing	                            L’hôpital contient des unités
hasWard 	      Wing	                  Ward	                            Une unité contient des services
hasRoom	          Ward     	              Room	                            Un service contient des chambres
hasBeds  	      Room 	                 BedList	                        Une chambre contient des lits
hasBed	          BedList	              Bed	                            Liste de lits dans une chambre
bedTo	          Patient	              BedTo	                             Le patient est assigné à un lit
assignedBed	      BedTo	                  Bed      	                        Le lit assigné à un patient
recordTo	       Patient             	RecordTo	                        Patient lié à un dossier
hasRecord	        RecordTo	             Record	                            Dossier associé
hasHistoryUpdate	Record	           HistoryUpdate	                    Mises à jour du dossier
affiliatedDoctor	HistoryUpdate    	Doctor	                            Médecin responsable de l’update
affiliatedPatient	Record          	Patient                             Patient concerné par le dossier
worksIn           	Doctor              Department	                        Département d’un médecin

Description des requêtes SPARQL et leur signification:
Requête SPARQL	                           Signification
SELECT ?dept ?name ?phone	             Liste tous les départements avec nom et téléphone
SELECT ?doctor ?name ?speciality	     Liste les médecins et leurs spécialités
SELECT ?patient ?name ?bed	             Affiche les patients et leur lit assigné
SELECT ?doctor ?name (dans un département donné)	Trouve les docteurs dans un service spécifique
SELECT ?bed pour une chambre                    	Liste les lits d'une chambre donnée
SELECT ?updateDate ?updateTime ?doctor ?comments	Af fiche l’historique médical d’un patient
 Description des règles SWRL:
* Si un patient est assigné à un lit, alors ce lit est considéré comme occupé.
Patient(?p) ^ bedTo(?p, ?bto) ^ assignedBed(?bto, ?bed) → OccupiedBed(?bed).
 *Si un médecin est dans un département de cardiologie, alors c’est un cardiologue
Doctor(?d) ^ worksIn(?d, ?dep) ^ departmentName(?dep, "Cardiologie") → Cardiologist(?d)
* Une chambre est occupée si au moins un de ses lits est occupé
Room(?r) ^ hasBeds(?r, ?bl) ^ hasBed(?bl, ?bed) ^ OccupiedBed(?bed) → OccupiedRoom(?r)
conclusion
Ce projet a permis de mettre en place une ontologie représentant les principales entités d’un hôpital, comme les patients, les médecins, les services et les dossiers médicaux. Grâce à l'utilisation de RDF, RDFS et des règles SWRL, nous avons pu structurer les données de façon cohérente et logique.

Les requêtes SPARQL ont montré comment interroger efficacement ces données pour obtenir des informations précises. Cette ontologie facilite donc l’interopérabilité des données médicales, leur réutilisation, et permet une meilleure exploitation des connaissances dans un environnement hospitalier.

En résumé, ce travail illustre les apports du Web sémantique dans le domaine de la santé, en améliorant la compréhension, l’organisation et la recherche d’informations complexes.
