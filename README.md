This repository contains buy.php. 

The project description is :
You will use PHP sessions and the PHP SimpleXML interface. Using SimpleXML, to access all children of $x with tag A, use $x->A. To access the value of an attribute A of an element $x, use $x['A']. To store a SimpleXML value $x in the $_SESSION, you need to coerce it to a string first using (string)$x.

$xmlstr = file_get_contents('someURL.com');
$xml = new SimpleXMLElement($xmlstr);
foreach ($xml->BookList->BookData as $b)
    foreach ($b->Prices->Price as $p)
        if ($p['store_id'] == "amazon")
            print $b['isbn'] . ' ' . $b->Title[0] . ' ' . $p['price'] . "\n";

Project Requirements

You will develop a trivial web application that allows customers to buy products. You need to reverse-engineer my PHP script:

https://lambda.uta.edu/cgi-bin/php/buy.php

by filling out its forms and by looking at the HTML source that is generated by this script. Your task is to modify your own buy.php script in your project3 directory on Omega to make it behave like my buy.php, although you should make your script generate better HTML code to look nicer.

You will use the shopping.com API for shopping. The eBay Commerce Network API ("ECN API") is a flexible way to access and recreate practically everything you see on Shopping.com. First, you need to get an API key. You may use my API key in the given buy.php. You will use the Search by keyword method and the Requesting category tree information -> Include all descendants in category tree method from the eBay Commerce Network Publisher API Use Cases.

Your search form must have a menu to select a category, a text window to specify search keywords, and a submit button. The menu must contain all sub-categories of the category "computers" (whose id is 72). The menu items must be generated in your PHP code by requesting all the descendants of the category tree with id=72 (the computers category). The result of a keyword search must contain up to 20 products within the selected category that best match the keyword query. Each product contains a link productOffersURL to the shopping.com web page that gives a detailed description of the product and a list of best offers from various sellers. So each product has a range minPrice - maxPrice of the prices offered by these sellers. We will ignore the list of offers and we will assume that when we buy this product we pay the minPrice.

You need to use a PHP session to store the shopping basket (the list of chosen items throughout the session) as well as the results of the previous search. For each chosen item, you store the Id, the name, the minPrice, the first image, and the productOffersURL (the link to the shopping.com web page that lists the best offers for this item). Your PHP script must be able to handle the following query strings:

    To search for all items in the "Laptops" category that match the search keywords "samsung i7":

    buy.php?category=9007&search=samsung+i7

    This should call the web service request http://sandbox.api.ebaycommercenetwork.com/publisher/3.0/rest/GeneralSearch?apiKey=xxx&trackingId=yyy&category=9007&keyword=samsung+i7&numItems=20, where xxx and yyy are your access apiKey and your trackingId. The web service results will arrive in XML. To see how the XML data look like, cut and paste this URL on your browser and look at the reply.

    To put a listed item with id=138681275 in the shopping basket (if it does not exist):

    buy.php?buy=138681275

    Your script should look at the results of the last search stored in $_SESSION to find the item under this Id and should copy it into the shopping basket.

    To remove a listed item with Id=138681275 from the shopping basket (if exists):

    buy.php?delete=138681275

    To clear the shopping basket:

    buy.php?clear=1

Hints: you may use the PHP function print_r for debugging. It prints human-readable information about a variable. 
