Country to Region mapping
==============

This is a JSON configuration which allows mapping country codes to regions. It contains two files: a full list of countries to region codes, and a generalized region config that rolls-up standardized regions into more recognisable general world regions. This second config may-or-may-not match your business's rules.

###Country to Region
country_region.json
```json
{
  "countries" : {
    "AD" : {
      "region" : "39"
    },
    "AE" : {
      "region" : "145"
    },
    "AF" : {
      "region" : "34"
    },
  }
}
```

###Country to Region
regions_generalized.json
```json
{
  "regions_generalized" : {
    "1730" : {
      "name" : "North America",
      "sub_regions" : "21"
    },
    "1733" : {
      "name" : "Europe",
      "sub_regions" : "150,39,151,154,155"
    },
  }
}
```
