# Guide MCP MatchMyStage & Manuel pour Agents IA

Ce guide presente l'architecture des outils MCP du serveur MatchMyStage, les regles d'or de creation et d'optimisation de CV (Methode CAR, parsing ATS, cadrage 1 page), et le workflow recommande pour generer ou adapter des candidatures ultra-cibles.

---

## 1. Regles Strategiques & Guide Methodologique de Creation de CV

Tout agent IA generant ou adaptant un CV sur MatchMyStage doit respecter les regles methodologiques suivantes :

### 1.1 Cadrage 1 Page Strict
- Pour les etudiants, stagiaires et jeunes diplomes, le CV doit imperativement tenir sur **une seule page A4**.
- L'agent doit calibrer les espacements verticaux avec des balises `#v(4pt)` a `#v(6pt)` entre les sections pour eviter tout debordement sur une seconde page.

### 1.2 Alignement Titre & Matching ATS (Applicant Tracking Systems)
- **Titre Exact** : L'accroche du CV (sous le nom) doit reproduire fidelement l'intitule du poste ou du stage vise (ex : `Stage - Developpeur Back-End Node.js & Cloud (6 mois)`).
- **Mots-cles Cibles** : Les 5 a 7 mots-cles techniques principaux de l'offre doivent apparaitre au moins 2 fois chacun dans le document (dans l'accroche, les puces d'experience et la grille de competences).

