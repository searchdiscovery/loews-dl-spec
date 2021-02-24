![SDI Logo](https://www.searchdiscovery.com/wp-content/uploads/2017/03/SDI-Black.svg)

# DL - Loews Hotels

## Index of Contents

* [Summary](#Summary)
* [Event Order](#Event-Order)
* [Accommodation Booking Cancelled](#Accommodation-Booking-Cancelled)
* [Accommodation Booking Completed](#Accommodation-Booking-Completed)
* [Availability Listings Displayed](#Availability-Listings-Displayed)
* [Checkout Started](#Checkout-Started)
* [Discount Code Entry Failed](#Discount-Code-Entry-Failed)
* [Discount Code Entry Succeeded](#Discount-Code-Entry-Succeeded)
* [Download Link Clicked](#Download-Link-Clicked)
* [Exit Link Clicked](#Exit-Link-Clicked)
* [Form Submission Failed](#Form-Submission-Failed)
* [Interstitial Viewed](#Interstitial-Viewed)
* [Listing Filter Added](#Listing-Filter-Added)
* [Listing Filter Removed](#Listing-Filter-Removed)
* [Location Listing Displayed](#Location-Listing-Displayed)
* [Onsite Search Performed](#Onsite-Search-Performed)
* [Page Load Started](#Page-Load-Started)
* [Page Load Completed](#Page-Load-Completed)
* [Rate Detail Viewed](#Rate-Detail-Viewed)
* [Rate Selected](#Rate-Selected)
* [Related Product Addition](#Related-Product-Addition)
* [Room Detail Viewed](#Room-Detail-Viewed)
* [Room Listing Displayed](#Room-Listing-Displayed)
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
Each section in the file represents a separate use case that needs to be implemented on the site. Details on the deployment of each specification below can be found at https://docs.google.com/spreadsheets/d/1jd5Ep6VKfyVEhrl4CFvfo4WLYZxD0aoIm2GQV8Ep1DA/edit?usp=sharing.

## Event Order

As this site is a single page application, the order in which events are dispatched from the application are critical to proper analytics functioning. First there is an event below named [Page Load Started](#Page-Load-Started) that contains "page" information such as page name, site section, etc. Whenever a scenario occurs where there is a standard "page view" OR an "on view," this [Page Load Started](#Page-Load-Started) event should be first dispatched, followed by any relevant "On View" events and then finally, the [Page Load Completed](#Page-Load-Completed) event would be dispatched, indicating that all relevant data about out a page or view was ready and the final communication can be sent to the analytics platform.

---> Page Load Started > On View Events > Page Load Completed

## Accommodation Booking Cancelled

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Accommodation Booking Cancelled",
  "booking": {
    "roomList": [
      {
        "location": {
          "locationId": "<locationId>",
          "marketCode": "<marketCode>"
        },
        "room": {
          "typeCode": "<typeCode>",
          "rateCode": "<rateCode>",
          "numAdults": "<numAdults>",
          "numChildren": "<numChildren>",
          "cancelAdvanceDays": "<cancelAdvanceDays>",
          "cancelDaysAfterBooking": "<cancelDaysAfterBooking>"
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
|cancelAdvanceDays|integer|The number of days between the cancellation and the start date of the stay.|7|||||
|cancelDaysAfterBooking|integer|The number of days between the creation of the booking and the cancelation.|3|||||
|cancellationID|string|Unique identifier of a cancellation of a booking. Typically not the same as the booking ID.|CN-34456789|||||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448|||||
|marketCode|string|Unique identifier of the market code.|123, 65588, 987764448||
|numAdults|integer|Integer number of adults for the booking.|1, 2, 3, 4, 5||||1|
|numChildren|integer|Integer number of kids for the booking.|1, 2, 3, 4, 5||||0|
|rateCode|string|Description of the rate beinged. Should match rate codes from back-end systems to allow data import.|AAA, MILITARY, CORP-567, CORP-345|||||
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
          "numChildren": "<numChildren>",
          "numAdults": "<numAdults>",
          "rateCode": "<rateCode>",
          "marketCode": "<marketCode>",
        },
        "location": {
          "locationId": "<locationId>"
        },
        "scheduling": {
          "startDate": "<startDate>",
          "daysBeforeStartDate": "<daysBeforeStartDate>",
          "endDate": "<endDate>",
          "numNights": "<numNights>"
        }
      }
    ],
    "discountList":[
        {
          "discount": {
          "discountAmount": "<discountAmount>",
          "discountCode": "<discountCode>"
          }
        }
    ],
    "total": {
      "currency": "<currency>",
      "paymentAmount": "<paymentAmount>",
      "totalRooms": "<totalRooms>",
      "totalNights": "<totalNights>"
      
    },
    "checkoutType": "<checkoutType>",
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
|checkoutType|string|The type of checkout associated with the completed booking.|travel agent, consumer, booking for someone else||
|currency|string|Currency of the payment for the booking. ISO 4217 (3 character alpha), uppercase|USD, CAD, GBP, CHF|^[A-Z]{3}$|3|3||
|daysBeforeStartDate|integer|Integer Number of days until the requested start date (same day = 0)|0, 1, 5, 6, 7, 10||0|
|endDate|string|End date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448|||||
|marketCode|string|Unique identifier of the market code.|123, 65588, 987764448||
|numAdults|integer|Integer number of adults for the booking.|1, 2, 3, 4, 5||||1|
|numChildren|integer|Integer number of kids for the booking.|1, 2, 3, 4, 5||||0|
|numNights|integer|Total number of nights in the stay.|1, 3, 5||
|paymentAmount|string|String representation of the total payment made as a part of the transaction, not including tax. Positive. Up to two decimal places for cents. No currency symbol.|425.57|||||
|paymentMethod|string|Describes the method of payment for a transaction.|Credit Card, PayPal, Mastercard, Visa, Amex, Discover|||||
|discountAmount|string|String representation of booking level discount for a transaction. Positive. Up to two decimal places for cents. No currency symbol.|350.65|||||
|discountCode|string|Discount code applied at the booking level of a transaction..|10% off stay|||||
|rateCode|string|Description of the rate being offered. Should match rate codes from back-end systems to allow data import.|AAA, MILITARY, CORP-567, CORP-345|||||
|ratePerNight|string|String representation of the price per use-period. Typically nightly rate for a hotel room or monthly rate for an apartment. Positive. Up to two decimal places for cents. No currency symbol.|200, 75.29, 150, 89.2|^[0-9]*(\.[0-9]{1,2})?$||||
|startDate|string|Start date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|totalNights|integer|Total number of nights in the booking. If multiple rooms are booked, this is the total of nights across all rooms.|5, 7, 10||
|totalRooms|integer|Total number of rooms in the booking. If multiple rooms are booked, this is the total of rooms booked.|2, 3, 4||
|transactionID|string|Unique identifier of the transaction. Max Length 20. Used as a key for upload of post transaction data.||^[a-zA-Z0-9]{6,20}$|6|20||
|typeCode|string|A code describing the room features. Often indicates number of beds, smoking or non-smoking, ADA accessibility and so on.|1-K-NS, 2-Q-S, S-K-NS-City|||||




## Availability Listings Displayed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Availability Listings Displayed",
  "availabilityListingsDisplayed": {
    "findingMethod": "<findingMethod>",
    "locationId": "<locationId>",
    "marketCode": "<marketCode>",
    "rateCode": "<rateCode>"
  }
});
```

|Field|Type|Description|Examples|Pattern|Minimum|
|---|---|---|---|---|---|
|findingMethod|string|The method that brought the user to availability listings.|Onsite Search, Breadcrumbs, Direct Entry||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448||
|marketCode|string|Unique identifier of the market code.|123, 65588, 987764448||
|rateCode|string|Description of the rate being offered. Should match rate codes from back-end systems to allow data


## Checkout Started

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Checkout Started"
  "booking": {
    "roomList": [
      {
        "room": {
          "ratePerNight": "<ratePerNight>",
          "typeCode": "<typeCode>",
          "numChildren": "<numChildren>",
          "numAdults": "<numAdults>",
          "rateCode": "<rateCode>",
          "marketCode": "<marketCode>"
        },
        "location": {
          "locationId": "<locationId>"
        },
        "scheduling": {
          "startDate": "<startDate>",
          "daysBeforeStartDate": "<daysBeforeStartDate>",
          "endDate": "<endDate>",
          "numNights": "<numNights>",
        }
      }
    ]
  }
});
```

|Field|Type|Description|Examples|Pattern|Min Length|Max Length|Minimum|
|---|---|---|---|---|---|---|---|
|daysBeforeStartDate|integer|Integer Number of days until the requested start date (same day = 0)|0, 1, 5, 6, 7, 10||0|
|endDate|string|End date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448|||||
|marketCode|string|Unique identifier of the market code.|123, 65588, 987764448||
|numAdults|integer|Integer number of adults for the booking.|1, 2, 3, 4, 5||||1|
|numChildren|integer|Integer number of kids for the booking.|1, 2, 3, 4, 5||||0|
|numNights|integer|Total number of nights in the stay.|1, 3, 5||
|rateCode|string|Description of the rate being offered. Should match rate codes from back-end systems to allow data import.|AAA, MILITARY, CORP-567, CORP-345|||||
|ratePerNight|string|String representation of the price per use-period. Typically nightly rate for a hotel room or monthly rate for an apartment. Positive. Up to two decimal places for cents. No currency symbol.|200, 75.29, 150, 89.2|^[0-9]*(\.[0-9]{1,2})?$||||
|startDate|string|Start date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|typeCode|string|A code describing the room features. Often indicates number of beds, smoking or non-smoking, ADA accessibility and so on.|1-K-NS, 2-Q-S, S-K-NS-City|||||


## Discount Code Entry Failed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Discount Code Entry Failed",
  "voucherDiscount": {
    "discountCode": "<discountCode>",
    "discountType": "<discountType>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|discountCode|string|Discount code entered or applied|10offnight, freelunch|
|discountType|string|Discount type entered or applied|rate, add-on|


## Discount Code Entry Succeeded

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Discount Code Entry Succeeded",
  "voucherDiscount": {
    "discountCode": "<discountCode>",
    "discountType": "<discountType>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|discountCode|string|Discount code entered or applied|10offnight, freelunch|
|discountType|string|Discount type entered or applied|rate, add-on|


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
|name|string|Name of interstitial being viewed|alert, offer, info required|


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
      "daysBeforeStartDate": "<daysBeforeStartDate>",
      "endDate": "<endDate>",
      "numNights": "<numNights>"
    },
    "locationId": "<locationId>",
    "marketCode": "<marketCode>",
    "rateCode": "<rateCode>"
    "capacity": {
      "people": {
        "numRooms": "<numRooms>",
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
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448||
|marketCode|string|Unique identifier of the market code.|123, 65588, 987764448||
|numNights|integer|Total number of nights in the stay.|1, 3, 5||
|rateCode|string|Description of the rate being offered. Should match rate codes from back-end systems to allow data
|numRooms|integer|Integer number of rooms searched for|1, 2, 3, 4, 5||1|
|numAdults|integer|Integer number of adults for the booking|1, 2, 3, 4, 5||1|
|numChildren|integer|Integer number of children for the booking|1, 2, 3, 4, 5||0|
|searchType|string|Describes the domain of the search.|products, properties, articles, authors, coupons, publications|||
|startDate|string|Start date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||


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


## Page Load Completed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Page Load Completed"
});
```


## Rate Detail Viewed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Rate Detail Viewed",
  "booking": [
    {
      "roomlist": {
        "typeCode": "<typeCode>",
        "locationID": "<locationID>",
        "marketCode": "<marketCode>",
        "rateCode": "<rateCode>"
        },
        "capacity": {
          "people": {
            "numRooms": "<numRooms>",
            "numAdults": "<numAdults>",
            "numChildren": "<numChildren>"
        },
        "scheduling": {
          "startDate": "<startDate>",
          "daysBeforeStartDate": "<daysBeforeStartDate>",
          "endDate": "<endDate>",
          "numNights": "<numNights>"
        }
      }
    }
  ]
});
```
|Field|Type|Description|Examples|Pattern|Min Length|Max Length|Minimum|
|---|---|---|---|---|---|---|---|
|daysBeforeStartDate|integer|Integer Number of days until the requested start date (same day = 0)|0, 1, 5, 6, 7, 10||0|
|endDate|string|End date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448|||||
|numAdults|integer|Integer number of adults for the booking.|1, 2, 3, 4, 5||||1|
|numChildren|integer|Integer number of children for the booking.|1, 2, 3, 4, 5||||0|
|numNights|integer|Total number of nights in the stay.|1, 3, 5||
|numRooms|integer|Total number of rooms in the stay.|1, 3, 5||
|rateCode|string|Description of the rate being offered. Should match rate codes from back-end systems to allow data import.|AAA, MILITARY, CORP-567, CORP-345|||||
|startDate|string|Start date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|typeCode|string|A code describing the room features. Often indicates number of beds, smoking or non-smoking, ADA accessibility and so on.|1-K-NS, 2-Q-S, S-K-NS-City|||||


## Rate Selected

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Rate Selected",
  "rateSelectionmethod": "<rateSelectionmethod>",
  "booking": [
    {
      "roomlist": {
        "typeCode": "<typeCode>",
        "locationID": "<locationID>",
        "marketCode": "<marketCode>",
        "rateCode": "<rateCode>",
        },
        "capacity": {
          "people": {
            "numAdults": "<numAdults>",
            "numChildren": "<numChildren>"
        },
        "scheduling": {
          "startDate": "<startDate>",
          "daysBeforeStartDate": "<daysBeforeStartDate>",
          "endDate": "<endDate>",
          "numNights": "<numNights>"
        }
      }
    }
  ]
});
```
|Field|Type|Description|Examples|Pattern|Min Length|Max Length|Minimum|
|---|---|---|---|---|---|---|---|
|daysBeforeStartDate|integer|Integer Number of days until the requested start date (same day = 0)|0, 1, 5, 6, 7, 10||0|
|endDate|string|End date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448|||||
|numAdults|integer|Integer number of adults for the booking.|1, 2, 3, 4, 5||||1|
|numChildren|integer|Integer number of children for the booking.|1, 2, 3, 4, 5||||0|
|numNights|integer|Total number of nights in the stay.|1, 3, 5||
|rateCode|string|Description of the rate being offered. Should match rate codes from back-end systems to allow data import.|AAA, MILITARY, CORP-567, CORP-345|||||
|rateSelectionMethod|The rate selection method associated with the rate selection.|Same as Room 1, Standard|||||
|startDate|string|Start date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|typeCode|string|A code describing the room features. Often indicates number of beds, smoking or non-smoking, ADA accessibility and so on.|1-K-NS, 2-Q-S, S-K-NS-City|||||


## Related Product Addition

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Related Product Addition",
  "product": [
  {
    "productInfo": {
      "productID": "<productID>",
      "locationID": "<locationID>",
      "priceType": "<priceType>",
      "basePrice": "<basePrice>",
      "quantity": "<quantity>"
    }
  }
]
});

});
```
|Field|Type|Description|Examples|Pattern|Min Length|Max Length|Minimum|
|---|---|---|---|---|---|---|---|
|productID|string|Product ID that was added to the booking|12345, breakfast|
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448||
|priceType|string|Describes the price type of the related product|per night\/day, one time|
|basePrice|integer|cost of single unit of the product|15.00, 25.45|
|quantity|integer|The quantity of the addition that was added to the booking|1, 3|


## Room Detail Viewed

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "Room Detail Viewed",
  "booking": [
    {
      "roomlist": {
        "typeCode": "<typeCode>",
        "locationID": "<locationID>",
        "marketCode": "<marketCode>",
        "rateCode": "<rateCode>"
        },
        "capacity": {
          "people": {
            "numRooms": "<numRooms>",
            "numAdults": "<numAdults>",
            "numChildren": "<numChildren>"
        },
        "scheduling": {
          "startDate": "<startDate>",
          "daysBeforeStartDate": "<daysBeforeStartDate>",
          "endDate": "<endDate>"
        }
      }
    }
  ]
});
```
|Field|Type|Description|Examples|Pattern|Min Length|Max Length|Minimum|
|---|---|---|---|---|---|---|---|
|daysBeforeStartDate|integer|Integer Number of days until the requested start date (same day = 0)|0, 1, 5, 6, 7, 10||0|
|endDate|string|End date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|locationId|string|Unique Identifier of a Location.|155, 65588, 987764448|||||
|numAdults|integer|Integer number of adults for the booking.|1, 2, 3, 4, 5||||1|
|numChildren|integer|Integer number of children for the booking.|1, 2, 3, 4, 5||||0|
|numRooms|integer|Total number of rooms in the stay.|1, 3, 5||
|rateCode|string|Description of the rate being offered. Should match rate codes from back-end systems to allow data import.|AAA, MILITARY, CORP-567, CORP-345|||||
|startDate|string|Start date requested. ISO 8601 form (YYYY-MM-DD). Jan 1, 2019 is 2019-01-01|2001-12-22, 2011-01-01|^([0-9]{4})-(1[0-2]|0[1-9])-(3[01]|0[1-9]|[12][0-9])$||
|typeCode|string|A code describing the room features. Often indicates number of beds, smoking or non-smoking, ADA accessibility and so on.|1-K-NS, 2-Q-S, S-K-NS-City|||||


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
|sortDefault|integer|The default sort value on listings|A to Z, Low to High, Newest to Oldest|0|
|sortOrder|string|Indicates the sort order.|high-low, low-high, nearest-farthest, a-z, newest-oldest||
|typeCode|string|A code describing the room features. Often indicates number of beds, smoking or non-smoking, ADA accessibility and so on.|1-K-NS, 2-Q-S, S-K-NS-City||


## User Detected

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "User Detected",
  "user": {
    "loginStatus": "<loginStatus>",
    "custKey": "<custKey>"
    }
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|loginStatus|string|Describes the login state of the user|logged in, logged out, guest|
|custKey|string|Unique identifier of a customer if logged in. Any id's considered PII must be hashed.||


## User Profile Updated

```js
window.appEventData = window.appEventData || [];
appEventData.push({
  "event": "User Profile Updated",
  "user": {
    "profileUpdateType": "<profileUpdateType>"
  },
  "roomPreferences": {
  "preferenceList": "<preferenceList>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|profileUpdateType|string|Captures the type of profile update (i.e. change email) completed by the visitor.|email, address, phone, subscriptions|
|preferenceList|string|A twice delimited string of preferenceType and preferenceValue pairs. Use ~ between type and value. Use the pipe character between pairs.|floor\|low floor,pillows\|soft|


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
    "loginStatus": "<loginStatus>",
    "loginMethod": "<loginMethod>"
  }
});
```

|Field|Type|Description|Examples|
|---|---|---|---|
|custKey|string|Unique identifier of a customer. Any id's considered PII must be hashed.||
|loginStatus|string|Describes the login state of the user|logged in, logged out, guest|
|loginMethod|string|Describes the method in which the user was brought to sign-in|global footer, checkout faster, etc.|


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
