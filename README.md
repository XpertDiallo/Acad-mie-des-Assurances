# Académie des Assurances

MVP complet d’une plateforme web de formation professionnelle en assurance. L’application propose quatre modules, une lecture vocale séquencée, un tuteur documentaire, des QCM corrigés, un examen final, le suivi de progression et des attestations vérifiables.

## Modules intégrés

1. **Les fondements de l’assurance** — gratuit pour tous les auditeurs
2. **Le contrat d’assurance** — accès payant
3. **L’assurance vie** — accès payant
4. **L’assurance automobile** — accès payant

Les documents fournis ont été transformés en **26 leçons** et **52 questions pédagogiques**. Les références réglementaires issues des supports doivent être validées par le Responsable Pédagogique avant une mise en exploitation commerciale.

## Trois profils

| Profil | Accès principal |
|---|---|
| Webmaster Administrateur | comptes, activation, paiements, droits, statistiques, journal d’activité |
| Responsable Pédagogique | modules, publication, contenu, banque de QCM, contrôle qualité |
| Auditeur étudiant | cours, voix, questions, QCM, examen, progression, attestations |

## Accès de démonstration

| Profil | Email | Mot de passe |
|---|---|---|
| Administrateur | `admin@academie-assurances.demo` | `Admin@2026!` |
| Responsable pédagogique | `pedagogie@academie-assurances.demo` | `Pedago@2026!` |
| Auditeur étudiant | `auditeur@academie-assurances.demo` | `Etudiant@2026!` |

Le compte auditeur dispose initialement du module gratuit. L’administrateur peut activer les autres modules pour tester le parcours de paiement hors plateforme.

## Démarrage rapide avec Docker

```bash
cp .env.example .env
# Remplacer obligatoirement JWT_SECRET dans .env
docker compose up --build
```

Ouvrir ensuite `http://localhost:8080`.

## Démarrage pour le développement

### Linux / macOS

```bash
./scripts/run_local.sh
```

### Windows PowerShell

```powershell
.\scripts\run_local.ps1
```

Application : `http://localhost:5173`  
Documentation API : `http://localhost:8000/api/docs`

## Services documentaires et vocaux

L’application fonctionne sans service payant :

- lecture du cours par la synthèse vocale du navigateur ;
- saisie vocale du navigateur lorsqu’elle est disponible ;
- index vectoriel local à n-grammes pour retrouver les passages pertinents ;
- réponses de secours strictement construites à partir des passages du cours.

Pour enrichir les explications et la transcription serveur, renseigner une clé personnelle dans `GROQ_API_KEY`. **Aucune clé partagée ou publique n’est intégrée au code source.** Une clé doit rester exclusivement côté serveur et ne doit jamais être commise dans Git.

Le paramétrage recommandé est fourni dans `.env.example` :

- `openai/gpt-oss-120b` pour les explications approfondies ;
- `llama-3.3-70b-versatile` comme modèle de repli ;
- `llama-3.1-8b-instant` pour les réponses rapides ;
- `whisper-large-v3-turbo` pour la transcription audio ;
- `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2` en option pour les représentations sémantiques locales.

## Tests validés

```bash
cd backend && .venv/bin/pytest -q
cd frontend && npm run test
cd frontend && npm run build
```

Le test d’intégration couvre la connexion des trois profils, le verrouillage des modules, l’activation après paiement, la progression, le QCM, l’examen, l’attestation PDF, la vérification publique et les fonctions pédagogiques.

## Structure

```text
backend/                 API FastAPI, base SQLite, authentification, RAG, examens, certificats
frontend/                React, TypeScript, interface responsive et lecteur vocal
docs/                    architecture, sécurité, API, installation et cahier des charges
scripts/                 scripts de lancement Linux/macOS et Windows
.github/workflows/       intégration continue
```

## Documentation

- [Architecture](docs/ARCHITECTURE.md)
- [Installation et déploiement](docs/INSTALLATION.md)
- [Référence API](docs/API.md)
- [Sécurité et protection des données](docs/SECURITY.md)
- [Stratégie de tests](docs/TESTS.md)
- [Gestion des contenus](docs/CONTENT.md)
- [Cahier des charges fonctionnel](docs/Cahier_des_charges_Academie_des_Assurances_MVP.docx)

## Règles importantes de production

- remplacer les mots de passe de démonstration ;
- utiliser PostgreSQL pour un déploiement multi-utilisateur important ;
- conserver toutes les clés dans les variables d’environnement du serveur ;
- activer HTTPS ;
- sauvegarder la base et tester la restauration ;
- faire valider les références CIMA et les seuils réglementaires avant publication définitive ;
- définir une politique de confidentialité, des durées de conservation et une procédure de gestion des droits des personnes.

Le dépôt est publié sans licence open source explicite. Les contenus pédagogiques restent sous le contrôle de leur titulaire.
