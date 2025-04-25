# Utilise Node.js 18 avec Alpine Linux comme image de base
FROM node:18-alpine  

# Définit le répertoire de travail dans le conteneur
# Toutes les commandes suivantes seront exécutées à partir de ce répertoire
WORKDIR /app  

# Copie uniquement le fichier package.json dans le répertoire de travail
# Cette étape est séparée pour profiter du cache de Docker lors des reconstructions
COPY package.json ./ 

# Installe les dépendances définies dans package.json
# Cette commande ne s'exécutera à nouveau que si package.json a changé
RUN npm install  

# Copie tous les fichiers du contexte de construction (répertoire courant) 
# dans le répertoire de travail du conteneur
COPY . .  

# Définit la commande à exécuter lorsque le conteneur démarre
# Démarre le serveur de développement Vite
# Le paramètre --host 0.0.0.0 permet d'accéder au serveur depuis l'extérieur du conteneur
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]