Now that you know how to upload and convert an existing dataset, in the next section you'll learn how to install and use a Google Sheets add-on tool to geocode address data into latitude and longitude coordinates.

# Geocode Addresses in Google Sheets

In this section, you'll learn how to geocode data by installing a free Google Sheets add-on tool. This allows you to geocode addresses directly inside your spreadsheet, which will be very useful when using Leaflet map code templates in Chapter 12.

Geocoding means converting addresses or location names into geographic coordinates (or x and y coordinates) that can be plotted on a map, as shown in Figure 2-3. For example, the Statue of Liberty in the New York City area is located at 40.69, -74.04. The first number is the latitude and the second is the longitude. Since the equator is 0 degrees latitude, positive latitude is the northern hemisphere, and negative latitude is in the southern hemisphere. Similarly, the prime meridian is 0 degrees longitude, which passes through Greenwich, England. So positive longitude is east of the meridian, and negative longitude is west, until you reach the opposite side of the globe, roughly near the International Date Line in the Pacific Ocean.

![](bbox=[0.17, 0.235, 0.9, 0.6])

Figure 2-3. To map addresses, you first need to geocode them.

If you have just one or two addresses, you can quickly geocode them with Google Maps. Search for an address, right-click that point, and select “What’s here?” to reveal a pop-up window with its latitude and longitude, as shown in Figure 2-4.

![](bbox=[0.31, 0.685, 0.76, 0.958])

Figure 2-4. To geocode one address, search in Google Maps and right-click “What’s here?” to show coordinates.