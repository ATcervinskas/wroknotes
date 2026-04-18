
Enable guest check
/-------------------------------------------------------

Customers - >groups -> visitors -> edit -> show prices (will enables cart)
/-------------------------------------------------------
Removed quote button cart icon inside quates module and add padding to button

#fmm_quote_button {
    padding: 9px;
}

/-------------------------------------------------------

unhook social mdeia share module from displayProductAdditionalInfo

/-------------------------------------------------------
displayNav2 unhook shopping cart

/-------------------------------------------------------

%price% € be PVM. add to translations search by tax excl

/-------------------------------------------------------

add this line                     <p class="product-without-taxes">{l s='%price% tax excl.' d='Shop.Theme.Catalog' sprintf=['%price%' => $product.price_tax_exc]}</p>
in  <div class="current-price">

/-------------------------------------------------------
Terms and conditions GDPR  module translate and config
/-------------------------------------------------------

/----------------------------------------------------------
 product.tpl product_price_and_shipping take out from   product-description"
 /--------------------------------------------------
 produt.tpl add check if prices is not null display add to cart 
      {if $product.price != 0}
                                {block name='product_add_to_cart'}
                                    {include file='catalog/_partials/product-add-to-cart.tpl'}
                                {/block}
                                {/if}
/-----------------------------------------------------
If trying to delete product from shopping cart dosenot work 
Step 1: Open your core.js file which is inside your themes directory and edit it.

Step 2: Search for code below:

o.default.cart=e.resp.cart;
And replace it with:

o.default.cart=e.reason.cart;
Step 3: Reset your ps_shoppingcart module from your BO -> Modules.

Step 4: Go to your BO -> Advanced Parameters -> and click on Clear Cache button.

Step 5: Go to your store front-office, press Ctrl+Shift+R (or ⌘+shift+R on mac) to refresh your browser cookies.

You're done!
/--------------------------------------------------------------------
 Prestashop htaccess Regenerate
Log into your Prestashop back office.
Go to Configure > Shop Paramters > Traffic & SEO.
Now find out the Set up URLs > Friendly URL.
Set “Friendly URL” No and click on the Save button.
Set “Friendly URL” Yes and click on the save button.
/-------------------------------------------------------------------
fields in address
Customers > Addresses > Set required fields for this section > Phone_mobile

Make sure youre added in locations > countries > phone_mobile
/--------------------------------------------------------------------------------/
http://dezomeda.local/admin089otzrdx/index.php/sell/orders/2/generate-invoice-pdf download invoice 


$db = \Db::getInstance(_PS_USE_SQL_SLAVE_);
$carId = $db->getValue('SELECT id_cart FROM `' . _DB_PREFIX_ . 'orders` WHERE reference = \'UQZWJAAML\'');
$results = $db->executeS('SELECT * FROM `' . _DB_PREFIX_ . 'dpd_pudo_cart` WHERE `id_cart` = ' . (int)$carId);
die();


set define('PS_TAX_EXC', 0); in define.inc 