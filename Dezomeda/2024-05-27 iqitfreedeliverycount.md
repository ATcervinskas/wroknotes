Already done until this date 

Installed iqitfreedeliverycount module to show left price until free shipping

BackOffice configuration. 
iqitfreedeliverycount module translations: 
	 cart/spend/ = Iki nemokamo pristatymo liko:
	 product/spend = Iki nemokamo pristatymo liko:
iqitfreedeliverycount settings
		Text color - 232323 hex
		Background color - rgba(255,154,82,.3)
		Inner border color - ff9a52 hex 

- In product page moved "Pristatysime per 1–4 darbo dienas." to the add to cart container, also moved and change iqitfreedeliverycount  module style, added cart icon svg.
- Set main shipping to free (Pristatymas to dezomeda store)
- Inside product modal hidden shipping price and added "Norint patvirtinti užsakymą reikia, kad prekių krepšelio suma būtų bent 15,00 € (be PVM). Šiuo metu prekių krepšelio suma yra 2,50 € (be PVM)."
- Inside cart moved iqitfreedeliverycount text near  alert "Norint patvirtinti užsakymą reikia".
- Created copy of drekinamasis veido kremas and added 100% discout also hidden category has been created to store cloned products there. option inside product selected to not display everywhere.
- For post machine delivery added max weight. 
- Hidden shipping price inside checkout page

**Created new branch V3-Task-19****
Increase upload size in apache php.ini (localy)
**Navigate to side menu > Advanced Parameters > Administration > Upload Quota.**