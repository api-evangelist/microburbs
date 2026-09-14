---
name: microburbs-suburb-screening
description: Screen every Australian suburb on real-estate, risk and gentrification metrics using the Suburb Finder -- discover filterable fields, size the result for free, then run the filtered search.
api: Microburbs API
operations:
- get_finder_fields_v1_suburbs_finder_fields_get
- get_finder_count_v1_suburbs_finder_count_get
- get_finder_search_v1_suburbs_finder_search_get
---

# Screen Australian suburbs with the Finder

The Finder is a boolean-AND range filter over suburb metrics. Calls are `GET` with a
bearer key. The `count` step is cheap (5c) and lets you size a query before paying per
returned row in `search`.

1. **Discover the filter columns.** Call `get_finder_fields_v1_suburbs_finder_fields_get`
   (free) to list every field you can filter or sort on, with its label and unit. Build
   your range filters only from these field names.
2. **Size the result for free-ish.** Call `get_finder_count_v1_suburbs_finder_count_get`
   with your candidate filter to learn how many suburbs match WITHOUT paying per row.
   Tighten the ranges if the count is too large.
3. **Run the search.** Call `get_finder_search_v1_suburbs_finder_search_get` with the
   same filter plus a single sort column. Each returned suburb row is metered, so pull
   only as many as you need.

Always run step 2 before step 1's fields are turned into a paid `search`; it is the
documented way to avoid an unexpectedly large per-row bill.
