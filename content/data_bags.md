+++
title = "About Data Bags"
draft = false
gh_repo = "chef-web-docs"
aliases = ["/data_bags.html", "/secrets.html", "/secrets/", "/essentials_data_bags.html"]
product = ["client", "server"]

[menu]
  [menu.infra]
    title = "Data Bags"
    identifier = "chef_infra/policyfiles/data_bags.md Data Bags"
    parent = "chef_infra/policyfiles"
    weight = 40
+++

{{% data_bag %}}

## Create a Data Bag

{{% data_bag_create %}}

### Create a Data Bag with Knife

{{% data_bag_create_knife %}}

### Manually

{{% data_bag_create_manual %}}

## Store Data in a Data Bag

{{% data_bag_store %}}

### Directory Structure

{{% data_bag_directory_structure %}}

### Data Bag Items

{{% data_bag_item %}}

## Encrypt a Data Bag Item

{{% data_bag_encryption %}}

### Encryption Versions

{{% data_bag_encryption_versions %}}

### Knife Options

{{% data_bag_encryption_knife_options %}}

### Secret Keys

{{% data_bag_encryption_secret_key %}}

### Encrypt

{{% data_bag_encryption_encrypt %}}

### Verify Encryption

{{% data_bag_encryption_verify %}}

### Decrypt

{{% data_bag_encryption_decrypt %}}

## Edit a Data Bag Item

{{% data_bag_edit %}}

### Edit a Data Bag with Knife

{{% knife_data_bag_edit %}}

{{% knife_data_bag_edit_item %}}

## Use Data Bags

Data bags can be accessed in the following ways:

### Search

{{% data_bag_search %}}

### Environments

{{% data_bag_environments %}}

### Recipes

{{% data_bag_recipes %}}

#### Load with Chef Infra Language

{{% data_bag_recipes_load_using_recipe_dsl %}}

#### Create and edit

{{% data_bag_recipes_edit_within_recipe %}}

#### Create users

{{% data_bag_recipes_create_users %}}

### chef-solo

{{% data_bag_chef_solo %}}
