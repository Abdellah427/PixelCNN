# PixelCNN : Modèle de Génération d'Images à partir de Pixel par Pixel

## Introduction

PixelCNN est un modèle de réseau neuronal profond utilisé pour la génération d'images. Contrairement aux modèles comme les GANs qui génèrent des images en une seule étape, PixelCNN génère chaque pixel de manière séquentielle, en tenant compte des pixels précédemment générés pour chaque nouvelle prédiction. Seuls les pixels précédents sont utilisés, et jamais les pixels suivants, ce qui fait de cette méthode un modèle autoregressif et non un auto-encodeur.

Ce projet présente un modèle PixelCNN, avec un focus sur :

- L'entraînement du modèle à partir d'un dataset d'images.

- La génération d'images en utilisant l'échantillonnage pixel par pixel.

- La visualisation des images générées.

## Sommaire : 

1. Objectif du modèle

2. Architecture du modèle PixelCNN

3. Entraînement du modèle

4. Génération d'images

5. Visualisation des images générées

6. Conclusion


## Architecture du modèle PixelCNN
1. Masquage et Convolutions

La structure principale de PixelCNN repose sur une série de convolutions masquées. Ces convolutions masquent certaines parties de l'image lors de la prédiction d'un pixel, garantissant ainsi que le modèle ne "voit" que les pixels précédemment générés lors de l'échantillonnage. Il existe deux types de masques :

Masque A : Le modèle ne voit que les pixels à gauche et en haut du pixel actuel.
Masque B : Le modèle peut voir les pixels à gauche, en haut, et en diagonale, mais pas à droite du pixel actuel.
Les résidus sont utilisés dans des blocs résiduels pour améliorer la propagation de l'information à travers le réseau, permettant au modèle d'apprendre plus efficacement.

2. Blocs Résiduels

Les blocs résiduels aident le modèle à mieux apprendre en permettant à l'information de circuler plus facilement à travers les couches du réseau. Chaque bloc résiduel commence par une convolution masquée, suivie d'une activation non linéaire, puis d'une autre convolution. Ce qui est particulier, c'est que la sortie du bloc est ajoutée directement à l'entrée d'origine, grâce à ce qu'on une skip connection. Cela permet à l'information de ne pas se perdre dans les couches profondes du réseau, rendant l'apprentissage plus stable et plus rapide. Cela aide le modèle à générer des images plus précises en facilitant la propagation de l'information dans le réseau.

3. Construction du Modèle

Le modèle PixelCNN est construit autour de plusieurs couches de convolutions masquées, combinées à des blocs résiduels pour permettre une meilleure propagation de l'information à travers le réseau. Les convolutions masquées sont essentielles car elles garantissent que chaque pixel ne dépend que des pixels précédents (ceux qui sont au-dessus et à gauche de lui), respectant ainsi la dépendance autoregressive nécessaire pour générer une image pixel par pixel. Les blocs résiduels, quant à eux, ajoutent des skip connections qui permettent de maintenir une version directe de l'information tout au long des couches profondes du modèle. Cela améliore la stabilité de l'entraînement et accélère la convergence. À la fin de l'architecture, une série de couches de convolutions permet de prédire la distribution de probabilité pour chaque pixel, ce qui permet au modèle de déterminer l'intensité de chaque pixel dans l'image générée. Le modèle est conçu pour générer des images avec une grande fidélité en apprenant progressivement la relation complexe entre les pixels.

4. Entraînement du Modèle
L'entraînement du modèle PixelCNN se fait en minimisant la perte de cross-entropie entre les prédictions de distribution des pixels et les valeurs réelles des pixels dans les images. À chaque itération, l'entrée est un lot d'images, et le modèle prédit la distribution de probabilité pour chaque pixel. Le critère de loss utilisé est la cross-entropie, qui compare les prédictions du modèle avec les classes réelles (les intensités des pixels). L'optimiseur Adam est couramment utilisé pour mettre à jour les poids du modèle, en ajustant les paramètres pour minimiser cette perte. L'entraînement est effectué sur plusieurs epochs pour permettre au modèle de bien apprendre les relations entre les pixels, ce qui est crucial pour générer des images réalistes. Au fur et à mesure de l'entraînement, la performance du modèle s'améliore, et les prédictions deviennent plus précises.

5. Génération d'images
Une fois le modèle PixelCNN entraîné, il peut générer des images de manière autoregressive. Le processus commence par une image vide, puis chaque pixel est généré un à un, de haut en bas et de gauche à droite, en fonction des pixels précédents. Pour chaque pixel, le modèle calcule la probabilité de chaque intensité possible et sélectionne un pixel en fonction de cette distribution. Le paramètre de température contrôle la diversité des images générées, avec une température basse rendant l'image plus précise et une température haute augmentant la diversité.

6. Visualisation des Images Générées
Les images générées peuvent être visualisées pour évaluer leur qualité. En utilisant des bibliothèques comme Matplotlib, les images sont affichées sous forme de grilles, permettant de juger de la performance du modèle, de la diversité et de la qualité des images produites.

7. Conclusion
PixelCNN est une approche puissante pour générer des images pixel par pixel, en apprenant les relations conditionnelles entre les pixels voisins. Grâce aux convolutions masquées et aux blocs résiduels, il génère des images réalistes tout en maintenant une architecture stable. Il offre des applications intéressantes dans la génération d'images et la compréhension des distributions visuelles.