### 1.3 Methode CAR (Contexte - Action - Resultat)
Bannir les descriptions passives et transformer chaque experience en preuve d'impact :
- **Formule d'Impact** : `[Verbe d'action vigoureux] + [Projet / Enjeu metier] + [Outils / Methode] + [Indicateur de resultat mesurable]`
- **Verbes d'action recommandes** :
  - *Tech & Conception* : Concevoir, Deployer, Automatiser, Optimiser, Refactoriser, Integrer, Configurer, Tester.
  - *Analyse & Data* : Quantifier, Evaluer, Diagnostiquer, Modeliser, Prevoir, Structurer, Piloter.
  - *Organisation* : Coordonner, Negocier, Federer, Developper, Faciliter, Livrer, Resoudre.
- **Exemple Avant** : *'Aide au developpement de fonctionnalites backend sur l'application.'*
- **Exemple Apres (CAR)** : *'Conception et deploiement de 14 endpoints REST securises (FastAPI / PostgreSQL), reduisant la latence de 35% et accueillant +500 utilisateurs actifs.'*

---

## 2. Convention de Syntaxe Markdown & Typst

Le moteur de rendu integre convertit directement le Markdown enrichi en Typst et en apercu visuel interactif.

### 2.1 Structure Canonique d'un CV

```markdown
# Louis Le Forestier
**Stage - Developpeur Fullstack FastAPI & Next.js**
Email: louis@example.com | Tel: 07 00 00 00 00 | Ville: Paris, France
[LinkedIn](https://linkedin.com/in/louis) | [GitHub](https://github.com/louis)

## Profil
Etudiant en derniere annee d'ecole d'ingenieur, passionne par les architectures logicielles modernes et les pipelines RAG. Rigoureux et oriente impact, je souhaite apporter mon expertise en FastAPI et React a l'equipe TechCorp.

#v(5pt)

## Experiences Professionnelles
### Developpeur Fullstack & IA - TechCorp | 2024-03 - 2024-09
- Conception et deploiement d'une API REST haute performance (FastAPI) pour le traitement de 10k requetes/jour.
- Mise en place d'un pipeline RAG avec vector database et embeddings avances.
**Tech :** Python, FastAPI, React, PostgreSQL, Docker

#v(5pt)

## Formation
### Master Ingenierie des Systemes Intelligents - Ecole d'Ingenieur | 2021 - 2024
Specialisation en apprentissage profond, architectures distribuees et data engineering.

#v(5pt)

## Competences
- **Langages :** Python, TypeScript, SQL, C++
- **Frameworks :** FastAPI, Next.js, React, PyTorch
- **Outils :** Docker, Git, Linux, PostgreSQL
- **Atouts :** Rigueur, Autonomie, Communication, Esprit d'equipe

#v(5pt)

## Projets
### MatchMyStage | [GitHub](https://github.com/matchmystage)
- Plateforme de gestion de candidatures et compilation automatique de documents PDF en 1 page.
**Tech :** Next.js, FastAPI, Typst
```

---

## 3. Architecture Regroupee des Outils MCP

Les outils du serveur MatchMyStage sont organises en 4 hubs unifies, garantissant clarte, puissance et compatibilite ascendante :

### 3.1 Profil & Identite Candidat
1. **`manage_candidate_profile(action, ...)`** : Hub unifie pour :
   - `action="get"` : Consulter la fiche RAG complete.
   - `action="update_summary"` : Mettre a jour le titre, la bio et les coordonnees.
   - `action="add_experience"` / `"add_education"` / `"add_skills"` / `"add_project"` : Enrichir les sections du parcours.
   - `action="set_avatar"` : Definir la photo de profil via fichier local ou URL.
2. **`get_candidate_profile(user_id, user_name)`** : Consultation directe de la fiche RAG.
3. **`get_candidate_cv_markdown(user_id, user_name)`** : Export structure du CV au format Markdown.
4. **`set_candidate_avatar(image_path_or_url, user_id, user_name)`** : Mise a jour de la photo de profil.
5. **`list_users()` & `create_user_profile(...)`** : Administration des profils.

### 3.2 Base de Connaissances RAG & Ingestion
6. **`get_cv_optimization_guidelines()`** : Guide officiel complet (CAR, ATS, mots-cles, cadrage 1 page).
7. **`manage_knowledge_document(action, ...)`** : Hub unifie pour :
   - `action="add"` : Ajouter un document Markdown (notes, synthese).
   - `action="import_file"` : Importer et extraire le texte d'un fichier local (PDF, DOCX, Markdown, texte).
   - `action="list"` / `action="delete"` : Lister et supprimer des documents de connaissances.
8. **`manage_knowledge_links(action, ...)`** : Hub unifie pour ajouter, lister et supprimer des references web.

### 3.3 Gestion des Offres & Suivi
9. **`manage_job_offer(action, ...)`** : Hub unifie pour :
   - `action="import"` : Enregistrer une nouvelle offre de stage trouvee sur le web.
   - `action="get"` : Recuperer l'integralite de la fiche de poste et des exigences.
   - `action="list"` : Lister les candidatures selon leur statut (`to_apply`, `applied`, `interview`, etc.).
   - `action="update_status"` : Mettre a jour l'avancement d'une candidature.

### 3.4 Studio Documentaire & Compilation Typst
10. **`generate_tailored_cv(job_id, ...)`** : Genere un CV ultra-cible appliquant la methode CAR, l'alignement du titre, l'accentuation des competences requises et les espacements `#v(5pt)`.
11. **`generate_tailored_cover_letter(job_id, ...)`** : Genere une lettre de motivation personnalisee d'1 page.
12. **`save_and_compile_document(job_id, doc_type, content, content_format, template_id)`** : Sauvegarde, convertit en Typst si besoin et compile instantanement en PDF vectoriel avec generation d'apercu PNG pour la plateforme.
13. **`adjust_document_spacing(job_id, doc_type, target_section, spacer_size_pt)`** : Insere un espacement vertical `#v(Xpt)` pour un cadrage 1 page parfait.
14. **`get_application_document(job_id, doc_type)`** : Recupere le contenu source, statut et lien PDF d'une offre.

---

## 4. Workflow Idéal pour un Agent IA

Pour adapter une candidature a une offre de stage :
1. **Consulter les regles** : Appeler `get_cv_optimization_guidelines()`.
2. **Analyser l'offre** : Appeler `manage_job_offer(action="get", job_id=...)`.
3. **Consulter le profil** : Appeler `manage_candidate_profile(action="get")`.
4. **Generer le CV personnalise** : Appeler `generate_tailored_cv(job_id=...)` en personnalisant l'accroche et les puces avec la methode CAR.
5. **Sauvegarder et compiler** : Appeler `save_and_compile_document(job_id=..., doc_type="cv", content=...)`.
6. **Ajuster le cadrage** : Si besoin, appeler `adjust_document_spacing()` pour garantir un rendu parfait sur exactement 1 page.
