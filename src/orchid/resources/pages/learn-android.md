---
title: Learn Android
---
Quick snippets of Android-related code and bookmarks to increase productivity.

## Map
There are two ways to implement Google Maps into an Android app: `MapFragment` and `MapView`

Setup:
- `Good` [Google Maps for Android, part 1 (Intro)](https://medium.com/@paultr/google-maps-for-android-pt-1-intro-setup-5f22a1417995): How to implement Google maps from scratch, setup Google Developer Console to enabled the Maps SDK and get the API key
  - Add dependency: `implementation 'com.google.android.gms:play-services-maps:16.1.0'`  
- `Good` [Google Maps for Android, part 2 (User Location)](https://medium.com/@paultr/google-maps-for-android-pt-2-user-location-f7416966aa67): How to access user's location, including setting up permissions

More resources:
- GoogleMap API Reference: https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap

### How to show map marker at street address

```kotlin
import android.location.Geocoder
import com.google.android.gms.maps.model.LatLng
import com.google.android.gms.maps.model.MarkerOptions

...

val markerName = "Example"
val streetAddress = "123 Fake St; Denver, CO 80246"
if (Geocoder.isPresent()) {
    val geocoder = Geocoder(requireContext())
    val addresses = geocoder.getFromLocationName(streetAddress, 1)
    val address = if (addresses == null) null else addresses[0]
    if (address != null) {
        val location = LatLng(address.latitude, address.longitude)
        map?.addMarker(MarkerOptions().position(location).title(markerName))
    }
} else {
    toast("Geocoder is not available on your device")
}
```

## Navigation

## View




## Default sample page

### Syntax Highlighting with Pygments 

```pebble
{% verbatim %}
{% highlight language="java" %}
    public abstract class OrchidGenerator extends Prioritized implements OptionsHolder {
        
        protected final String key;
    
        protected final OrchidContext context;
    
        @Inject
        public OrchidGenerator(OrchidContext context, String key, int priority) {
            super(priority);
            this.key = key;
            this.context = context;
        }
    
        /**
         * A callback to build the index of content this OrchidGenerator intends to create.
         *
         * @return a list of pages that will be built by this generator
         */
        public abstract List<? extends OrchidPage> startIndexing();
    
        /**
         * A callback to begin generating content. The index is fully built and should not be changed at this time. The
         * list of pages returned by `startIndexing` is passed back in as an argument to the method.
         *
         * @param pages the pages to render
         */
        public abstract void startGeneration(Stream<? extends OrchidPage> pages);
    }
{% endhighlight %}
{% endverbatim %}
```

{% highlight language="java" %}
    public abstract class OrchidGenerator extends Prioritized implements OptionsHolder {
        
        protected final String key;
    
        protected final OrchidContext context;
    
        @Inject
        public OrchidGenerator(OrchidContext context, String key, int priority) {
            super(priority);
            this.key = key;
            this.context = context;
        }
    
        /**
         * A callback to build the index of content this OrchidGenerator intends to create.
         *
         * @return a list of pages that will be built by this generator
         */
        public abstract List<? extends OrchidPage> startIndexing();
    
        /**
         * A callback to begin generating content. The index is fully built and should not be changed at this time. The
         * list of pages returned by `startIndexing` is passed back in as an argument to the method.
         *
         * @param pages the pages to render
         */
        public abstract void startGeneration(Stream<? extends OrchidPage> pages);
    }
{% endhighlight %}


***

### Embed Github Gist

```pebble
{% verbatim %}
{% gist user="cjbrooks12" id="83a11f066388c9fe905ee1bab47ecca8" %}
{% endverbatim %}
```

{% gist user="cjbrooks12" id="83a11f066388c9fe905ee1bab47ecca8" %}

***
