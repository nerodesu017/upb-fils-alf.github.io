---
sidebar_position: 9
description: Utiliser le tableau de symboles
slug: /tp/08
---

# 08 - Génération de code

La génération de code est l’une des dernières étapes dans le processus de compilation. En fonction du langage cible, on peut avoir soit une génération intermédiaire de code (dans le cas de Three Address Code) ou une génération de code dans un langage d’assemblage.

Dans ce TP, on travaillera sur avec un intermédiaire pour une machine à pile infinie. Donc, le principe de fonctionnement sera le suivant: on a une pile sur laquelle on met les opérandes (soient-ils des valeurs ou des variables) et, ensuite, on utilise les instructions sur le contenu de la pile.

:::tip
Rappelez-vous comment on a procédé pour le premier devoir. Le principe est le même.
:::

## Instructions

### Instructions avec la pile

| Instruction  | Opérandes | Description |
|:-----------:|-----------|-------------|
|`push`| val/var| Ajoute la valeur `val` ou la variable `var` sur la pile| 
|`pop`| var| Supprime et sauvegarde la valeur au sommet de la pile dans la variable `var`| 

### Instructions mathématiques

Pour tout opération mathématique, le résultat se trouvera toujours au sommet de la pile.

| Instruction  | Opérandes | Description |
|:-----------:|-----------|-------------|
|`add`|- | Prend les premières deux valeurs de la pile et fait leur somme| 
|`sub`|- | Prend les premières deux valeurs de la pile et fait leur différence| 
|`mul`|- | Prend les premières deux valeurs de la pile et fait leur produit| 
|`div`|- | Prend les premières deux valeurs de la pile et fait leur division| 

### Instructions logiques

Pour tout opération logique, le résultat se trouvera toujours au sommet de la pile: 1 pour `VRAI`, 0 pour `FAUX`. Toutes les instructions prennent les deux premières valeurs de la pile et font la comparaison associée.

| Instruction  | Opérandes | Description |
|:-----------:|-----------|-------------|
|`gt`, `ge`|- | Prend les premières deux valeurs de la pile et les compare en utilisant plus grand que/plus grand que ou égal | 
|`lt`, `le`|- | Prend les premières deux valeurs de la pile et les compare en utilisant moins que/Moins que ou égal | 
|`eq`, `ne`|- | Prend les premières deux valeurs de la pile et les compare en utilisant égal/différent| 

### Contrôle du flux

| Instruction  | Opérandes | Description |
|:-----------:|-----------|-------------|
| `if goto` | label| Prend le sommet de la pile. Si la valeur est vraie (pas `0`), on fait le saut vers l’étiquette `label`. Autrement, on continue l’exécution.| 
| `ifFalse goto` | label| Prend le sommet de la pile. Si la valeur est fausse (`0`), on fait le saut vers l’étiquette `label`. Autrement, on continue l’exécution.| 


### Appel de fonctions

| Instruction  | Opérandes | Description |
|:-----------:|-----------|-------------|
| `call` | `fonction`, `nr_params`  | Appele la fonction `fonction` avec `nr_params` paramètres| 

:::info
Pour les paramètres, il faut les ajouter à la pile dans l’ordre invèrse de leur positions dans l’entête de la fonction. Par exemple, pour l'appel suivant:

```
call sum(2, 4, x);
```

On obtiendra le code:

```
push x
push 4
push 2
call sum, 3
```

:::

## Exemples

Prenons l’exemple suivant d’expréssion et le code généré pour elle:

```
/* Expression */
x = (5-3)/7;

/* Code */
push 5
push 3
sub
push 7
div
pop x
```

## Exercices
1. Ouvrez le dossier `TP8`. Dans le fichier `Ex1.txt`, écrivez le code cible pour les expressions suivantes:
```
a = (5-3)*7+2+4;
e = (a+5)/(a-2);
```
2. Ouvrez le dossier `TP8`. Dans le fichier `Ex2.txt`, écrivez le code cible pour le code source suivant:
```c
if (a > 0)
{
  result = "positive";
}
else
{
  result = "negative";
}
```
3. Ouvrez le dossier `TP8/Code`. Suivez les `TODO3` dans le fichier `app/src/main/kotlin/CodeGenerator` pour implanter la génération de code pour les expressions. Les étapes sont les suivantes:
- commencez avec les valeurs (donc, avec les instances de la classe `Value`). Les instructions résultantes doivent tout simplement ajouter la valeur sur la pile
- continuez avec les expressions binaires, pour lesquelles il faut ajouter les deux opérandes et faire l’opération corréspondante à partir de l’opérateur

    Vérifiez votre code en utilisant des entrées situées dans le dossier `Ex8/Inputs/Ex3`.

4. Ouvrez le dossier `TP8/Code`. Suivez les `TODO4` dans le fichier `app/src/main/kotlin/CodeGenerator` pour implanter la génération de code pour les afféctations.
    Vérifiez votre code en utilisant des entrées situées dans le dossier `Ex8/Inputs/Ex4`.

5. Ouvrez le dossier `TP8/Code`. Suivez les `TODO5` dans le fichier `app/src/main/kotlin/CodeGenerator` pour implanter la génération de code pour les instructions `if`.

    :::tip
    Faites attention: sachant que ce genre d’instruction nécéssite des étiquettes et que les étiquettes doivent être unique dans le code, il faut aussi ajouter une variable qui les compte et qui est incrementée chaque fois qu’on ajoute une nouvelle étiquette.
    :::

    Vérifiez votre code en utilisant des entrées situées dans le dossier `Ex8/Inputs/Ex5`.

6. Ouvrez le dossier `TP8/Code`. Suivez les `TODO6` dans le fichier `app/src/main/kotlin/CodeGenerator` pour implanter la génération de code pour les appels des fonctions.

    Vérifiez votre code en utilisant des entrées situées dans le dossier `Ex8/Inputs/Ex6`.