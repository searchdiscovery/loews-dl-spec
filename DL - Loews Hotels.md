![SDI Logo](https://www.searchdiscovery.com/wp-content/uploads/2017/03/SDI-Black.svg)

# DL - Loews Hotels

## Index of Contents

* [Summary](#Summary)
* [Accommodation Booking Cancelled](#Accommodation-Booking-Cancelled)
* [Beacon Globals Setup](#Beacon-Globals-Setup)
* [Checkout Started](#Checkout-Started)
* [Discount Code Entry Failed](#Discount-Code-Entry-Failed)
* [Discount Code Entry Succeeded](#Discount-Code-Entry-Succeeded)
* [Download Link Clicked](#Download-Link-Clicked)
* [Exit Link Clicked](#Exit-Link-Clicked)
* [Form Submission Failed](#Form-Submission-Failed)
* [Interstitial Viewed](#Interstitial-Viewed)
* [Listing Filter Added](#Listing-Filter-Added)
* [Listing Filter Removed](#Listing-Filter-Removed)
* [Listing Sort Order Changed](#Listing-Sort-Order-Changed)
* [Location Listing Displayed](#Location-Listing-Displayed)
* [Onsite Search Performed](#Onsite-Search-Performed)
* [Order Placed](#Order-Placed)
* [Page Load Started](#Page-Load-Started)
* [Room Listing Displayed](#Room-Listing-Displayed)
* [Room Listing Item Clicked](#Room-Listing-Item-Clicked)
* [User Detected](#User-Detected)
* [User Profile Updated](#User-Profile-Updated)
* [User Registered](#User-Registered)
* [User Signed In](#User-Signed-In)
* [User Signed Out](#User-Signed-Out)
* [Video Completed](#Video-Completed)
* [Video Milestone Reached](#Video-Milestone-Reached)
* [Video Started](#Video-Started)

## Summary

This repository contains the Data Layer specification for Loews Hotels.
Each section in the file represents a separate use case that needs to be implemented on the site.



## Accommodation Booking Cancelled

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Accommodation Booking Cancelled",
  "booking": {
    "roomList": [
      {
        "location": {
          "locationId": "<locationId>"
        },
        "room": {
          "typeCode": "<typeCode>",
          "rateCode": "<rateCode>",
          "numAdults": "<numAdults>",
          "numKids": "<numKids>"
        }
      }
    ],
    "transactionID": "<transactionID>",
    "cancellationID": "<cancellationID>"
  }
});
```

|Field|Type|Description|Examples|Pattern|Min Length|Max Length|Minimum|
|---|---|---|---|---|---|---|---|
|cancellationID|string|Unique identifier of a cancellation of a booking. Typically not the same as the booking ID.|CN-34456789|||||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448|||||
|numAdults|integer|Integer number of adults for the booking.|1, 2, 3, 4, 5||||1|
|numKids|integer|Integer number of kids for the booking.|1, 2, 3, 4, 5||||0|
|rateCode|string|Description of the rate being offered. Should match rate codes from back-end systems to allow data import.|AAA, MILITARY, CORP-567, CORP-345|||||
|transactionID|string|Unique identifier of the transaction. Max Length 20. Used as a key for upload of post transaction data.||^[a-zA-Z0-9]{6,20}$|6|20||
|typeCode|string|A code describing the room features. Often indicates number of beds, smoking or non-smoking, ADA accessibility and so on.|1-K-NS, 2-Q-S, S-K-NS-City|||||


## Accommodation Booking Completed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Accommodation Booking Completed",
  "booking": {
    "roomList": [
      {
        "room": {
          "ratePerNight": "<ratePerNight>",
          "typeCode": "<typeCode>",
          "numKids": "<numKids>",
          "numAdults": "<numAdults>",
          "rateCode": "<rateCode>"
        },
        "location": {
          "locationId": "<locationId>"
        }
      }
    ],
    "total": {
      "currency": "<currency>"
    },
    "transactionID": "<transactionID>",
    "payment": [
      {
        "paymentMethod": "<paymentMethod>"
      }
    ]
  }
});
```

|Field|Type|Description|Examples|Pattern|Min Length|Max Length|Minimum|
|---|---|---|---|---|---|---|---|
|currency|string|Currency of the payment for the booking. ISO 4217 (3 character alpha), uppercase|USD, CAD, GBP, CHF|^[A-Z]{3}$|3|3||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448|||||
|numAdults|integer|Integer number of adults for the booking.|1, 2, 3, 4, 5||||1|
|numKids|integer|Integer number of kids for the booking.|1, 2, 3, 4, 5||||0|
|paymentMethod|string|Describes the method of payment for a transaction.|Credit Card, PayPal, Mastercard, Visa, Amex, Discover|||||
|rateCode|string|Description of the rate being offered. Should match rate codes from back-end systems to allow data import.|AAA, MILITARY, CORP-567, CORP-345|||||
|ratePerNight|string|String representation of the price per use-period. Typically nightly rate for a hotel room or monthly rate for an apartment. Positive. Up to two decimal places for cents. No currency symbol.|200, 75.29, 150, 89.2|^[0-9]*(\.[0-9]{1,2})?$||||
|transactionID|string|Unique identifier of the transaction. Max Length 20. Used as a key for upload of post transaction data.||^[a-zA-Z0-9]{6,20}$|6|20||
|typeCode|string|A code describing the room features. Often indicates number of beds, smoking or non-smoking, ADA accessibility and so on.|1-K-NS, 2-Q-S, S-K-NS-City|||||


## Beacon Globals Setup

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Beacon Globals Setup"
});
```



## Checkout Started

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Checkout Started"
});
```



## Discount Code Entry Failed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Discount Code Entry Failed",
  "voucherDiscount": {
    "discountCode": "<discountCode>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|discountCode|string|Discount code entered or applied|5OFFSHOES, AKRONCANDLES2019|


## Discount Code Entry Succeeded

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Discount Code Entry Succeeded",
  "voucherDiscount": {
    "discountCode": "<discountCode>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|discountCode|string|Discount code entered or applied|5OFFSHOES, AKRONCANDLES2019|


## Download Link Clicked

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Download Link Clicked",
  "linkInfo": {
    "fileName": "<fileName>",
    "linkId": "<linkId>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|fileName|string|Indicates the filename for download link tracking.|Year End 2012.pdf, Operating Instructions.doc`|
|linkId|string|Identifier of the link clicked|act now, cancel, ok, 3456, 8765|


## Exit Link Clicked

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Exit Link Clicked",
  "linkInfo": {
    "linkId": "<linkId>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|linkId|string|Identifier of the link clicked|act now, cancel, ok, 3456, 8765|


## Form Submission Failed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Form Submission Failed",
  "form": {
    "formError": "<formError>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|formError|string|Error text or code describing a form error. This is the form-level error.|Credit card declined, Required entries missing, EC3456, EC8976|


## Interstitial Viewed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Interstitial Viewed",
  "interstitial": {
    "viewType": "<viewType>",
    "name": "<name>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|viewType|string|Type of interstitial view|alert, offer, info required|


## Listing Filter Added

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Listing Filter Added",
  "listingRefined": {
    "filterList": "<filterList>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|filterList|string|A twice delimited string of filterType and filterValue pairs. Use ~ between type and value. Use | between pairs|sort~price ascending|color~green|size~medium|


## Listing Filter Removed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Listing Filter Removed",
  "listingRefined": {
    "filterList": "<filterList>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|filterList|string|A twice delimited string of filterType and filterValue pairs. Use ~ between type and value. Use | between pairs|sort~price ascending|color~green|size~medium|


## Listing Sort Order Changed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Listing Sort Order Changed",
  "listingRefined": {
    "sortOrder": "<sortOrder>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|sortOrder|string|Indicates the sort order.|high-low, low-high, nearest-farthest, a-z, newest-oldest|


## Location Listing Displayed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Location Listing Displayed",
  "listingDisplayed": {
    "listingDriver": "<listingDriver>",
    "listingType": "<listingType>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|listingDriver|string|Describes the action that caused the listing to be displayed|Onsite Search, Curated Assortment, Navigation|
|listingType|string|The type of results being listed|text, product, location, event, room, product location|


## Offer Successfully Applied

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Offer Successfully Applied",
  "offerDiscount": {
    "offerCode": "<offerCode>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|offerCode|string|Offer code applied|5OFFSTAY, NEWYORKPARKING|


## Onsite Search Performed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Onsite Search Performed",
  "onsiteSearch": {
    "keyword": {
      "searchType": "<searchType>"
    },
    "scheduling": {
      "startDate": "<startDate>",
      "requestedDatesCount": "<requestedDatesCount>",
      "daysBeforeStartDate": "<daysBeforeStartDate>",
      "endDate": "<endDate>"
    },
    "capacity": {
      "people": {
        "numAdults": "<numAdults>",
        "numChildren": "<numChildren>"
      }
    }
  }
});
```

|Field|Type|Description|Examples|Pattern|Minimum|
|---|---|---|---|---|---|
|daysBeforeStartDate|integer|Integer Number of days until the requested start date (same day = 0)|0, 1, 5, 6, 7, 10||0|
|endDate|string|End date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|numAdults|integer|Integer number of adults for the booking|1, 2, 3, 4, 5||1|
|numChildren|integer|Integer number of kids for the booking|1, 2, 3, 4, 5||0|
|requestedDatesCount|integer|Integer Number of days requested.|8, 1, 5, 6, 7, 10||1|
|searchType|string|Describes the domain of the search.|products, properties, articles, authors, coupons, publications|||
|startDate|string|Start date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||


## Order Placed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Order Placed",
  "transaction": {
    "item": [
      {
        "price": {
          "sellingPrice": "<sellingPrice>"
        },
        "productInfo": {
          "productID": "<productID>"
        },
        "quantity": "<quantity>"
      }
    ],
    "transactionID": "<transactionID>",
    "total": {
      "currency": "<currency>"
    }
  }
});
```

|Field|Type|Description|Examples|Pattern|Min Length|Max Length|Minimum|
|---|---|---|---|---|---|---|---|
|currency|string|Currency of the transaction. ISO 4217 (3 character alpha), uppercase|USD, CAD, GBP, CHF|^[A-Z]{3}$|3|3||
|productID|string|Unique Identifier of a product or offering. Must match the format of back-end systems if used as a key for import of product meta data. Most often, one level above SKU for products with SKU variants.|155, 65588, 987764448|||||
|quantity|integer|Integer number of products being acted upon (added to a cart, removed from wishlist, purchased, reserved)|1, 2, 3, 4, 5||||1|
|sellingPrice|string|String representation of the price paid after coupons or discounts. Positive. Up to two decimal places for cents. No currency symbol.|200, 29.99, 50, 0|^[0-9]*(\.[0-9]{1,2})?$||||
|transactionID|string|Unique identifier of the transaction. Max Length 100. Used as a key for upload of post transaction data.||^[a-zA-Z0-9]{6,20}$|6|100||


## Page Load Started

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Page Load Started",
  "page": {
    "pageName": "<pageName>",
    "pageType": "<pageType>",
    "pageCategory": "<pageCategory>",
    "subsection": "<subsection>",
    "breadCrumbs": "<breadCrumbs>",
    "siteLanguage": "<siteLanguage>",
    "siteExperience": "<siteExperience>"
  }
});
```

|Field|Type|Description|Examples|Pattern|
|---|---|---|---|---|
|breadCrumbs|unknown||||
|pageCategory|string|General category or Site Section of the page. Top level of page hierarchy.|Home, About Us, Shop, Account, Blog, Investors||
|pageName|string|Describes the page and its content specifically.|product - XYZ123, Mens - Tops - Sweaters, Order Confirmation||
|pageType|string|Describes what purpose the page serves. Often aligns with the CMS template.|Home, Event Detail, Property Detail, Product Listing, Blog Post, Shopping Cart||
|siteExperience|string|Describes the version of the site that is being shown|Responsive, Mobile, Desktop||
|siteLanguage|string|Language in which the site is presented ISO 639-1 code.|en-us, en-gb, ch-cn, fr-ca, fr-fr, da|^[a-z]{2}([-]{1}[a-z]{2}){0,1}$|
|subsection|string|First sub-level of hierarchy under pageCategory or Site Section.|Shop > Kids, Shop > Mens, Shop > Womens||


## Room Listing Displayed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Room Listing Displayed",
  "listingDisplayed": {
    "listingDriver": "<listingDriver>",
    "listingType": "<listingType>",
    "listing": [
      {
        "location": {
          "locationId": "<locationId>"
        },
        "room": {
          "typeCode": "<typeCode>"
        }
      }
    ],
    "resultsCount": "<resultsCount>",
    "previousResultsCount": "<previousResultsCount>",
    "filterList": {
      "length": "<length>"
    },
    "sortDefault": "<sortDefault>",
    "sortOrder": "<sortOrder>"
  }
});
```

|Field|Type|Description|Examples|Minimum|
|---|---|---|---|---|
|filterList|string|A twice delimited string of filterType and filterValue pairs. Use ~ between type and value. Use | between pairs|sort~price ascending|color~green|size~medium||
|length|integer|Native Javascript Array property. Returns the number of filters in the listingDisplayed.filterList array|0, 20, 12|0|
|listingDriver|string|Describes the action that caused the listing to be displayed|Onsite Search, Curated Assortment, Navigation||
|listingType|string|The type of results being listed|text, product, location, event, room, product location||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448||
|resultsCount|integer|The total number of items returned that matched the search criteria. (Integer)|1, 21, 111, 166|0|
|previousResultsCount|integer|The total number of items returned for the previous search, when subsequent filters are applied to a current search. (Integer)|1, 21, 111, 166|0|
|sortDefault|integer|The default sort value on listings|A to Z, Low to High, Newest to Oldest|0|
|sortOrder|string|Indicates the sort order.|high-low, low-high, nearest-farthest, a-z, newest-oldest||
|typeCode|string|A code describing the room features. Often indicates number of beds, smoking or non-smoking, ADA accessibility and so on.|1-K-NS, 2-Q-S, S-K-NS-City||


## Room Listing Item Clicked

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Room Listing Item Clicked"
});
```



## User Detected

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "User Detected",
  "user": {
    "loginStatus": "<loginStatus>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|loginStatus|string|Describes the login state of the user|logged in, logged out, guest|


## User Profile Updated

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "User Profile Updated",
  "user": {
    "profileUpdateType": "<profileUpdateType>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|profileUpdateType|string|Captures the type of profile update (i.e. change email) completed by the visitor.|email, address, phone, subscriptions|


## User Registered

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "User Registered",
  "user": {
    "custKey": "<custKey>",
    "registrationType": "<registrationType>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|custKey|string|Unique identifier of a customer. Any id's considered PII must be hashed.||
|registrationType|string|Describes the thing that was registered for|account, loyalty program, event, sweepstakes, warranty|


## User Signed In

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "User Signed In",
  "user": {
    "custKey": "<custKey>",
    "loginStatus": "<loginStatus>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|custKey|string|Unique identifier of a customer. Any id's considered PII must be hashed.||
|loginStatus|string|Describes the login state of the user|logged in, logged out, guest|


## User Signed Out

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "User Signed Out",
  "user": {
    "custKey": "<custKey>"
  }
});
```

|Field|Type|Description|
|---|---|---|
|custKey|string|Unique identifier of a customer. Any id's considered PII must be hashed.|


## Video Completed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Video Completed",
  "video": {
    "videoID": "<videoID>",
    "videoName": "<videoName>",
    "secondsLength": "<secondsLength>"
  }
});
```

|Field|Type|Description|Examples|Minimum|
|---|---|---|---|---|
|secondsLength|integer|The total length of the video in seconds|36, 67, 178, 600|0|
|videoID|string|Video ID|YT456789, BC4567890, 876546789||
|videoName|string|Video Name|Twitch_FPS, Age of Empires, Halo||


## Video Milestone Reached

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Video Milestone Reached",
  "video": {
    "milestone": "<milestone>",
    "videoID": "<videoID>",
    "videoName": "<videoName>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|milestone|string|The milestone being tracked as an integer less than 100|25, 50, 77, 99|
|videoID|string|Video ID|YT456789, BC4567890, 876546789|
|videoName|string|Video Name|Twitch_FPS, Age of Empires, Halo|


## Video Started

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Video Started",
  "video": {
    "videoID": "<videoID>",
    "videoName": "<videoName>",
    "secondsLength": "<secondsLength>"
  }
});
```

|Field|Type|Description|Examples|Minimum|
|---|---|---|---|---|
|secondsLength|integer|The total length of the video in seconds|36, 67, 178, 600|0|
|videoID|string|Video ID|YT456789, BC4567890, 876546789||
|videoName|string|Video Name|Twitch_FPS, Age of Empires, Halo||
