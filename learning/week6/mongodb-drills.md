# Mongodb Drills


## 1- Find the listing detail information for the Evernote app?

{% mongoquery %}

{"n":"com.evernote"}

{% endmongoquery %}

{{ data | json }}

There are {{ data.length }} apps.
The star rating of the first Evernote app is: {{ data[0].rate }}

## 2- How many apps developed by GO Launcher EX?
{% mongoquery %}

{"crt":"GO Launcher EX"},{"n":1}

{% endmongoquery %}

{{ data | json }}

There are {{ data.length }} apps.

## 3- How many apps have been downloaded more than 10,000,000?
{% mongoquery %}
{"dct":{"$gt": 10000000}}
{% endmongoquery %}

{{ data | json }}

{{data.length}} apps have been downloaded more than 10,000,000 times

## 4- How many apps have the word "share" in their description?
Hint: the desc field is indexed with a text index. Use the $text operator.

{% mongoquery %}
{"$text":{"$search":"share"}}
{% endmongoquery %}

{{ data | json }}

There are {{data.length}} apps with the word "share" in the description

## 5- Find apps that are categorized in any of the following categories: Productivity, Business, or Finance?
Hint: Use the $in operator.

{% mongoquery %}
{"$or":[{"cat": "Productivity"}, {"cat": "Business"}, {"cat": "Finance"}]}
{% endmongoquery %}

{{ data | json }}

There are {{data.length}} apps in the Productivity, Business, and Finanance categories

## 6- Find apps that have been downloaded 10,000,000 or more but have star rating lower than 3 stars?
{% mongoquery %}
{"$and":[{"dct":{ "$gte":10000000}},{"rate":{"$lt":3}}]}
{% endmongoquery %}

{{ data | json }}

There are {{ data.length }} apps.

Hint: The query matches one app in the dataset and the link to its Google Play listing page is:
https://play.google.com/store/apps/details?id={{data[0].n}} Read the reviews to find out why?

## 7- Find the title and creator of apps that have been rated above 4 stars by 1,000,000 or more users?
{% mongoquery %}
{"$and":[{"rct":{"$gte":1000000}},{"rate":{"$gte":4}}]},{"n":1, "crt":1}
{% endmongoquery %}

{{ data | json }}

There are {{ data.length }} apps.

## 8- Find apps whose content rating is Teen but not categorized in any of the following categories: Card, Entertainment, Comics, and Puzzle?
{% mongoquery %}
{"$and":[{"crat":"Teen"}, {"cat":{"$nin":["Card", "Entertainment", "Comics", "Puzzle"]}}]}
{% endmongoquery %}

{{ data | json }}

There are {{ data.length }} apps.

## 9- Find the title, category, download count, and star rating of apps with the phrase "in-app purchase" in their description and download count greater than 10,000,000?
{% mongoquery %}
{"$and":[{"$text":{"$search":"in-app purchase"}},{"dct":{"$gt":10000000}}]},{"n":1, "cat":1, "dct":1, "rate":1}
{% endmongoquery %}

{{ data | json }}

There are {{ data.length }} apps.

## 10- Find the title and address of the creator of apps with download count greater than 1,000,000 and star rating greater than 4.5?
Hint: ensure that the results include apps that have creator address (not equal "")

{% mongoquery %}
{"$and":[{"cadd":{"$not":{"$eq":""}}},{"dct":{"$gt":1000000}},{"rate":{"$gt":4.5}}]},{"n":1, "cadd":1}
{% endmongoquery %}

{{ data | json }}

There are {{ data.length }} apps.

## 11- Find the title, category, download count, and rating of apps with download count of 1,000,000 or more but have no privacy ststements?

{% mongoquery %}
{"$and":[{"dct":{"$gte":1000000}}, {"purl":{"$eq":""}}]}, {"n":1,"cat":1,"dct":1,"rate":1}
{% endmongoquery %}

{{ data | json }}

There are {{ data.length }} apps.

## 12- Find apps that have been released or updated this month (September 2015)?
Hint: You can use the $regex operator

{% mongoquery %}
{"$and":[{"dtp":{"$regex":"September.*"}},{"dtp":{"$regex":".*2015"}}]}
{% endmongoquery %}

{{ data | json }}

There are {{ data.length }} apps
