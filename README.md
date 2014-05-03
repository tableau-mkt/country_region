Country to Region mapping
==============

This is a JSON configuration which allows mapping country codes to regions. It contains two files: a full list of countries to region codes, and a generalized region config that rolls-up standardized regions into more recognisable general world regions. This second config may-or-may-not match your business's rules.

###Country to Region
Example entries from within `country_region.json`
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
Entire contents of `regions_generalized.json`
```json
{
  "regions_generalized" : {
    "0" : {
      "name" : "North America",
      "sub_regions" : "21"
    },
    "1" : {
      "name" : "Europe",
      "sub_regions" : "150,39,151,154,155"
    },
    "2" : {
      "name" : "Asia Pacific",
      "sub_regions" : "142,30,34,35,53,54,57,61,143,145"
    },
    "3" : {
      "name" : "Latin America",
      "sub_regions" : "419,5,13,29"
    },
    "4" : {
      "name" : "Middle East & Africa",
      "sub_regions" : "2,9,11,14,15,17,18"
    }
  }
}
```
