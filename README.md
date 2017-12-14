# mavenrepo
Ce projet explique comment déployer une librairie maven open source sur un repo Github. Et comment la récupérer dans un autre projet.

## Mettre à disposition une librairie sur Github
1. Dans le projet que vous souhaitez exporter ouvrez le fichier `pom.xml` et complétez les informations suivantes `groupeId`, `artifactId` et `version`. Elles seront utilisées plus loin pour indiquer la dépendance que vous souhaitez récupérer dans votre nouveau projet maven.
```
   <groupId>groupIdTest</groupId>
    <artifactId>artifactIdTest</artifactId>
    <version>0.0.1</version>
```
2. Générer le jar de votre projet. Vous pouvez utiliser la commande `mvn clean install`.
3. Créer un repo github `mavenrepo` ou tout autre nom que vous souhaitez lui donner.
4. Clonez votre repo.
5. Ajoutez 2 dossiers à la racine du repo github nommez `releases`et `snapshots`, le premier utilisé pour les versions final de votre librairies, le second pour la version en cours de développement (attention cela ne veut pas dire qu'elle ne doit pas être fonctionnelle).
6. A hauteur de votre `.jar` exécutez la commande suivante : 
```
mvn deploy:deploy-file -DgroupId=groupIdTest -DartifactId=artifactIdTest -Dversion=0.0.1 -Dpackaging=jar -Dfile="artifactIdTest-0.0.1.jar"  -Durl=file:<YOUR_PATH>/mavenrepo/releases
```
7. Faite de même avec `snapshots` ou copiez-collez simplement le contenu de `releases` dans `snapshots`. Faites un add et commit de vos fichiers, et faites un push.
```
git add .
git commit -m "First deployment"
git push origin master
```
Voilà votre librairie est désormais accessible sur le repos github. 

## Récupérer la librairie déposée précédemment sur github
Allez dans votre nouveau projet maven et ouvrez le `pom.xml` et ajoutez ces quelques lignes de codes. 
```
    <dependencies>
        <dependency>
            <groupId>groupIdTest</groupId>
            <artifactId>artifactIdTest</artifactId>
            <version>0.0.1</version>
        </dependency>
    </dependencies>

    <repositories>
        <repository>
            <id>groupIdTest</id>
            <url>https://raw.github.com/<YOUR_USER_NAME>/groupIdTest/releases/</url>
        </repository>
    </repositories>
```
Faites maintenant un `mvn clean install` vous pouvez constater que vous venez de télécharger votre librairie.
