# topomap

Topographical Maps

This project fetched the bounaries and elevation data and generates stl files for 3d printing of this data.
This project is run from google colab environment.

Example map of california:
* sampled at 500m, 
* reference at lon=-120 lat=36 
* scale x=0.0005 y=0.0005 z=0.005
* segmented into 7x7 chunks
![Screenshot from 2024-04-15 17-40-06](https://github.com/puchkarev/topomap/assets/28606823/7c744d45-266b-485d-a166-4b5216cc0bd0)

With the additional printing in mm and stl units defined in miles the final scale is:
* 2,000,000:1 for x and y
* 200,000:1 for z

Additionally the colab will add a scale legend in the bottom righ (max_x, min_y) corner, the scale marker is
computed automatically, so pay attention to the output of the stl generator to get the size of it,
it will be something like 10^k (1km and 1mile) where k is an integer that is based on the overall
size of your map. For the california map this generated a scale of 10 km and 10 miles.

![Screenshot from 2024-04-15 17-41-57](https://github.com/puchkarev/topomap/assets/28606823/cfc987be-af3f-4a0a-91e8-8b30e8a2b055)

## Setup

The only thing you need out of this repo to run this is the colab Topologica_Map_Generator.ipynb everything
else is either instructions, license or the resulting output files, and you can open the colab directly from
this repository by clicking the Open in Colab at the top of that file.

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

## Cost

The sampling of california at 500m scale resulted in 1,687,755 samples with 450 queries / sample = 3,751 queries
3,751 queries * 0.005 $/q = $18.75 for the elevation map data, which is well below the $200 monthly free credit
from the maps team. Thank you google maps team.

When printing at with scale factors(x=0.0005 y=0.0005 z=0.005) with extra meters to mm conversion and broken
into 7x7 chunks and printing with a raft (to avoid warping). The biggest chunk consumed (74.34g of PLA) and
took 6.5 hours to print. Note that most blocks were smaller and took about 35g of PLA and 4 hours to print.
Also note that some blocks were empty and I deleted them, but you may want a complete base.

So at ~35g/block * 49 blocks = 1,715g of PLA. 1kg of PLA costs about $13 so $22.295 of PLA cost.
