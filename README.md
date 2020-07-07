# GSTD (Google Sheets to Drupal) Migration - Example
### THIS IS A WORK IN PROGRESS
July 7th, 2020: I'm literally in the middle of getting this migration working...and have created this repo because I need some help with a few of the configurations. But since I thought it best to upload these files to share with my prospective helpers, I thought the info might also become useful now or in the future for others struggling with Drupal migrations that have less than "standard" requirements. I wish there was better documentation on many of the more complex migration plugins, so hopefully, through documenting my own migration, I can help improve that.

### Introduction
I struggled for a while trying to get a complex migration to work (and in fact, I'm still working on it). But if what I've learned and created can help you, great! This Drupal 8.9+ destined migration is really a *set* of migrations. It's based on a migration I needed to perform for a client. This migration demonstrates how you can create references to other nodes, users, multiple taxonomy terms, and proper media entities using a Google Spreadsheet as your source. You'll be able to migrate image files, and then turn those image files into proper media entities that can be accessed via the Drupal Media Manager. You'll see how to use the  office_hours plugin in your migration, as well as how to properly target subfields of an address from the address module. You'll also see how migration dependancies work, as most of these migrations require and are dependent on some of the other migrations to be performed first.

### The Google Spreadsheet Source
You'll need to make a copy of this spreadsheet:
```
https://docs.google.com/spreadsheets/d/1GuKab6Ezi3GckSjMD5BmAQVa0qnOKJX9kadWz_mDxcU
```
*Please Note: When you make a COPY of the Google Spreadsheet, you will get a unique URL/id for the document. You'll need to use THAT new URL/id in your own migrations by modifying the source urls in each migration .yml sheet, andof course, you'll want a sheet you can edit if you want to start with a working template.*

There are FOUR key pages to the Google Spreadsheet, and each sheet is has column data which is linked in some way to the data in another sheet. Our migrations are also dependant on other migrations, just like our google sheets are.

1. business_branch: This is our primary migration (file: gstd_node_business_location.yml). This sheet contains most of our business data. It depends on all the other migrations to happen first though.
2. file_image: This sheet lists all the image url's (and titles/alt columns). We reference these rows in our business_branch sheet in the "Photo ID" column (though I made a dropdown that autofills in the file id when you select the file title name from the "Photo Title 1" and "Photo Title 2" dropdowns). Currently, I have only been able to successfully link ONE media file via migrations. I'm working on a solution now.
3. business_top: This sheet is where we list "Parent Businesses." Since some businesses have multiple locations, I provisioned another content type that will allow for better control of child business locations. The important thing here, is that "Parent Business Name/PB Ref" columns on the business_branch sheet reference the parent businesses on the business_top sheet (just like the photo files)
4. users: while importing business_top nodes, business_branch nodes, and media entities, I wanted to assign users as "creators" of each of these pieces of content because ultimately, we intent to give these users control over their respective business listings. Again, just like the other sheets, all users from the users sheet are references in the business_branch sheet under the "Username/User ID" columns

### Migration files
In the migrations folder, there are 5 migration configurations:
- `gstd_file_image.yml` (source: sheet 2)
- `gstd_file_image_to_media.yml` (source: sheet 2)
- `gstd_node_business_locations.yml` (source: sheet 1)
- `gstd_node_parent_business.yml` (source: sheet 3)
- `gstd_users.yml` (source: sheet 4)

### Requirements
You'll need to create content types "business_top" and "business_branch" with the corresponding fields using the correct field types. I haven't exported the content/node/field config files yet to make this a comprehensive working module. I'll do this soon.

More Documentation coming soon...
