---
title: "Introduction"
description: "Doks is a Hugo theme for building secure, fast, and SEO-ready documentation websites, which you can easily update and customize."
lead: "REST APIs often include dependency constraints that restrict how two or more parameters can be combined to form valid API calls. These inter-parameter dependencies make it difficult to automatically interact with the services, since API specification languages offer little or no support for them. We carried out a study on 40 industrial APIs and found that 85% of them contain inter-parameter dependencies. More importantly, we classified all the dependencies found (over 600) into seven patterns, serving as the basis for future proposals for modeling and analyzing inter-parameter dependencies automatically. "
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "idl"
weight: 100
toc: true
---

## Catalogue of inter-parameter dependencies

### 1. Requires
The Requires dependency is the simplest pattern. If a parameter is used (or it has a certain value), then another parameter must be used too (or be set to a certain value). 
{{< details "PayPal example" >}}
```
//PayPay-Create invoice 
IF custom.label THEN custom.amount;
IF minimum_amount_due THEN allow_partial_payment==true;
```
{{< /details >}}
{{< details "Google Map example" >}}
```
 
IF rankby=='distance' THEN NOT radius;
IF rankby=='distance' THEN Or(keyword, name, type);
minprice<=maxprice;
IF radius THEN NOT rankby;
```
{{< /details >}}

{{< details "Github example" >}}
```
//Github - Create deployment status
IF state=='inactive' THEN Accept LIKE '*application/vnd.github.ant-man-preview+json*';
IF state=='in_progress'|'queued' THEN Accept LIKE '*application/vnd.github.flash-preview+json*';
IF log_url THEN Accept LIKE '*application/vnd.github.ant-man-preview+json*';
IF environment THEN Accept LIKE '*application/vnd.github.flash-preview+json*';
IF environment_url THEN Accept LIKE '*application/vnd.github.ant-man-preview+json*';
IF auto_inactive THEN Accept LIKE '*application/vnd.github.ant-man-preview+json*';
```
{{< /details >}}
### 2. Or
Out of two or more parameters, at least one must be used. For example, in the Flickr API, when setting the information of a photo, the documentation states: “At least one of description or title must be set”.
{{< details "CityGrid example" >}}
```
//CityGrid - Places Detail 
Or(id, listing_id, infousa_id, public_id, phone);
AllOrNone(id, id_type);
Or(listing_id, public_id, infousa_id, phone);
image_size_min <= image_size_max;
```
{{< /details >}}

### 3. OnlyOne
Out of two or more parameters, one, and only one of them must be used. As an example, in the Twilio API, when sending an SMS, only one of From or MessagingServiceSid must be used: “You must also include either the From parameter or MessagingServiceSid parameter”.

{{< details "Weather Company Data example" >}}
```
//Weather Company Data - Search locations
OnlyOne(geocode, postalKey, iataCode, icaoCode);
```
{{< /details >}}

{{< details "Foursquare example" >}}
```
//Foursquare - Search Venues
OnlyOne(ll, near);
IF intent=='browse' THEN OnlyOne(ll AND radius, sw AND ne);
IF intent=='match' THEN name AND ll;
IF radius THEN intent=='browse' OR (intent=='checkin' AND (categoryId OR query));
AllOrNone(sw, ne);
IF sw THEN intent=='browse';
IF sw THEN NOT (ll OR radius);
IF ne THEN intent=='browse';
IF ne THEN NOT (ll OR radius);
IF intent=='match' THEN NOT categoryId;
AllOrNone(providerId, linkedId);
```
{{< /details >}}

### 4. AllOrNone
Out of two or more parameters, either all of them are used, or none of them. An instance of this dependency can be found in the GitHub API, between the parameters subject_id and subject_type of the operation GET /users/{username}/hovercard, as shown in the screenshot below:

{{< details "Stripe example" >}}
```
//Stripe - Create coupon
OnlyOne(amount_off, percent_off); 
IF amount_off THEN currency;
AllOrNone(duration=='repeating', duration_in_months); 
```
{{< /details >}}

