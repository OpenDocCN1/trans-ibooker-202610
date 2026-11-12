
## References

15. [15](#c05.xhtml#ic05-fn-15) “Business Analytics, Hybrid Cloud & Consulting | Teradata,” accessed April 7, 2020, [`https://www.teradata.com/Press-Releases/2018/Teradata-Vantage-the-Platform-for-Pervasive`](https://www.teradata.com/Press-Releases/2018/Teradata-Vantage-the-Platform-for-Pervasive)

16. [16](#c05.xhtml#ic05-fn-16) “R: The R Project for Statistical Computing,” accessed April 7, 2020, [`https://www.r-project.org/`](https://www.r-project.org/).

17. [17](#c05.xhtml#ic05-fn-17) Alex Jones, “Top 10 Data Analysis Tools for Business,” KDNuggets, June 2014, [`www.kdnuggets.com/2014/06/top-10-data-analysis-tools-business.html`](http://www.kdnuggets.com/2014/06/top-10-data-analysis-tools-business.html).

18. [18](#c05.xhtml#ic05-fn-18) OpenRefine.org

19. [19](#c05.xhtml#ic05-fn-19) “Crime in the U.S., 2013,” Federal Bureau of Investigation website, [`www.fbi.gov/about-us/cjis/ucr/crime-in-the-u.s/2013/crime-in-the-u.s.-2013`](http://www.fbi.gov/about-us/cjis/ucr/crime-in-the-u.s/2013/crime-in-the-u.s.-2013).

20. [20](#c05.xhtml#ic05-fn-20) “Machine Learning, Data Science, Big Data, Analytics, AI,” accessed April 7, 2020, [`https://www.kdnuggets.com/`](https://www.kdnuggets.com/).

# Chapter 6: Big Data End User and Accounting Tools

## Learning Objectives

* Recognize how optimization is used with big data.
* Recall an actual example of big data in the electronics retail business.
* Identify other big data resources.

## Introduction

This chapter examines accounting tools that can be used for big data analysis. The reader may be familiar with some of the tools but may not have considered the use of the tools as a resource for analyzing big data. One of these tools is very helpful in the analysis of generating sales and referrals. We’ll start our discussion by examining the use of big data in a real-life example, using data pulled from a nationwide retailer. The example will illustrate how to access, dissect, and evaluate big data, both in terms of data contained in the information as well as inferences that can be drawn from the big data.

## Actual Example of Big Data

In this section, the participant is going to be involved in a real-life big data example. As you look at the example, keep in mind the following:

* Notice the “look” of real big data.
* Determine if any trends can be identified within the big data.
* Speculate on how the trends changed.
* Determine what management actions could be taken in view of the data.

## Example of Accessing Actual Corporate Data — Best Buy

Best Buy allows the public to access some of its big data. You can access the data by visiting [`https://developer.bestbuy.com/`](https://developer.bestbuy.com/).

![](images/ufig6-1.png)
![](images/ufig6-2.png)

It is necessary to get an `API key`, so click on the box in the middle of the image where it says Get API Key. A key will be assigned to be used when accessing the applicable apps.

In this chapter, we will illustrate Best Buy’s Developer program.

### `Query Builder`

`Query Builder` helps users get the most out of the Best Buy APIs by creating custom queries. Create a base for your own custom queries or use the `Query Builder` to access Best Buy API data. You can even customize your responses to better serve your purpose.

Examples of Best Buy and other companies apps that access the Best Buy Developer tools as well as the app’s function include the following:

### `IFTTT`

`IFTTT` helps you create powerful connections with one simple statement: “If This, Then That.” `IFTTT` has already integrated with channels like Facebook, Evernote, LinkedIn, email and now Best Buy. With the Best Buy channel, you can create `IFTTT` recipes that include product price, trend and availability changes. Check out Best Buy’s sample recipes on `IFTTT`.

### `Factual`

`Factual`’s Global Places is a definitive database of listings for more than 65 million local businesses and points of interest in 50 countries. Global Places uses the store’s API to incorporate data on more than one thousand Best Buy store locations, giving access to dependable information for developers worldwide.

### `Listia`

`Listia` is a popular online marketplace that allows users to post items they no longer need to earn credits that can be redeemed for items listed by other users or new merchandise posted by Best Buy and other retailers. By partnering with `Listia`, Best Buy is helping customers reduce clutter, rescuing still-usable electronics from landfills and finding good homes for new tech gear.

### `TrackIf`

`TrackIf` is an innovative shopping aggregator that searches the web to find exactly the products you’re looking for. `TrackIf` will scour its partner sites for the best bargain of the day, as well as price-drop and supply notifications.

### `ShopSavvy`

Best Buy’s partnership with `ShopSavvy` allows customers to find low online prices even when shopping in store. Shoppers can scan in-store items using the `ShopSavvy` app to compare competitor prices, find sales, and make online purchases and more.

### `CitiBank`

`CitiBank`’s ThankYou Rewards allow customers to earn points by using a participating credit card or checking account. Those points can be redeemed for a wide array of products, including tablets, gaming consoles and other electronics from Best Buy. Customers can pick up their purchases at the nearest Best Buy store or have the products shipped, making it a model of convenience for shoppers in all walks of life and a fantastic discovery tool for Best Buy customers.

### `Jifiti`

`Jifiti` is a clever app for anyone weary of dropping fruitless hints about that upcoming birthday or other event. A list of suggested gifts is presented to your event guests and their gift lands as a virtual gift card in your email. It’s a thoughtful gift that allows both giver and receiver flexibility in choosing their favorites.

### `Best Buy App`

The `Best Buy App` puts a Best Buy store in your pocket. Browse the entire catalog, read reviews, compare specs, track My Best Buy rewards points and more. You can even make purchases right from your phone for home delivery or in-store pickup.

### `RedLaser`

The `RedLaser` app allows users to scan `UPC` and `QR` codes at most major retail stores and receive instant price comparison to direct you to the lowest available price, as well as any applicable coupons or special offers. You can even cut down on web browsing time by making your purchase within the application, and My Best Buy customers can even scan their cards for instant access to loyalty benefits.

### `FindTheBest`

`FindTheBest` is an online research engine that provides detailed information on more than 2,000 topics, enabling people to research with confidence. `FindTheBest` leverages our `Products API` to include timely pricing and product details for Best Buy products.

After the author had received his key to access the data (via email), he returned to the gallery page referenced previously and clicked on `Query Builder`.

The following image depicts the process to access the Best Buy catalog.

![](images/ufig6-3.png)

Source: Best Buy website, [`http://bestbuyapis.github.io/bby-query-builder/#/productSearch`](http://bestbuyapis.github.io/bby-query-builder/#/productSearch).

Note that there are a number of options that can be included in the query, such as `category` and `attribute`. (See the following images.)

![](images/ufig6-4.png)
![](images/ufig6-5.png)

The author entered the `data key` and ran the query for the `most viewed products`. The following image shows the `query detail`.

![](images/ufig6-6.png)

## Knowledge Checks

1. `Query Builder` allows a user to do what?
    1. Perform “what if” analysis.
    2. Dispose of electronic items they no longer need.
    3. Write custom queries for Best Buy products.
    4. Use software to create, “If this, then that” statements for data analysis.
