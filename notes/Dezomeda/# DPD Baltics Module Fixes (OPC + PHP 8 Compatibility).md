
## Overview

These changes fix compatibility issues between the DPD Baltics module and one-page checkout (OPC) modules—especially `ets_onepagecheckout`—and resolve failures occurring under PHP 8.x.

---

## 1. Config Update — Recognize `ets_onepagecheckout` as OPC

**File:** `modules/dpdbaltics/src/Config/Config.php`  
**Line:** 266

### Change

Added `ets_onepagecheckout` to the list of recognized OPC modules.

```php
// Before:
public const DPD_OPC_MODULE_LIST = ['onepagecheckoutps', 'supercheckout', 'thecheckout'];

// After:
public const DPD_OPC_MODULE_LIST = ['onepagecheckoutps', 'supercheckout', 'thecheckout', 'ets_onepagecheckout'];
```

### Why

Ensures DPD treats this module as a one-page checkout and applies correct logic.

---

## 2. Hook Fix — Prevent Crash When Cart Is Missing

**File:** `modules/dpdbaltics/dpdbaltics.php`  
**Line:** 551

### Change

Added fallback to context cart when `$params['cart']` is missing.

```php
// Before:
$cart = $params['cart'];

// After:
// ets_onepagecheckout and some other OPC modules don't pass 'cart' in params
$cart = !empty($params['cart']) ? $params['cart'] : $this->context->cart;
```

### Why

- Some OPC modules don’t pass `cart` in hook params
    
- PHP 8.x throws errors → hook silently fails
    
- Result: PUDO widget doesn’t render
    

---

## 3. Frontend Fix — Pass `id_carrier` in AJAX

**File:** `modules/dpdbaltics/views/js/front/pudo.js`  
**Function:** `selectPickupPointEvent()` (~line 282)

### Change

Explicitly send `id_carrier` in AJAX request.

```js
// Before:
data: {
  'ajax': 1,
  'id_pudo': $pudoId,
  'action': 'savePudoPickupPoint',
  'token': typeof prestashop !== 'undefined' ? prestashop.static_token : ''
},

// After:
data: {
  'ajax': 1,
  'id_pudo': $pudoId,
  'id_carrier': $idReference, // <-- added
  'action': 'savePudoPickupPoint',
  'token': typeof prestashop !== 'undefined' ? prestashop.static_token : ''
},
```

### Why

- OPC modules may not update cart carrier in time
    
- Backend previously received `id_carrier = 0`
    
- Caused incorrect or failed PUDO assignment
    

---

## 4. Backend Fix — Accept Carrier ID + Fallback Logic

### 4a. Pass Carrier ID to Method

**File:** `controllers/front/Ajax.php`  
**Line:** 124

```php
// Before:
$this->savePudoPickupPoint($pudoId, $countryCode);

// After:
$this->savePudoPickupPoint($pudoId, $countryCode, $carrierId);
```

---

### 4b. Update Method Signature + Fallback

**Line:** 265

```php
// After:
private function savePudoPickupPoint($pudoId, $countryCode, $idCarrier = 0)
{
    $pudoService = ...;

    // Use carrier from POST when available
    if (!$idCarrier) {
        $idCarrier = $this->context->cart->id_carrier;
    }

    $addPudoCartOrderStatus = $pudoService->savePudoCartOrder(
        $idCarrier,
        ...
    );
}
```

### Why

Ensures correct carrier is used even if cart isn't updated yet.

---

### 4c. Parcel Shop Flow — Pass Carrier ID

**Line:** 198

```php
// Before:
$this->saveParcelShop($countryCode, $city, $street);

// After:
$this->saveParcelShop($countryCode, $city, $street, $carrierId);
```

---

### 4d. Parcel Shop Method — Add Fallback

**Line:** 364

```php
// After:
private function saveParcelShop($countryCode, $city, $street, $idCarrier = 0)
{
    $cartId = $this->context->cart->id;

    if (!$idCarrier) {
        $idCarrier = $this->context->cart->id_carrier;
    }
}
```

---

## Key Outcomes

- ✅ Fixes PHP 8.x silent failures
    
- ✅ Ensures PUDO widget renders correctly
    
- ✅ Supports `ets_onepagecheckout`
    
- ✅ Prevents incorrect carrier assignment in OPC
    
- ✅ Stabilizes AJAX → backend carrier flow
    

---

## Summary

The core issue was **missing or incorrect carrier context in OPC environments**.  
Fixes introduce:

- Explicit carrier passing (frontend → backend)
    
- Safe fallbacks to cart context
    
- Recognition of additional OPC modules
    

Together, these changes make the DPD module robust across modern checkout implementations.