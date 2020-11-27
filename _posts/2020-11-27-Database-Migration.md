---
layout: post
title: Database Migration
order: 10
---

I've added the capability to set different roles for users. For now, a user can be a customer or an admin. In the future we may have more roles for the different types of partner portals I create.

I set up a new `role` table and a new `role_user` table using a Rails migration file. This will allow me to store a list of roles and store each specific user's role in the database!

I also created the Role model and RoleUser model to set up the connections in rails. I added validations and updated our `admin?` check on the user to look at the association!

I also created a seed file for my development database to create a customer role and an admin role and a user with each of these roles.

I added this line to my Procfile so that the database migrations would run automatically
```
release: bundle exec rake db:migrate
```
Lastly, I wrote specs to test out these new associations and validations.

Next I'll work on updating the views for the dashboard and admin landing pages.
