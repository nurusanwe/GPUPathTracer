# GPU Path Tracer

Ce dépôt contient le code d'un **path tracer** GPU écrit en C++/CUDA. Il s'agit du projet final de Peter et Karl pour le cours *CIS 565: GPU Programming* (printemps 2012, University of Pennsylvania).

## Structure des dossiers

- `GPUPathTracer/` : contient la solution Visual Studio, le code source et les bibliothèques tierces.
- `GPUPathTracer/GPUPathTracer/` : fichiers sources C++/CUDA.
- `GPUPathTracer/bin/` : exécutables Windows déjà compilés.
- `GPUPathTracer/glew/` et `GPUPathTracer/freeglut/` : dépendances OpenGL.
- Plusieurs fichiers `.obj` : modèles 3D utilisés pour les scènes d'exemple.

## Aperçu de l'architecture

- **display.cpp** : point d'entrée de l'application. Initialise OpenGL/GLUT, la caméra interactive et lance le rendu GPU.
- **interactive_camera.*** : gère une caméra orbitale avec profondeur de champ. Utilisé pour générer la caméra finale (`Camera`).
- **path_tracer.* / path_tracer_kernel.cu** : coeur du moteur de rendu. Le fichier C++ prépare la scène (sphères, triangles) et appelle le noyau CUDA qui calcule l'illumination par lancer de rayons.
- **material.h**, **medium.h**, **absorption_and_scattering_properties.h** : structures décrivant les matériaux et les milieux traversés par les rayons (couleurs diffusées, réfraction, absorption, etc.).
- **objcore/** : petit chargeur de fichiers OBJ pour importer des maillages.

Le path tracing consiste à tirer aléatoirement des rayons depuis la caméra et à cumuler, pour chaque pixel, la lumière reçue après de multiples rebonds sur les surfaces. Les algorithmes sont écrits dans `path_tracer_kernel.cu` et utilisent la bibliothèque CUDA et `cutil_math`.

## Où modifier le code ?

- Pour changer ou ajouter des objets à la scène : voir `PathTracer::setUpScene` et `PathTracer::createDeviceData` dans `path_tracer.cpp`. C'est ici que les sphères et triangles sont initialisés et copiés sur le GPU.
- Pour étendre l'algorithme (nouveaux matériaux, autre stratégie de lancer de rayons, etc.) : modifier `path_tracer_kernel.cu` et les structures associées.
- Les interactions utilisateur (rotation/zoom de la caméra) se trouvent dans `interactive_camera.cpp` et `display.cpp`.
- Les modèles 3D supplémentaires peuvent être placés dans des fichiers `.obj` puis chargés via `objcore/objloader.h`.

## Compilation

Le projet a été conçu pour Windows avec Visual Studio et le Toolkit CUDA 4.x. Ouvrez `GPUPathTracer.sln` et lancez la compilation. Les exécutables se trouvent ensuite dans `GPUPathTracer/bin/`. Sous d'autres systèmes, il faudra créer manuellement un projet CUDA équivalent (aucun Makefile n'est fourni).

## Prérequis
 
Pour compiler et tester cet exemple, il vous faut :
- un GPU NVIDIA compatible CUDA ;
- le Toolkit CUDA (fournit `nvcc`) ;
- un compilateur C++ (Visual Studio sous Windows, ou `g++`/`clang++` sous Linux) ;
- les bibliothèques OpenGL (`glew`, `freeglut`), déjà incluses dans le projet pour Windows.
 
Sous Ubuntu, `nvcc` est fourni par le paquet `nvidia-cuda-toolkit` :
```bash
apt-get install nvidia-cuda-toolkit
```

Sous **macOS**, installez les dépendances via Homebrew puis récupérez le toolkit CUDA depuis le site de NVIDIA :
```bash
brew install glew freeglut
# Télécharger et installer le .dmg CUDA correspondant à votre version de macOS
```
Assurez‑vous que `nvcc` est présent dans votre `PATH` (en général dans `/usr/local/cuda/bin`). Vous pourrez alors compiler un projet Xcode ou générer un Makefile adapté.
