Magento-Functions
=================

A Resource of Magento Functions


####Table of Contents
***

1. [Category](#category)
2. [Product](#product)
3. [User](#user)
4. [Cart](#cart)
5. [Checkout](#checkout)
6. [General](#general)
7. [Account](#account)
8. [Working w/ URL's] (#urls)

***


##Category

##Product

####Fetch Product Collection
```php

$collection = Mage::getModel('catalog/product')
              ->getCollection()
              ->addAttributeToSelect('*')
              ->addAttributeToSort('name', 'DESC')
              ->setOrder('name', 'ASC');
```

####Magento Load Products
```php
Individual Product Helper
--------------------------------------------------------------------------------------
$_helper = $this->helper('catalog/output');
$_product = $this->getProduct();

Load Product from Collection
--------------------------------------------------------------------------------------
// Load by name
$_product = Mage::getModel('catalog/product')->loadByAttribute('name', 'product_name');

// Load by SKU
$_product = Mage::getModel('catalog/product')->loadByAttribute('sku', '234SKU93');

//Load by ID (just load):
$_product = Mage::getModel('catalog/product')->load($productID);
```

####Fetch Default Product Info
```php
<?php
echo $_product->getShortDescription(); //product's short description
echo $_product->getDescription(); // product's long description
echo $_product->getName(); //product name
echo $_product->getSku(); //product Sku
echo $_product->getPrice(); //product's regular Price
echo $_product->getSpecialPrice(); //product's special Price
echo $_product->getProductUrl(); //product url
echo $_product->getImageUrl(); //product's image url
echo $_product->getSmallImageUrl(); //product's small image url
echo $_product->getThumbnailUrl(); //product's thumbnail image url
echo $_product->getAvailability(); //product's thumbnail image url     
?>
```

####Custom Product Attributes
```php
For drop-down Product Attributes use the following code 
--------------------------------------------------------------------------------------
<?php echo $_product->getAttributeText('attribute_name') ?>


For all other Product Attribute types
--------------------------------------------------------------------------------------
<?php echo $_product->getAttributeName() ?>

-or-

<?php echo $_product['attribute_name'];?>


Display Product Attributes Globally
--------------------------------------------------------------------------------------
<?php if($product->getResource()->getAttribute('ATTRIBUTE_CODE_HERE')->getFrontend()->getValue($product)) : ?> 

 <?php echo 'Attribute Title: '.$product->getResource()->getAttribute('ATTRIBUTE_CODE_HERE')->getFrontend()->getValue($product); ?>  

<?php endif; ?>
```
####Product Stock Count
```php
<?php 
$stock_count = (int) Mage::getModel(’cataloginventory/stock_item’)->loadByProduct($this->getProduct())->getQty(); 
echo $stock_count;
?>
```

##User
```php
/* Check if the customer is logged in or not */
if (Mage::getSingleton('customer/session')->isLoggedIn()) {
 
    /* Get the customer data */
    $customer = Mage::getSingleton('customer/session')->getCustomer();
    /* Get the customer's full name */
    $fullname = $customer->getName();
    /* Get the customer's first name */
    $firstname = $customer->getFirstname();
    /* Get the customer's last name */
    $lastname = $customer->getLastname();
    /* Get the customer's email address */
    $email = $customer->getEmail();
 
}
```

##Cart
```php
/* Get all the items in the cart */
$cartItems = Mage::getSingleton('checkout/session')->getQuote()->getAllItems();
/* Iterate through the items */
foreach ($cartItems as $item) {
 /* Load the product and get the custom attribute */
 Zend_Debug::dump(Mage::getModel('catalog/product')->load($item->getProduct()->getId())->getMyCustomAttribute());
}
```

##Checkout


####Shipping
```php
Retrive Shipping Method from Quote
--------------------------------------------------------------------------------------
$rate = Mage::getSingleton('checkout/session')->getQuote()->getShippingAddress()->getShippingRatesCollection();

$rate->getCarrier(); // This will provide you with the carrier code
$rate->getCarrierTitle(); // This will give you the carrier title
$rate->getCode(); // This will give you **current shipping method** code
$rate->getMethod(); // This will provide you with the **shipping method** code
$rate->getMethodTitle(); // This will tell you current shipping method title
$rate->getMethodDescription(); // And this is the description of the current shipping method and **it could be NULL**
```

##General


####Working with Blocks

```
Step 1:
Create your static block
 
Step 2:
Open the file that references the page you intend to put the block into IE‘page.xml’.
 
Step 3:
Add this to the appropriate place in the XML file
 
<block type="cms/block" name="xxxxxx">
  <action method="setBlockId"><block_id>xxxxxx</block_id></action>
</block>
 
Step 4:
Navigate your way to the template folder (app > design > frontend > default > your_theme > template) Open the file that you would like the block to appear in and insert the following code in the appropriate position where xxxxxx is the ‘Identifier’ you set earlier when creating your block.
 
<?php echo $this->getChildHtml('xxxxxx') ?>
 
 
Render Block within .phtml
 
<?php echo $this->getLayout()->createBlock('cms/block')->setBlockId('my-new-block')->toHtml() ?>
```

####Passing Data Through Blocks
```
<!-- Push Data to Child Block -->
<?php $this->getChild('block_name')->setData("product", $_product); ?>

<!-- Set Product Data Pulled through from view template file -->
<?php  $_product = $this->getData('product'); ?>
```

##Account


####Navigation Links

#####Removing Navigation Links
In local.xml add the following
```
<customer_account>
    <reference name="customer_account_navigation" >
            <action method="removeLinkByName"><name>recurring_profiles</name></action>
            <action method="removeLinkByName"><name>billing_agreements</name></action>
            <action method="removeLinkByName"><name>reviews</name></action>
            <action method="removeLinkByName"><name>downloadable_products</name></action>
            <action method="removeLinkByName"><name>OAuth Customer Tokens</name></action>

            <action method="removeLinkByName"><name>account</name></action>
            <action method="removeLinkByName"><name>account_edit</name></action>
            <action method="removeLinkByName"><name>address_book</name></action>
            <action method="removeLinkByName"><name>orders</name></action>
            <action method="removeLinkByName"><name>tags</name></action>
            <action method="removeLinkByName"><name>wishlist</name></action>
            <action method="removeLinkByName"><name>newsletter</name></action>
    </reference>
</customer_account>

```
#####Adding Navigation Links
In local.xml add the following
```
<customer_account>
    <reference name="customer_account_navigation">
        <action method="addLink">
            <name>my_new_section</name>
            <path>module/index/index</path>
            <!-- <url>http://mydomain.com</url> -->
            <label>Module Account View</label>
            <params><_secure>true</_secure></params>
        </action>
    </reference>
</customer_account>
```



##URLs

```php

To Retrieve URL path in STATIC BLOCK
---------------------------------------
To get SKIN URL
{{skin url='images/sampleimage.jpg'}}
 
To get Media URL
{{media url='/sampleimage.jpg'}}
 
To get Store URL
{{store url='mypage.html'}}
 
To get Base URL
{{base url='yourstore/mypage.html'}}
 
 
TO Retrieve URL path in PHTML
---------------------------------------
 
Not secure Skin URL:
<?php echo $this->getSkinUrl('images/sampleimage.jpg') ?>
 
Secure Skin URL
<?php echo $this->getSkinUrl('images/ sampleimage.gif', array('_secure'=>true)) ?>
 
Get  Current URL
$current_url = Mage::helper('core/url')->getCurrentUrl();
 
Get Home URL
$home_url = Mage::helper('core/url')->getHomeUrl();
 
Get Magento Media Url
Mage::getBaseUrl(Mage_Core_Model_Store::URL_TYPE_LINK);
 
Get Magento Media Url
Mage::getBaseUrl(Mage_Core_Model_Store::URL_TYPE_MEDIA);
 
Get Magento Skin Url
Mage::getBaseUrl(Mage_Core_Model_Store::URL_TYPE_SKIN);
 
Get Magento Store Url
Mage::getBaseUrl(Mage_Core_Model_Store::URL_TYPE_WEB);
 
Get Magento Js Url
Mage::getBaseUrl(Mage_Core_Model_Store::URL_TYPE_JS);



Mage::getBaseUrl() => Gets base url path e.g. http://my.website.com/

Mage::getBaseUrl('media') => Gets MEDIA folder path e.g. http://my.website.com/media/

Mage::getBaseUrl('js') => Gets JS folder path e.g. http://my.website.com/js/

Mage::getBaseUrl('skin') => Gets SKIN folder path e.g. http://my.website.com/skin/

Get DIRECTORY paths (physical location of your folders on the server) – Relative URL Path

Mage::getBaseDir() => Gives you your Magento installation folder / root folder e.g. /home/kalpesh/workspace/magento

Mage::getBaseDir('app') => Gives you your Magento's APP directory file location e.g. /home/kalpesh/workspace/magento/app

Mage::getBaseDir('design') => Gives you your Magento's DESIGN directory file location e.g. /home/kalpesh/workspace/magento/design

Mage::getBaseDir('media') => Gives MEDIA directory file path

Mage::getBaseDir('code') => Gives CODE directory file path

Mage::getBaseDir('lib') => Gives LIB directory file path

Mage::helper('core/url')->getCurrentUrl()

base Mage::getBaseDir()

Mage::getBaseDir('base') /var/www/magento/

app Mage::getBaseDir('app') /var/www/magento/app/

code Mage::getBaseDir('code') /var/www/magento/app/code

design Mage::getBaseDir('design') /var/www/magento/app/design/

etc Mage::getBaseDir('etc') /var/www/magento/app/etc

lib Mage::getBaseDir('lib') /var/www/magento/lib

locale Mage::getBaseDir('locale') /var/www/magento/app/locale

media Mage::getBaseDir('media') /var/www/magento/media/

skin Mage::getBaseDir('skin') /var/www/magento/skin/

var Mage::getBaseDir('var') /var/www/magento/var/

tmp Mage::getBaseDir('tmp') /var/www/magento/var/tmp

cache Mage::getBaseDir('cache') /var/www/magento/var/cache

log Mage::getBaseDir('log') /var/www/magento/var/log

session Mage::getBaseDir('session') /var/www/magento/var/session

upload Mage::getBaseDir('upload') /var/www/magento/media/upload

export Mage::getBaseDir('export') /var/www/magento/var/export
```



