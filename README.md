# Notes: Data Analysis & Dimensionality Reduction

Here are notes on common techniques used for data analysis, feature reduction, and visualization.

---

## 1. PCA (Principal Component Analysis)

* **What is it?** An unsupervised, linear dimensionality reduction technique.
* **How does it work?** It finds a new set of axes, called **Principal Components**, to represent the data. The first principal component (PC1) is the direction that captures the **maximum possible variance (spread)** in the data. PC2 is the next direction that captures the most *remaining* variance, and so on. It discards the later components that capture little variance (noise).
* **What is it used for?** Feature engineering, data compression, and noise reduction. It's the most common "first step" to simplify high-dimensional data.
* **Analogy: The Shadow 👥**
    Imagine a 3D object (your data). You want to represent it in 2D (on a wall). PCA is like a flashlight. It finds the *perfect angle* to shine the light so the 2D shadow on the wall shows the most shape and detail (variance) of the 3D object.

---

## 2. SVD (Singular Value Decomposition)

* **What is it?** A fundamental mathematical technique that breaks *any* matrix down into three simpler matrices ($A = U \Sigma V^T$).
* **How does it work?** It doesn't just find new axes; it breaks the matrix into:
    1.  **$U$:** Row patterns (left-singular vectors).
    2.  **$\Sigma$ (Sigma):** A diagonal matrix of "singular values." These act as "importance" knobs, ranking the patterns from most to least significant.
    3.  **$V^T$ (V-transpose):** Column patterns (right-singular vectors).
* **What is it used for?** SVD is the **mathematical engine that makes PCA work**. PCA is just one *application* of SVD. It's also used for image compression, recommendation systems (matrix factorization), and removing noise.
* **Analogy: The Recipe 🍲**
    If your data matrix $A$ is a complex curry, SVD is the master chef who "decomposes" it. It gives you a list of all the core "flavor profiles" (like *spicy*, *creamy*, *earthy*) and a recipe card ($\Sigma$) that says, "to make this exact curry, use 2 parts *spicy*, 1.5 parts *creamy*, and 0.1 parts *earthy*." To "compress" the dish, you just keep the most important flavors.

---

## 3. LDA (Linear Discriminant Analysis)

* **What is it?** A **supervised** linear dimensionality reduction technique.
* **How does it work?** Because it's **supervised**, it uses the *class labels* (e.g., "cat," "dog," "bird"). Its goal is **not** to find the most variance (like PCA), but to find the new axes that **maximize the separation between the known classes**. It tries to push the centers of the classes far apart while making the spread *within* each class as small as possible.
* **What is it used for?** Feature engineering *specifically* to help with classification problems.
* **Analogy: The Party Photographer 📸**
    * **PCA** is a photographer trying to get the *widest possible view* of a crowded party room (maximum variance).
    * **LDA** is a photographer who already *knows* who is on the "Bride's side" and who is on the "Groom's side." They find the *one specific angle* to take the photo so the two groups are standing as far apart as possible, making them easy to tell apart.

---

## 4. GDA (Gaussian Discriminant Analysis)

* **What is it?** This is a **generative classifier**, not a dimensionality reduction technique like the others.
* **How does it work?** It's related to LDA, but it makes a stronger assumption: that the data for *each* class follows a Gaussian (bell curve) distribution. It learns the mean and covariance (the "shape" of the data cloud) for each class. To classify a new point, it uses Bayes' theorem to calculate the probability that the point belongs to each class's "cloud."
* **What is it used for?** Classification. (Variants include QDA - Quadratic Discriminant Analysis).
* **Analogy: The Birdwatcher 🐦**
    You have data for "Robins" and "Bluejays" (classes). GDA assumes "Robins" have a specific average size and color-variation (a Gaussian "cloud") and "Bluejays" have a different one. When you see a new bird, you check which "cloud" it most likely belongs to.

---

## 5. Kernel PCA (KPCA)

* **What is it?** A non-linear version of PCA.
* **How does it work?** It tackles data that can't be separated by a straight line (e.g., a spiral or a "Swiss roll" shape). It uses a "kernel trick" to *implicitly* map the data into a much higher-dimensional space. In this new space, the data *becomes* linearly separable. It then performs regular PCA in this new, "unseen" higher dimension.
* **What is it used for?** Finding complex, non-linear patterns that regular PCA would miss.
* **Analogy: Untangling a Necklace ✨**
    Your data is a tangled 2D necklace. You can't just cast a shadow (PCA) to see its true shape. The "kernel trick" is like *tossing the necklace up into the air* (mapping to a 3D space). For a brief moment, it untangles, and *then* you shine the light (PCA) to get a clear 2D shadow of its true, unrolled shape.