### 5. ZeroOrOne
Out of two or more parameters, either zero or one can be used, but not more. This dependency can also be found in the YouTube search operation mentioned above, involving four input parameters forContentOwner, forDeveloper, forMine and relatedToVideoId:

{{< details "Google example" >}}
```
//Google Geocoding
OnlyOne(address OR components, latlng, place_id);
ZeroOrOne(bounds, latlng OR place_id);
ZeroOrOne(region, latlng OR place_id);
ZeroOrOne(result_type, address OR components);
ZeroOrOne(location_type, address OR components);
```
{{< /details >}}

{{< details "YouTube example" >}}
```
//YouTube - Get comment threads
OnlyOne(allThreadsRelatedToChannelId, channelId, id, videoId);
ZeroOrOne(id, moderationStatus);
ZeroOrOne(id, order);
ZeroOrOne(id, pageToken);
ZeroOrOne(id, searchTerms);
```
{{< /details >}}

{{< details "Tumblr example" >}}
```
//Tumblr - Get blog likes
ZeroOrOne(before, after, offset);
```
{{< /details >}}

### 6. Arithmetic/Relational
Two or more parameters are related by means of arithmetic (+, –, ×, ÷) or relational (<, ≤, >, ≥, =, ≠) operators. The most common shape of this dependency pattern is two parameters where one must be greater than or equal to the other, i.e., p1 ≥ p2. This happens with dates, times, IDs, prices, etc.

Other patterns are also possible. For example, in the Yelp API, when searching for businesses, the sum of limit + offset (parameters for paginating the results) must be less than or equal to 1000, otherwise the API returns an error: “Using the offset and limit parameters, you can get up to 1000 businesses from this endpoint if there are more than 1000 results. If you request a page out of this 1000 business limit, this endpoint will return an error”.

### 7. Complex
This pattern is simply a combination of two or more of the previous patterns. Based on our review, it is typical to see combinations of the Requires and the OnlyOne dependencies. See the following example from the Tumblr API. A post can be of different types, depending on the value of the type parameter. If type='video', then either the embed or the data parameter must be included too.


## Some stats
Some of these dependencies are more common than others. For example, Requires and ZeroOrOne dependencies are very recurrent across the APIs analyzed in our study [1], while Or and Complex are not so much. See the following charts for a summary.

<div class="card">
      <img src="https://miro.medium.com/max/1400/1*C_JU_DU3V90_ZWNHrhcmLw.png" alt="idl" >
</div>
Frequency of the dependencies according to the number of occurrences and the number of APIs (out of 40) presenting them.

### Tutorial

{{< alert icon="👉" text="The Tutorial is intended for novice to intermediate users." />}}

Step-by-step instructions on how to start a new Doks project. [Tutorial →](https://getdoks.org/tutorial/introduction/)

### Quick Start

{{< alert icon="👉" text="The Quick Start is intended for intermediate to advanced users." />}}

One page summary of how to start a new Doks project. [Quick Start →]({{< relref "quick-start" >}})

## Go further

Recipes, Reference Guides, Extensions, and Showcase.

### Recipes

Get instructions on how to accomplish common tasks with Doks. [Recipes →](https://getdoks.org/docs/recipes/project-configuration/)

### Reference Guides

Learn how to customize Doks to fully make it your own. [Reference Guides →](https://getdoks.org/docs/reference-guides/security/)

### Extensions

Get instructions on how to add even more to Doks. [Extensions →](https://getdoks.org/docs/extensions/breadcrumb-navigation/)

### Showcase

See what others have build with Doks. [Showcase →](https://getdoks.org/showcase/electric-blocks/)

## Contributing

Find out how to contribute to Doks. [Contributing →](https://getdoks.org/docs/contributing/how-to-contribute/)

## Help

Get help on Doks. [Help →]({{< relref "how-to-update" >}})
