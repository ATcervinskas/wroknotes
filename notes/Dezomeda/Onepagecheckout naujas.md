1. disable onepagechechout

modules/ets_onepagecheckout/views/templates/hook/layout_4.tpl 

move additional info before  block-address

![[Pasted image 20250826203850.png]]

```js
  
$(document).ready(function () {  
    var $checkbox = $('#additional_info_3_Įmonė_0');  
    var $fields = $('#additional_info_4, #additional_info_5, #additional_info_6, #additional_info_7');  
  
    function toggleFields() {  
        if ($checkbox.is(':checked')) {  
            $fields.closest('.form-group')  
                .show()  
                .find('input').addClass('is_required')  
                .end()  
                .find('label').addClass('required');  
        } else {  
            $fields.closest('.form-group')  
                .hide()  
                .find('input').removeClass('is_required')  
                .end()  
                .find('label').removeClass('required');  
        }    }  
    // Initial check  
    toggleFields();  
  
    // On change  
    $checkbox.on('change', toggleFields);  
});
```


HTMLtemplateInvoice.php

```php
'additional_data '=>  array_map(fn($item) => ['title' => $item['title'], 'value' => $item['value']], Ets_opc_additionalinfo_field_value::getFieldValuesByIDCart($this->order->id_cart))
```

`/override/classes/pdf/HTMLTemplateInvoice.php`
```php
<?php

class HTMLTemplateInvoice extends HTMLTemplateInvoiceCore
{
    public function getContent()
    {
        // build the data array as usual …
        $data = parent::getContent(); // if you don’t call parent, copy the original method body

        // Build additional_data
        $raw = Ets_opc_additionalinfo_field_value::getFieldValuesByIDCart((int)$this->order->id_cart);
        $additional = array_map(function ($item) {
            return ['title' => $item['title'], 'value' => $item['value']];
        }, (array)$raw);

        // assign to smarty (important!)
        $this->smarty->assign([
            'additional_data' => $additional,
        ]);

        // return the original fetch
        return parent::getContent();
    }
}

```

![[Pasted image 20250826214905.png]]

![[Pasted image 20250826214915.png]]
![[Pasted image 20250826214923.png]]
![[Pasted image 20250826214930.png]]![[Pasted image 20250826214936.png]]

**DPD to show even before a guest creates an address**. Since most DPD modules won’t quote without postcode/city, the only reliable way is to **feed them a placeholder address** when none exists yet.

Below is a safe, reversible approach that works with any module expecting a normal PrestaShop address.

```mysql
-- 1) Create a technical customer (if you don't already have one)
INSERT INTO ps_customer (id_shop_group, id_shop, id_gender, firstname, lastname, email, passwd, date_add, date_upd)
VALUES (1, 1, 0, 'DPD', 'Placeholder', 'dpd-placeholder@example.com', MD5(RAND()), NOW(), NOW());
SET @cust_id = LAST_INSERT_ID();

-- 2) Create its address
INSERT INTO ps_address (id_country, id_state, id_customer, alias, company, lastname, firstname, address1, postcode, city, phone, active, date_add, date_upd)
VALUES (
  (SELECT id_country FROM ps_country WHERE iso_code='LT' LIMIT 1),
  0,
  @cust_id,
  'DPD Placeholder',
  'DPD Placeholder Ltd',
  'Placeholder',
  'DPD',
  'Antakalnio g. 1',
  '01100',        -- valid 5-digit LT ZIP
  'Vilnius',
  '+37060000000',
  1,
  NOW(),
  NOW()
);
SET @addr_id = LAST_INSERT_ID();

-- 3) Store the id_address in configuration
INSERT INTO ps_configuration (name, value, date_add, date_upd)
VALUES ('DPD_PLACEHOLDER_ID_ADDRESS', @addr_id, NOW(), NOW())
ON DUPLICATE KEY UPDATE value = VALUES(value), date_upd = NOW();

```

Create `/override/classes/Cart.php`:
```php
<?php
class Cart extends CartCore
{
    public function getDeliveryOptionList($default_country = null, $dontAutoSelectOptions = false, $use_cache = true)
    {
        // If the cart has no delivery address (guest not saved yet),
        // temporarily use the DPD placeholder address to fetch carrier options.
        if (!$this->id_address_delivery) {
            $idPlaceholder = (int) Configuration::get('DPD_PLACEHOLDER_ID_ADDRESS');
            if ($idPlaceholder > 0) {
                $original = $this->id_address_delivery;
                $this->id_address_delivery = $idPlaceholder;

                // PS 8+ signature wants a Country object or null; we can just pass null.
                $result = parent::getDeliveryOptionList(null, $dontAutoSelectOptions, $use_cache);

                // restore original state
                $this->id_address_delivery = $original;
                return $result;
            }
        }

        return parent::getDeliveryOptionList($default_country, $dontAutoSelectOptions, $use_cache);
    }
}

```


## Rollback

- Delete `/override/classes/Cart.php` and clear cache, or set `DPD_PLACEHOLDER_ID_ADDRESS` to empty:
    
    `DELETE FROM ps_configuration WHERE name='DPD_PLACEHOLDER_ID_ADDRESS';`
    
- (Optional) Disable or remove the placeholder customer/address.