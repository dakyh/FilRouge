# Utilise Python 3.11 avec une image "slim" comme base
# L'image "slim" est une version réduite qui contient moins de packages que l'image complète,
# ce qui permet d'avoir une image Docker plus légère
FROM python:3.11-slim  

# Définit le répertoire de travail dans le conteneur
# Toutes les commandes suivantes seront exécutées à partir de ce répertoire
WORKDIR /app  

# Copie uniquement le fichier requirements.txt depuis le sous-dossier "odc" vers le répertoire de travail
# Cette séparation permet d'optimiser le cache Docker lors des reconstructions
COPY odc/requirements.txt .

# Installe les dépendances Python listées dans requirements.txt
# L'option --no-cache-dir évite de stocker les fichiers temporaires du cache pip,
# ce qui réduit la taille de l'image finale
RUN pip install --no-cache-dir -r requirements.txt  

# Copie tout le contenu du contexte de construction dans le répertoire de travail du conteneur
# Cela inclut tous les fichiers de l'application Django
COPY . .  

# Définit la commande qui sera exécutée au démarrage du conteneur
# Lance le serveur de développement Django sur l'adresse 0.0.0.0 (accessible depuis l'extérieur)
# et sur le port 8000
CMD ["python", "odc/manage.py", "runserver", "0.0.0.0:8000"]