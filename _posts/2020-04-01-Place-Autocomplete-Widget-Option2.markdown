---
layout: post
title: Place Autocomplete Widget-Option2
tags: [Android, autocomplete]
---

앱에 지도 검색 기능을 추가하기 위해 Place Autocomplete를 적용.
Option이 2개 있었는데, 그 중에 2번으로 진행. [참고 사이트](https://developers.google.com/places/android-sdk/autocomplete)

### 0. Places API 초기화

* 기본 : `Places.initialize(applicationContext, getString(R.string.google_maps_api_key))`
* 언어설정 : `Places.initialize(applicationContext, getString(R.string.google_maps_api_key), Locale.ENGLISH)`
> 뒤에 원하는 Locale을 지정해주면 결과가 지정해놓은 언어로 내려오지만, 검색을 한글로 하면 가끔 결과를 한글로 주기도 한다.

### 1. 어떤 결과를 받길 원하는지 필드를 지정해주고

{% highlight kotlin %}
val fields = arrayListOf(
                        Place.Field.ID,
                        Place.Field.NAME,
                        Place.Field.ADDRESS,
                        Place.Field.ADDRESS_COMPONENTS
             )
{% endhighlight %}

### 2. 인텐트로 실행해주면 된다. (실행화면은 위 사이트를 통해 확인 가능)

* setCountry에 나라 코드를 넣으면 그 나라에 대한 결과만 보여진다.
{% highlight kotlin %}
val intent = Autocomplete.IntentBuilder(AutocompleteActivityMode.FULLSCREEN, fields)
                        .setCountry(countryCode)
                        .build(this)

startActivityForResult(intent, SELECT_ADDRESS)
{% endhighlight %}

### 3. onActivityResult를 통해 넘어온 데이터를 꺼내서 사용하면 된다. (필드에 지정한 애들만 데이터가 넘어온다.)

{% highlight kotlin %}
@Override
public override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    when {
      resultCode != RESULT_OK -> return
      resultCode == AutocompleteActivity.RESULT_ERROR -> {
      val status = Autocomplete.getStatusFromIntent(data!!)
      return
      }
      resultCode == Activity.RESULT_CANCELED -> return
    }
    
    val result = Autocomplete.getPlaceFromIntent(this)
}
{% endhighlight %}

### 4. 그 외

#### [지정할 수 있는 필드의 종류](https://developers.google.com/places/android-sdk/reference/com/google/android/libraries/places/api/model/Place.Field)

* Place.Field.ADDRESS
* Place.Field.ADDRESS_COMPONENTS
* Place.Field.ID
* Place.Field.LAT_LNG
* Place.Field.NAME
* Place.Field.OPENING_HOURS
* Place.Field.PHONE_NUMBER
* Place.Field.PHOTO_METADATAS
* Place.Field.PLUS_CODE
* Place.Field.PRICE_LEVEL
* Place.Field.RATING
* Place.Field.TYPES
* Place.Field.USER_RATINGS_TOTAL
* Place.Field.VIEWPORT
* Place.Field.UTC_OFFSET
* Place.Field.WEBSITE_URI

#### [IntentBuilder에서 사용할 수 있는 메서드](https://developers.google.com/places/android-sdk/reference/com/google/android/libraries/places/widget/Autocomplete.IntentBuilder)

* setCountry(String country)
* setHint(String hint)
* setInitialQuery(String initialQuery)
* setLocationBias(LocationBias locationBias)
* setLocationRestriction(LocationRestriction locationRestriction)
* setTypeFilter(TypeFilter typeFilter)

#### [Country Code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)