+++
title = "Testing Django Rest API with Pytest"
author = ["jeanjayquitayen"]
date = 2023-05-14T00:00:00+08:00
categories = ["Tutorials"]
draft = false
+++

If you are wondering how to use pytest with Django DRF then you are in for some treat.
Pytest is great for unit testing but when working with DRF some work needs to be done.
Luckily there is `drf_pytest` to help with structuring the test cases.


## Project Setup {#project-setup}

We will be using the Django blog project for the tutorial.

```sh
django-admin start project blog
```
