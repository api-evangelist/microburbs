---
name: microburbs-property-valuation
description: Value an Australian property and put the number in context -- AVM point estimate with an 80% range, recent comparable sales, and sale history, from a freeform address.
api: Microburbs API
operations:
- geocode_address_v1_geocode_address_get
- get_property_avm_v1_properties__gnaf_id__valuation_avm_get
- get_property_cma_comp_set_v1_properties__gnaf_id__comparables_cma_comp_set_get
- get_property_sale_history_latest_v1_properties__gnaf_id__sale_history_latest_get
---

# Value an Australian property

All calls are `GET` with `Authorization: Bearer <key>` (use `test` for the sandbox
properties). Every response is `{ "data": ... }`; when data is unavailable the call
still returns HTTP 200 with `data: null` and a `reason` slug -- branch on
`data !== null`, do not treat no-data as an error. Each call bills the cents in
its `X-Cost-Cents` header.

1. **Resolve the address to a G-NAF id.** Call `geocode_address_v1_geocode_address_get`
   with the freeform address string. Take the best candidate's `gnaf_id`
   (e.g. `GANSW704074813`).
2. **Get the AVM.** Call `get_property_avm_v1_properties__gnaf_id__valuation_avm_get`
   with `gnaf_id`. Read `predicted_price` plus `predicted_price_low`/`predicted_price_high`
   (the 80% interval) and the confidence score. If `data` is null, there is no AVM for
   this property -- say so rather than guessing.
3. **Pull comparables.** Call
   `get_property_cma_comp_set_v1_properties__gnaf_id__comparables_cma_comp_set_get`
   to get the comparable sales that triangulate the estimate.
4. **Add the last sale.** Call
   `get_property_sale_history_latest_v1_properties__gnaf_id__sale_history_latest_get`
   for the most recent Sold/For-Sale record to anchor the AVM against a real transaction.

Report the point estimate WITH its range and confidence; never present the midpoint as
a precise price. Microburbs data informs decisions and is not financial advice.
