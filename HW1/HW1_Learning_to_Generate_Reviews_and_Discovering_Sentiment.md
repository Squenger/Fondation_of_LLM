# [MVA] Homework #1 : Essay on "Learning to Generate Reviews and Discovering Sentiment"

**Aimine Meddeb**

## 1. Why do you find it interesting?
*   **Le changement de paradigme :** Expliquer comment cet article démontre que l'apprentissage purement non supervisé (la simple tâche de prédire le caractère suivant) force un modèle à internaliser des représentations sémantiques de haut niveau.
*   **La justification architecturale :** Mettre en avant le choix du Multiplicative LSTM (mLSTM). Tu peux préciser mathématiquement pourquoi cette architecture, qui permet aux matrices de transition de dépendre de l'entrée courante, offre une expressivité supérieure pour modéliser le texte au niveau du caractère par rapport à un RNN classique.
*   **L'importance historique :** Souligner que ce papier de 2017 est un précurseur fondamental de l'apprentissage de transfert en NLP, prouvant l'efficacité de la modélisation générative avant même l'hégémonie des Transformers.


This article is very intresting due to its form, thy way thez researches wrote the paper, and the pioneering nature of the paper.

* First, they don't raise supervised learning as the royal way to train a model (which is the feeling I have when I read other papers), and on the contrary, they try to show that unsupervised learning can develop complex representation without any direction given by a human. We always hear about big LLMs trained by companies, like OpenAI or Anthropic, which are trained on the biggest dataset possible with the most powerful GPUs. However, I am more sensitive to researchers or companies that work on new and smarter ways to train a model because I think it is the only way this industry can be coherent with today's ecological challenges. 

* Also, the word that grabbed my attention in the title was "sentiment" because it is really interesting to see how a model (especially an LLM) simulates things that we cannot clearly describe ourselves. Indeed, I really appreciate when researchers dive into a complex model and desassemble it to understand how it works. It is like making retro-engineering on the model, which is in my opinion too often considered as an 
incomprehensible black box.

* I feel like the core of the article (the discovery of a unique weight that represents a positive or negative aspect of a sentence) was not the original objective of the research. It is like they find out this phenomenon in the middle of their work and realize it was more interesting than what they tried to do at first. In this article I really see this feeling in the flow of the writing and it makes the article more pleasant to read.

* Today, training a large model that wil be afterwords fintued to a specific task is a well-known method. However, if we look at the date of publication of this paper (april 2017), we realise that it is the fundamental paper of transfer learning in NLP, demonstrating the effectiveness of generative modeling well before the apparition of Transformers (june 2017).

## 2. What surprises you?
*   **La parcimonie extrême (Sparsity) :** Détailler la surprise liée à la régularisation mathématique. L'application d'une pénalité $\mathcal{L}_1$ sur les poids du classifieur linéaire révèle que l'information de polarité n'est pas distribuée dans tout le vecteur latent, mais concentrée sur une seule coordonnée parmi 4096.
*   **L'isolation sémantique :** Le fait qu'un concept humain aussi abstrait que le "sentiment" soit encodé de manière parfaitement isolée et linéaire dans l'espace d'état ($c_t$).
*   **La causalité prouvée par l'intervention :** La surprise ne s'arrête pas à l'observation passive. Le fait de pouvoir écraser (clamping) artificiellement la valeur de ce scalaire spécifique pendant la passe avant d'inférence, et de voir la distribution de probabilité générative s'adapter instantanément pour produire un texte positif ou négatif, prouve que ce neurone régit causalement la génération.

## 3. Any idea of a pedagogical exercise or experiment to do around it?
*   **L'objectif :** Créer un TP sous PyTorch visant à reproduire géométriquement la découverte et le contrôle de ce neurone sur une architecture simplifiée.
*   **Déroulé de l'expérience proposée :**
    1.  **Entraînement :** Entraîner un petit modèle (char-RNN ou petit Transformer décodeur) de zéro sur un corpus binaire très contrasté (ex: critiques d'allociné positives vs négatives).
    2.  **Régularisation Lasso :** Extraire les vecteurs latents de la dernière couche et entraîner un classifieur de polarité avec une forte fonction de coût $\mathcal{L}_1$ pour forcer la majorité des poids à zéro et isoler le "neurone du sentiment".
    3.  **Steering :** Modifier manuellement le script d'inférence en intervenant dans la boucle temporelle : `hidden_state[:, sentiment_index] = delta`. 
    4.  **Visualisation :** Générer des textes en faisant varier la constante arbitraire $\delta$ de $-5$ à $+5$, et tracer des graphiques montrant l'évolution dynamique des logits des mots positifs/négatifs en sortie.