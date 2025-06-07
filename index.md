---
title: Introduction
layout: home
---
# Gas or Pass?
![Logo](assets/images/gas%20or%20pass.svg)

## By: Kenji Gunawan and Sumadhu Rubaiyat

## An Algorithm for Deliciousness

Food is expensive, but hunger doesn’t care. So we cook. Whether you're a broke college student, a resourceful parent, or just someone trying to survive another week without eating out, recipes are the lifeline to affordable, satisfying meals. But here's the catch: not all recipes are created equal. You're scrolling through Food.com, staring at some stew that *might* be the next best thing, or might taste like boiled regret. How do you know if it’s worth your time, money, and appetite?

Enter our project: **Gas or Pass? - An Algorithm for Deliciousness**. With thousands of recipes at our fingertips and a goldmine of user reviews, we set out to build a model that helps you decide: **Gas or Pass?** In other words, given a recipe’s ingredients, instructions, and nutritional content, can we predict how well it will be received by the Food.com community?

To answer this, we analyzed the Food.com Recipes and Interactions dataset, which contains hundreds of thousands of user-submitted recipes and their associated ratings.

We wanted to investigate whether it’s possible to predict a recipe’s *average rating* using only the information you’d know before cooking it—things like the number of ingredients, prep time, or calorie count.

Ultimately, our goal was to create a model that helps people make smarter cooking choices, minimizing the risk of culinary disappointment and maximizing the chance for a five-star dinner.

So why not join us on this journey to uncover the science behind deliciousness and find out: What truly makes a recipe worth your plate?

To build our model for predicting deliciousness, we analyzed two datasets from Food.com: one containing **recipes**, and another containing **user interactions** such as reviews and ratings. Originally compiled for the research paper *Generating Personalized Recipes from Historical User Preferences* by Majumder et al., these datasets offer a rich history of cooking behavior and taste preferences going back to 2008.

Our central question is:

> **Given only the information available before cooking a recipe, such as ingredients, time, and nutritional data: Can we predict how well a recipe will be rated?**

This question matters because online recipes can be hit-or-miss, and ratings often provide little context until after you've already tried cooking. By predicting ratings in advance, we can help users avoid disappointing dishes and make smarter culinary choices.

We used two datasets:

* The **recipes** dataset contains **83,782** rows (one per unique recipe).
* The **interactions** dataset contains **731,927** rows (each representing a user review and rating for a specific recipe).

Here are the relevant columns from each:

#### `recipes.csv`

| Column Name      | Description                                                                          |
| ---------------- | ------------------------------------------------------------------------------------ |
| `id`             | Unique identifier for the recipe                                                     |
| `name`           | Name of the recipe                                                                   |
| `minutes`        | Time in minutes to prepare the recipe                                                |
| `contributor_id` | User ID who submitted the recipe                                                     |
| `submitted`      | Date the recipe was submitted                                                        |
| `tags`           | Food.com tags for the recipe                                                         |
| `nutrition`      | Nutrition info: \[calories, total fat, sugar, sodium, protein, saturated fat, carbs] |
| `n_steps`        | Number of steps in the recipe                                                        |
| `n_ingredients`  | Number of ingredients used                                                           |

#### `interactions.csv`

| Column Name | Description                            |
| ----------- | -------------------------------------- |
| `user_id`   | ID of the user who reviewed the recipe |
| `recipe_id` | ID of the recipe being reviewed        |
| `date`      | Date of interaction                    |
| `rating`    | Rating from 1 to 5                     |
| `review`    | Written review text (if any)           |

These columns allow us to explore what factors which are available *before* tasting the food, might influence a recipe’s eventual rating.

<!-- 
-------------------
This is a *bare-minimum* template to create a Jekyll site that uses the [Just the Docs] theme. You can easily set the created site to be published on [GitHub Pages] – the [README] file explains how to do that, along with other details.

If [Jekyll] is installed on your computer, you can also build and preview the created site *locally*. This lets you test changes before committing them, and avoids waiting for GitHub Pages.[^1] And you will be able to deploy your local build to a different platform than GitHub Pages.

More specifically, the created site:

- uses a gem-based approach, i.e. uses a `Gemfile` and loads the `just-the-docs` gem
- uses the [GitHub Pages / Actions workflow] to build and publish the site on GitHub Pages

Other than that, you're free to customize sites that you create with this template, however you like. You can easily change the versions of `just-the-docs` and Jekyll it uses, as well as adding further plugins.

[Browse our documentation][Just the Docs] to learn more about how to use this theme.

To get started with creating a site, simply:

1. click "[use this template]" to create a GitHub repository
2. go to Settings > Pages > Build and deployment > Source, and select GitHub Actions

If you want to maintain your docs in the `docs` directory of an existing project repo, see [Hosting your docs from an existing project repo](https://github.com/just-the-docs/just-the-docs-template/blob/main/README.md#hosting-your-docs-from-an-existing-project-repo) in the template README.



[Just the Docs]: https://just-the-docs.github.io/just-the-docs/
[GitHub Pages]: https://docs.github.com/en/pages
[README]: https://github.com/just-the-docs/just-the-docs-template/blob/main/README.md
[Jekyll]: https://jekyllrb.com
[GitHub Pages / Actions workflow]: https://github.blog/changelog/2022-07-27-github-pages-custom-github-actions-workflows-beta/
[use this template]: https://github.com/just-the-docs/just-the-docs-template/generate -->
