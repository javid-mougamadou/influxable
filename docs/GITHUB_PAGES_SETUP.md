# Configuration GitHub Pages — influxable.javid-space.cloud

## Déploiement automatique

Le workflow `.github/workflows/deploy-docs.yml` construit le site Jekyll (thème Cayman) à partir du dossier `docs/` et le déploie sur GitHub Pages à chaque push sur `main`, `master` ou `develop`.

## Étapes côté GitHub

1. **Settings → Pages**
   - **Source** : choisir **GitHub Actions** (et non « Deploy from a branch »).

2. **Domaine personnalisé**
   - Dans **Pages → Custom domain**, saisir : `influxable.javid-space.cloud`
   - Cocher **Enforce HTTPS** une fois le DNS actif.

## Configuration DNS (javid-space.cloud)

Chez ton hébergeur DNS (Cloudflare, OVH, etc.), ajouter :

| Type  | Nom       | Valeur                    |
|-------|-----------|---------------------------|
| CNAME | influxable | `<utilisateur>.github.io` |

Remplace `<utilisateur>` par ton identifiant GitHub (ou par `nom-org.github.io` si le dépôt est sous une organisation).

La propagation peut prendre quelques minutes à quelques heures.

## URL du site

Après déploiement : **https://influxable.javid-space.cloud**
