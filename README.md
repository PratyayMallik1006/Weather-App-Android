# Weather-App-Android

<div>
<img src="https://github.com/PratyayMallik1006/Weather-App-Android/blob/main/weather1.jpeg?raw=true" height="600" style="float: left">
<img src="https://github.com/PratyayMallik1006/Weather-App-Android/blob/main/weather2.jpeg?raw=true" height="600" style="float: left">
<img src="https://github.com/PratyayMallik1006/Weather-App-Android/blob/main/weather3.jpeg?raw=true" height="600" style="float: left">
</div>

## Notes
# Permissions
1. AndroidManifest.xml
```xml
<uses-permission android:name="android.permission.INTERNET" />  
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
```
2. MainActivity.java
```java
String LOCATION_PROVIDER = LocationManager.NETWORK_PROVIDER;
LocationManager mLocationManager= (LocationManager) getSystemService(Context.LOCATION_SERVICE);

LocationListener mLocationListener = new LocationListener() {  
    @Override  
  public void onLocationChanged(Location location) {  
  Log.d("Clima", "onLocationChanged");  
  String longitude = String.valueOf(location.getLongitude());  
  String latitude = String.valueOf(location.getLatitude());  
  
  RequestParams params=new RequestParams();  
  params.put("lat",latitude);  
  params.put("lon",longitude);  
  params.put("appid",APP_ID);  
  letsDoSomeNetworking(params);  
  }  
  
    @Override  
  public void onStatusChanged(String s, int i, Bundle bundle) {  
  
    }  
  
    @Override  
  public void onProviderEnabled(String s) {  
  
    }  
  
    @Override  
  public void onProviderDisabled(String s) {  
        Log.d("Clima", "onLocationDisabled");  
  }  
};
```
# API Services:
1. build.gradle(app)
```json
dependencies {
  implementation 'com.codepath.libraries:asynchttpclient:2.1.1'
}
```
2. MainActivity.java
```java
//https://api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&appid={API key}
RequestParams params = new RequestParams();
params.put("lat",latitude);
params.put("lat",latitude);
params.put("appid",API_KEY);

AsyncHttpClient client=new AsyncHttpClient();  
  
client.get(WEATHER_URL,params,new JsonHttpResponseHandler(){  
	    @Override  
	  public void onSuccess(int statusCode, Header[] headers, JSONObject response){  
	        WeatherDataModel weatherData = WeatherDataModel.fromJson(response);  
		  updateUI(weatherData); 
	  } 
	 @Override  
	  public void onFailure(int statusCode,Header[] headers,Throwable e,JSONObject response){  
	        Log.e("Clima","Fail"+e.toString());  
	  Toast.makeText(WeatherController.this,"Request Failed",Toast.LENGTH_SHORT).show();  
	  
	  }  
});
```
# Changing Layouts(screens)
1. AndroidManifest.xml
```xml
<activity android:name=".ChangeCityController">  
</activity>
```
2. MainActivity.java
```java
changeCityButton.setOnClickListener(new View.OnClickListener() {  
	@Override  
	public void onClick(View view) {  
		Intent newIntent = new Intent(WeatherController.this, ChangeCityController.class);  
		startActivity(newIntent);  
	}  
});

@Override  
protected void onResume() {  
	super.onResume();  
	Log.d("Clima", "onResume() called");  
	  
	Intent myIntent = getIntent();  
	String city =myIntent.getStringExtra("City");  
	  
	if(city!=null){  
		getWeatherForNewCity(city);  
	}else{  
		Log.d("Clima", "Getting weather for current location");  
		getWeatherForCurrentLocation();
	}  
}
```
