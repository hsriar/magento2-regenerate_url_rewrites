# magento2-regenerate_url_rewrites
Magento 2 Custom "Regenerate Url rewrites" extension by hsriar which add a CLI feature which allow to regenerate a Url rewrites of products/categories in all stores or specific store.
Extension homepage: https://github.com/hsriar/magento2-regenerate_url_rewrites.git

## CONTACTS
* Email: hsriar.work@gmail.com
* Instagram: https://www.instagram.com/hsriar.official/
* Facebook: https://www.facebook.com/hsriar.official

## INSTALLATION

### COMPOSER INSTALLATION
* run composer command:
>`$> composer require hsriar/magento2-regenerate-url-rewrites`

### MANUAL INSTALLATION
* extract files from an archive

* deploy files into Magento2 folder `app/code/`

### ENABLE EXTENSION
* enable extension (use Magento 2 command line interface \*):
>`$> php bin/magento module:enable HsRiar_RegenerateUrlRewrites`

* to make sure that the enabled module is properly registered, run 'setup:upgrade':
>`$> php bin/magento setup:upgrade`

* [if needed] re-deploy static view files:
>`$> php bin/magento setup:static-content:deploy`


## HOW TO USE IT:
* to regenerate URL rewrites of all products in all stores (only products) set entity type to "product":
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=product`

because `product` entity type is default - you can skip it:
>`$> php bin/magento ok:urlrewrites:regenerate`

* to regenerate URL rewrites of all categories\*\* in all stores set entity type to "category":
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=category`

\*\* take into account that this is a heavy process, see section "INFO".

* to regenerate URL rewrites in the specific store view (e.g.: store view id is "3") use option `--store-id`:
>`$> php bin/magento ok:urlrewrites:regenerate --store-id=3`

* to regenerate URL rewrites of specific product use option `product-id` (e.g.: product ID is "199"):
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=product --product-id=199`

or
>`$> php bin/magento ok:urlrewrites:regenerate --product-id=199`

* to regenerate URL rewrites of specific products range use option `products-range` (e.g.: regenerate for all products with ID between "161" and "182"):
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=product --products-range=161-182`

\* if in the range you have a gap of ID's (in range 111-155 products with ID's 111, 122, 155 not exists) - do not worry, script handle this.

or
>`$> php bin/magento ok:urlrewrites:regenerate --products-range=111-155`

* to regenerate URL rewrites of specific category use option `category-id` (e.g.: category ID is "13"):
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=category --category-id=13`

* to regenerate URL rewrites of specific categories range use option `categories-range` (e.g.: regenerate for all categories with ID between "6" and "15"):
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=category --categories-range=6-15`

\* if in the range you have a gap of ID's (in range 6-15 category with ID "8" not exists) - do not worry, script handle this.

* to skip re-generating a products URL rewrites in category if config option "Use Categories Path for Product URLs" is "No":
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=category --check-use-category-in-product-url`

* to save a current URL rewrites (you want to get a new URL rewites and save current) use option `--save-old-urls`:
>`$> php bin/magento ok:urlrewrites:regenerate --save-old-urls`

* to prevent regeneration of "url_key" values (use current "url_key" values) use option `--no-regen-url-key`:
>`$> php bin/magento ok:urlrewrites:regenerate --no-regen-url-key`

* do not run full reindex at the end of URL rewrites generation use option `--no-reindex`:
>`$> php bin/magento ok:urlrewrites:regenerate --no-reindex`

* do not run cache:clean at the end of URL rewrites generation use option `--no-cache-clean`:
>`$> php bin/magento ok:urlrewrites:regenerate --no-cache-clean`

* do not run cache:flush at the end of URL rewrites generation use option `--no-cache-flush`:
>`$> php bin/magento ok:urlrewrites:regenerate --no-cache-flush`

* do not display a progress dots in the console (usefull for a stores with a big number of products):
>`$> php bin/magento ok:urlrewrites:regenerate --no-progress`

### YOU CAN COMBINE OPTIONS
>`$> php bin/magento ok:urlrewrites:regenerate --store-id=3 --save-old-urls --no-regen-url-key --no-reindex`

### YOU CAN NOT COMBINE THIS OPTIONS TOGETHER
* `--entity-type=product` and `--category-id`/`--categories-range`
* `--entity-type=category` and `--product-id`/`--products-range`
* `--category-id` and/or `--categories-range` and/or `--product-id` and/or `--products-range`

### EXAMPLES
* Regenerate URL rewrites for product with ID "56" in store with ID "3":
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=product --store-id=3 --product-id=56`

or
>`$> php bin/magento ok:urlrewrites:regenerate --store-id=3 --product-id=56`

* Regenerate URL rewrites for products with ID's 4,5,6,7,8,9,10 in store with ID "3" and do not run full reindex at the end of process:
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=product --store-id=3 --products-range=4-10 --no-reindex`

* Regenerate URL rewrites for category with ID "48" in all stores and save current URL rewrites:
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=category --category-id=48 --save-old-urls`

* Regenerate URL rewrites for categories with ID's 15,16,17,18,19 in store with ID "3":
>`$> php bin/magento ok:urlrewrites:regenerate --entity-type=category --categories-range=15-19 --store-id=3`

## INFO
When you regenerate URL rewrites of some category then you regenerate URL rewrites of all products from this category AND URL rewrites of all subcategories (all levels - Magento use recursive logic) AND all products from this subcategories. This is the built-in Magento logic. So, take into account that regenerating of URL rewrites of any category (specially from top level) is a "heavy" process.

## HOW TO USE DEBUG INFORMATION:
If you see in the console log a message(-s) like this:
>`URL key for specified store already exists. Product ID: 1680. Request path: modelautos/schaal/revell-honda-nsx-1990-grijs-1-18.html`

or

>`URL key for specified store already exists. Category ID: 359. Request path: modelautos/automerk/filmauto.html`

Then you can find a product (or category) by provided ID and copy product (or category) name. After that you can search in the store for the product (or category) with same name and resolve conflict by updating/changing name of one of the products (or categories).

Enjoy!

Best regards,
Hs Riar

-------------

* For More Details check the link  http://devdocs.magento.com/guides/v2.0/config-guide/cli/config-cli-subcommands.html