---

## 6. Autoencoders (AE)

* **What is it?** A type of artificial neural network used for unsupervised, non-linear dimensionality reduction.
* **How does it work?** It has two parts:
    1.  **Encoder:** A neural network that "squishes" the high-D input data (e.g., a 784-pixel image) down into a very small, compressed representation (the "bottleneck" or "latent space").
    2.  **Decoder:** A second network that tries to *reconstruct* the original image *only* from that tiny compressed representation.
    The whole system is trained to make the (Reconstructed Output) as close to the (Original Input) as possible. The "bottleneck" (the compressed data) becomes your new, powerful, non-linear set of features.
* **What is it used for?** Non-linear dimensionality reduction, image denoising, and as a base for generative models (VAEs).
* **Analogy: The Expert Note-Taker ✍️**
    * **Encoder:** An expert reads a 10-page chapter (input) and writes a 3-sentence summary (bottleneck).
    * **Decoder:** A novice reads *only* the 3-sentence summary and tries to re-write the original 10-page chapter.
    You train the expert to write a summary *so good* that the novice's reconstruction is perfect. That 3-sentence summary is the ultimate compressed version of the chapter.

---

## 7. Isomap

* **What is it?** A non-linear dimensionality reduction technique (part of "Manifold Learning").
* **How does it work?** It assumes your data lies on a complex, curved surface (a "manifold") in a high-D space.
    1.  First, it connects each data point only to its *nearest neighbors*, creating a graph.
    2.  Then, it calculates the "true" distance between points by "walking along the curved surface" of the graph (this is the "geodesic distance"), not by cutting straight through empty space.
    3.  Finally, it creates a 2D/3D map that best preserves these "walking" distances.
* **What is it used for?** Unrolling complex, curved data structures, like the "Swiss Roll" dataset.
* **Analogy: The Mountain Hiker ⛰️**
    You have two villages on opposite sides of a mountain. A simple distance (like PCA uses) is the *straight line through the rock* (useless). Isomap finds the *actual walking path over the mountain's surface* (geodesic distance) and draws a flat map that preserves this *walking* distance, effectively "unrolling" the mountain.

---

## 8. t-SNE (t-distributed Stochastic Neighbor Embedding)

* **What is it?** A non-linear technique used almost exclusively **for visualization**.
* **How does it work?** Its only goal is to preserve "neighborhoods." It takes high-D data and creates a 2D/3D scatter plot. On this plot, points that were *close neighbors* in the high-D space are *also* close neighbors. It's fantastic at revealing clusters.
* **What is it used for?** **Visualization only.** It's the best tool for *seeing* if your data has natural groupings. **Do not** use its output as features for another model.
* **Analogy: The Socialite 💃**
    You're throwing a big party and want to arrange the seating (the 2D plot). You don't care about "big picture" relationships (like PCA). You just want to make sure that *people who are friends* (neighbors) get to sit *close to each other*. t-SNE is a master at this, creating clear tables (clusters) of social groups.
* **Key Warning:** The *distances between clusters* on a t-SNE plot are meaningless. A group on the far left isn't necessarily "more different" from a group on the far right.

---

## Comparison Table

| Technique | Type | Goal | Supervised? | Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **PCA** | Linear | Maximize variance | **Unsupervised** | Feature Engineering, Compression |
| **SVD** | Linear | Matrix decomposition | **Unsupervised** | The math *behind* PCA, RecSys |
| **LDA** | Linear | Maximize class separation | **Supervised** | Feature Engineering for Classification |
| **GDA** | (N/A) | Model class probability | **Supervised** | **Classifier** (not DR) |
| **Kernel PCA**| Non-Linear | Maximize variance (in high-D) | **Unsupervised** | Finding non-linear patterns |
| **Autoencoder** | Non-Linear | Reconstruction (from bottleneck) | **Unsupervised** | Feature Engineering, Denoising |
| **Isomap** | Non-Linear | Preserve "Geodesic" (path) distance | **Unsupervised** | Unrolling complex "manifolds" |
| **t-SNE** | Non-Linear | Preserve local "neighbors" | **Unsupervised** | **Visualization only** (seeing clusters) |
