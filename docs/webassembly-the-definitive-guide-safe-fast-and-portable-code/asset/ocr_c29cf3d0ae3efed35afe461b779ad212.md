```
init()
    .then(() => {
        getLocation();
    });

var output = document.getElementById("output");

function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition);
    } else {
        output.innerHTML = "This browser doesn't support Geolocation.";
    }
}

function showPosition(position) {
    output.innerHTML = "Your position:" +
        "<br>Latitude: " + position.coords.latitude +
        "<br>Longitude: " + position.coords.longitude;

    describe_location(position.coords.latitude,
    position.coords.longitude);
}
</script>
<p>Open up your JavaScript console and allow the browser to see your location</p>
<p id="output"></p>
</body>
</html>
```

The results of executing this code are shown in Figure 10-3.