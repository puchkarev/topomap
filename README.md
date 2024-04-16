# topomap
Topographical Maps

This project fetched the bounaries and elevation data and generates stl files for 3d printing of this data.
This project is run from google colab environment.

While the boundary data is free the elevation data api is "almost" free. It is provided by the google maps api,
which gives a credit of $200 / month of free access, and each query costs $0.005 (but check the billing page
for updated info). To get access to this data you need to configure a google project, enable the elevation maps
api and get the maps api key (https://developers.google.com/maps/documentation/elevation/start).
This key needs to be added to the secrets section of colab environment under "maps_api_key".

Once you start using the API you can track the billing here: https://console.cloud.google.com/billing/

Under the borders section indicate which state or country you would like to generate a map for, and a
sampling resolution in meters.

Since we are using a projection (from a sphere to a plane) we need a center point specified as
conversion_longitude_degrees, conversion_latitude_degrees as a point that will have the least distortion.

project_path is a parameter that indicates where you want to store the database and stl files, by default it will
attempt to store them in your google drive.

max_elements_per_elevation_query configures how many points to query per elevation query (there is a limit, and
the limit is not what google says it is in documentation, so here it is a parameter, just in case).

model_x_scale, model_y_scale, model_z_scale - allow to scale the output model, note the original units are in meters,
but most 3d printers interpret units in mm so a 1:1 conversion here is actually a 1:1000 conversion already.

Highly recommend to keep the model_z_value at 5 to 10 times larger than x and y values since the world is flatter
than a pancake.

split_blocks_x and split_blocks_y - indicate into how many blocks you want your print to be broken down into for
easier printing.

