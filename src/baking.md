---
layout: page
title: Baking
images:
  - image_path: /images/baking/baking_1.jpg
    title: Sponge Cake
  - image_path: /images/baking/baking_2.jpg
    title: Poppy Bread
  - image_path: /images/baking/baking_3.jpg
    title: Sourdough
  - image_path: /images/baking/baking_4.jpg
    title: Pretzel Bread
  - image_path: /images/baking/baking_5.jpg
    title: Mint Cake
  - image_path: /images/baking/baking_6.jpg
    title: Banana Bread
  - image_path: /images/baking/baking_8.jpg
    title: Peach Melba Tart
  - image_path: /images/baking/baking_9.jpg
    title: Apple Crumb Cake
  - image_path: /images/baking/baking_10.jpg
    title: Everything Bagel Baguette
  - image_path: /images/baking/baking_11.jpg
    title: Pumpkin Pie
  - image_path: /images/baking/baking_12.jpg
    title: Brioche Jelly Doughnuts
  - image_path: /images/baking/baking_13.jpg
    title: Carrot Cake
  - image_path: /images/baking/baking_14.jpg
    title: Smore Cronuts
  - image_path: /images/baking/baking_15.jpg
    title: Sourdough Loaves
  - image_path: /images/baking/baking_16.jpg
    title: Mojito Tart
  - image_path: /images/baking/baking_17.jpg
    title: Creme Brulee Cake
  - image_path: /images/baking/baking_18.jpg
    title: Pretzel Rolls
  - image_path: /images/baking/baking_19.jpg
    title: The Cookies
---

<div class="photo-gallery">
  {% for image in page.images %}
    <img src="{{ image.image_path }}" alt="{{ image.title}}"/>
  {% endfor %}
</div>