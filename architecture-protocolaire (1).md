# Fiche de protocole – IRC CanaDuck

Complétez cette fiche pour décrire comment les clients interagissent avec le serveur IRC actuel.

## Format général

- Chaque commande est envoyée par le client sous forme d’une **ligne texte** terminée par `\n`.

## Commandes supportées

| **Commande**  | **Syntaxe exacte**       | **Effet attendu**         | **Réponse du serveur**        | **Responsable côté serveur** |
|---------------|---------------------------|----------------------------|--------------------------------|-------------------------------|
| `/nick`       | `/nick <pseudo>`          | Attribue un pseudo unique  | Message de bienvenue ou erreur | `set_pseudo()`                |
| `/join`       | `/join <canal>`           | Rejoint ou crée un canal   | Canal <canal> rejoint. ou      | `rejoindre_canal()`           |
|                                                                          erreur de pseudo manquant                                      |
| `/msg`        | `/msg <texte>`            |Envoie un message au canal  |Diffusion aux membres du canal  | `envoyer_message()`           |
| `/read`       | `/read`                   |   Message d'info           |discussions en direct           | `lire_messages()`             |
| `/log`        | `/log`         |Affiche les 10 dernières lignes de log |Affiche les lignes ou erreur    | `lire_logs()`                 |
| `/alert`      | `/alert <texte>`          |Envoie une alerte globale   |Diffusion ou erreur             | `envoyer_alerte()`            |
| `/quit`       | `	/quit`                  | Déconnexion propre         |Au revoir !                     | `handle()`                    |

## Exemples d’interactions

### Exemple 1 : choix du pseudo

```
Client > /nick ginette
Serveur > Bienvenue, ginette !
```

### Exemple 2 :
```

Client >
Serveur >

```
... 

# Structure interne – Qui fait quoi ?

| Élément                      | Rôle dans l’architecture                           |
|------------------------------|----------------------------------------------------|
| `IRCHandler.handle()`        | Lit et traite les lignes de commande               |
| `etat_serveur`               |                                                    |
| `log()`                      |                                                    |
| `broadcast_system_message()` |                                                    |
|                              |                                                    |
|                              |                                                    |

# Points de défaillance potentiels

> Complétez cette section à partir de votre lecture du code.

| **Zone fragile**                | **Cause possible**            | **Conséquence attendue**        | **Présence de gestion d’erreur ?** |
|----------------------------------|-------------------------------|----------------------------------|-------------------------------------|
| `wfile.write(...)`               |                               |                                  |                                     |
| Modification d’`etat_serveur`   |                               |                                  |                                     |
| Lecture du fichier log          |                               |                                  |                                     |
| Pseudo déjà pris (`/nick`)      |                               |                                  |                                     |
| Utilisateur sans canal courant  |                               |                                  |                                     |
|                                  |                               |                                  |                                     |

# Remarques ou cas particuliers

- Les commandes sont traitées **en texte brut**, sans structure formelle.
- Une mauvaise commande renvoie un message générique (`Commande inconnue.`).
