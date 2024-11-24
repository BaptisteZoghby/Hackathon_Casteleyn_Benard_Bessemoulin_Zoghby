# Hackathon_Casteleyn_Benard_Bessemoulin_Zoghby

ReadME : 

L’objectif de notre projet est de fournir une solution de chatbot, facilement intégrable pour un retailer et qui propose une expérience client fluide et une fonctionnalité exclusive de négociation. Cette fonction de négociation peut être adaptée par le retailer selon sa stratégie et permet donc de démarquer notre produit de ceux de nos concurrents.
Le retailer peut ainsi proposer à ses clients une expérience de shopping personnalisée et possède une attractivité renforcée grâce à la fonctionnalité de négociation qui est décrite plus loin dans ce document.


Lien vers le vidéo de démonstration : 


Fonctionnalités du projet :
Interface chabot avec une interface graphique en HTML (index.html pour l’aspect graphique). On retrouve notamment sur cette interface, une fenêtre de chat, une barre de texte dans laquelle l’utilisateur peut écrire des messages, un bouton envoi et un bouton reset qui permet de réinitialiser la mémoire court terme du chat. 
Une exécutable python inclus dans un jupyternotebook qui permet de lancer l’interface graphique en local. 
Le chatbot est capable de : 
-	Expliquer son rôle avec un message prédéfini affiché sur la page, ce qui permet à un nouvel utilisateur de prendre en main l’outil plus simplement 
-	Reconnaitre un message de type recherche d’objet et fournir des objets qui se rapprochent le plus possible de ce qui est recherché*.
-	Reconnaitre qu’un utilisateur souhaite entrer dans une phase de négociation. Puis le chatbot est capable de mener la négociation, en prenant en compte des informations sur l’utilisateur (historique de négociation), le produit (price discounted price, et la stratégie du retailer (choix entre 3 types de stratégies)**
Ensuite, un bouton permet de gérer l’ajout d’un objet donné au panier. L’ajout d’un objet au panier lance une fonction de suggestion qui va proposer des objets ayant un lien avec l’objet ajouté au panier.

(*) Le fonctionnement est décrit dans la partie recherche.
(**) Les stratégies de négociation sont détaillées dans la fonction « négociation »

Le chatbot fonctionne donc avec l’API de MistralAI « mistral large », nous n’avons pas fait de fine tuning. Nous avons concentré nos efforts sur la création de fonctions qui permettent la gestion de scénarios correspondant aux cas d’usage sur un site de vente. Notre chatbot est donc largement basé sur les « function calling instruct ». 
Ainsi notre chatbot est adaptés lorsqu’on est dans des scénarios proches de scénarios recherche / vente / négociation, qui correspondent aux cas d’usages que nous avons voulu traiter.

La fonction recherche :

Lorsqu’une entrée de l’utilisateur est semblable à une recherche de produit, la fonction recherche s’active. 
Pour le modèle, la description est la suivante : "look for the product searched ".
A partir du message de l’utilisateur, la fonction est capable de déterminer les 2 à 4 mots clés les plus pertinents pour décrire l’objet recherché et le prix maximum.
A partir de ces mots clés, on appelle une fonction python « recherche_par_score ».
La fonction recherche_par_score est déterministe, elle associe un score à chaque objet de la base de données selon les principes suivants.
-	Plus un mot clé apparait tôt (à gauche) dans le « product_name », plus on gagne de points 
-	Plus il apparait tard (à droite) dans « « category plus on gagne de points
-	S’il apparait dans le about_product on gagne des points.
Il renvoie ensuite les 5 objets avec le score le plus haut (donc les 5 les plus pertinents pour les mots clés) à condition que leur prix soit inférieur au prix maximal mentionné par l’utilisateur. 
Le modèle renvoie ensuite une réponse avec ces différents produits à l’utilisateur.
Limitation : 
Il est préférable car la base de données est en anglais donc les keywords de déterminés par le modèle seront plus adaptés à la base de données s’ils sont en anglais. 

La fonction suggestion :
Lors de l’ajout d’un article au panier, on récupère « category » de l’objet, on séléctionne les 2 éléments les plus à gauche (catégories au sens large (par ex pour un cable on a : Computers&Accessories|Accessories&Peripherals|Cables&Accessories|Cables|USBCables) on récupère les éléments "Computers&Accessories|Accessories&Peripherals". A partir de ces catégories, on filtre le dataset et on suggère les 4 premiers éléments du dataset filtré et dont le « ranking » est supérieur à 4.2.

La fonction négociation :
But : Interagir avec le customer pour lui permettre de négocier le prix d'achat.
Il y a plusieurs types de retailers personnalities en fonction de leur capacité à commencer la négociation à des prix élevés, de leur capacité à arrêter une négociation en cours si le Customer essaye de négocier à des prix trop bas et sa flexibilité (tendance à diminuer ses prix.
La flexibilité est impactée par la flexibilité du costumer.
Cette dernière l'est aussi si le Customer demande des prix trop bas.
Si en 10 échanges, les deux parties n'arrivent pas à un accord, la négociation est stoppée et le prix revient au prix de départ.







