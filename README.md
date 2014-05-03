Country to region mapping
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

###Generalized Regions
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

## Usage Example
This example uses a deferred jQuery object in the global scope to keep track of whether the mapping object has been built out. It then uses the mapping to apply raw country user data against some DOM objects. This use-case was modified from an example of setting a client-side filter. _NOTE: dependecy on jQuery and Underscore.js_

```javascript
(function($) {
  $(document).ready(function() {

    if ($('form').hasClass('my-reaction-class')) {
      // Build mappers for later use.
      SomeGlobalScopeObject.buildMapping();
      if (typeof(UserDataObject) === 'undefined') {
        UserDataObject = collectUserDataSomehow().done(function() {
          if (typeof(UserDataObject) !== 'undefined') {
            trigger_thing();
          }
        });
      }
      else {
        trigger_thing();
      }
    }

    function trigger_thing() {
      $.each($('.things-to-act-on'), function(index, $val) {
        UseMappingSomehow($val, index);
      });
    }

  });

  SomeGlobalScopeObject.buildMapping = function() {
    SomeGlobalScopeObject["mapping_deferred"] = $.Deferred();
    // Build mappings if not already built.
    if (!_.has(SomeGlobalScopeObject, 'data_mapping')) {
      var json_location = '/your/library/path/';
      var mapping_holder = {};

      var result_regions = $.getJSON(json_location + 'country_region.json').done(function(data) {
        mapping_holder["countries"] = data.countries;
      });
      var result_general = $.getJSON(json_location + 'regions_generalized.json').done(function(data) {
        mapping_holder["regions_generalized"] = data.regions_generalized;
      });
      $.when(result_regions, result_general).done(function() {
        SomeGlobalScopeObject.data_mapping = mapping_holder;
        SomeGlobalScopeObject.mapping_deferred.resolve();
      });
    }
    else {
      SomeGlobalScopeObject.mapping_deferred.resolve();
    }
  };

})(jQuery);

UseMappingSomehow = function($whereToUseItSelector) {
  // Ensure the mapping was built.
  if (!_.has(SomeGlobalScopeObject, "mapping_deferred")) {
    SomeGlobalScopeObject.buildMapping();
  }
  SomeGlobalScopeObject.mapping_deferred.done(function() {
    var data = false;

    // Get the mapper for region to country.
    if (typeof UserDataObject.country !== 'undefined') {
      if (_.has(SomeGlobalScopeObject.data_mapping.countries, UserDataObject.country)) {
        var country_data = SomeGlobalScopeObject.data_mapping.countries[country_code];

        _.each(SomeGlobalScopeObject.data_mapping.regions_generalized, function(e, i) {
          if (_.indexOf(e.sub_regions.split(','), country_data.region) !== -1) {
            data = i;
          }
        });
      }
    }

    if (data && typeof data !== "undefined") {
      DoSomethingToSomething($whereToUseItSelector, data);
    }
  });
};
```
