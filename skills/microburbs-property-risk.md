---
name: microburbs-property-risk
description: Assess environmental and neighbourhood risk for an Australian property -- bushfire, flood, heritage, landslide and more -- plus its suburb-level risk backdrop, in as few metered calls as possible.
api: Microburbs API
operations:
- geocode_address_v1_geocode_address_get
- get_property_profile_v1_properties__gnaf_id__profile_get
- get_property_risks_all_v1_properties__gnaf_id__risks_all_get
- get_suburb_risks_all_v1_suburbs__suburb_name__risks_all_get
---

# Assess property risk

Calls are `GET` with a bearer key. Prefer the bundled `risks/all` endpoint: it returns
every property-level risk overlay in one flat-priced call instead of paying for each
overlay separately.

1. **Resolve the address.** Call `geocode_address_v1_geocode_address_get` to get the
   `gnaf_id`.
2. **Get the property's ABS geography** (for the suburb name you'll need in step 4). Call
   `get_property_profile_v1_properties__gnaf_id__profile_get`; read the resolved SAL
   suburb name.
3. **Pull all property risks in one call.** Call
   `get_property_risks_all_v1_properties__gnaf_id__risks_all_get`. For each overlay read
   `on_property` -- a true value means the hazard intersects the parcel itself, not just
   the 1 km surroundings.
4. **Add the suburb backdrop.** Call
   `get_suburb_risks_all_v1_suburbs__suburb_name__risks_all_get` with the SAL from step 2
   to report the share of the suburb area under bushfire/flood designation.

Distinguish "on this property" from "nearby in the suburb" in your summary; conflating
them overstates the risk.
