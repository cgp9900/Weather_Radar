# Weather Radar Project

The Weather Radar Project utilizes sequential radar images and their associated radar key to convert the looped radar into a singular, aggregate, rainfall accumulation map. Screenshots are input of each radar step (1 through 6 in order, and one empty map with no radar displayed), and the code maps and aggregates these to a rainfall accumulation map. Several methods are employed to accomplish this goal, including interpolating and exponential regression, image vectorization, color conversions, array mapping, and complex visualizaton. 

## Background Information
The data is gathered via the [RadarScope](https://www.radarscope.app/take-a-tour/) app, a robust mobile weather application with robust and detailed radar capabalities. The application displays loops of radar imagery and an associated key. The colorscale and rainfall amounts of the radar and key are based on dBZ values. This stands for decibels of reflectivity, and acts as a logarithmic, dimensionless unit used across most weather radars. Below is from the [NOAA](https://www.noaa.gov/jetstream/reflectivity) and details the mapping from dBZ to rain rates. 

| dBZ | Rain Rate (in/hr) | Rain Rate (mm/hr) |
|:---:|:-----------------:|:-----------------:|
|  65 |        16+        |        420+       |
|  60 |        8.00       |        205        |
|  55 |        4.00       |        100        |
|  50 |        1.90       |         47        |
|  45 |        0.92       |         24        |
|  40 |        0.45       |         12        |
|  35 |        0.22       |         6         |
|  30 |        0.10       |         3         |
|  25 |        0.05       |         1         |
|  20 |        0.01       |       Trace       |
| <15 |      No rain      |      No rain      |

This mapping is the basis of the project, and in order to make this mapping, one first had to be made between RGB values and dBZ values, a difficult task which involved several manipulation steps. 

## Step by Step Project Explanation: 

1. Required Imports
    
     Importing all necessary modules - includes pandas, numpy, imageio, pyglet, and more. 

2. Displaying the Radar Images as GIF
     
     Reading in the six radar images, saving as a gif with pyglet, and displaying the gif 

3. dBZ to Rain Rate Mapping

     A dataframe is initialized with the dBZ to rain rate mapping displayed above. To get a more granular view of this mapping, interpolation and exponential regression is applied. 
     
     A model is first fit with the log of y (the rain rates), the exponential is applied to the model as an inverse to isolate y, a series of nulls twice the size of the dBZ to rain rate mapping dataframe is instantiated, the known dBZ and rain rate values are inserted in every other position, the null values are interpolated, and the process repeats. This loop is done until there are 1000 points in the dBZ to rain rate mapping.

3. Reading and Formatting the Radar key

     The radar key is the gradient of black to blue to green to red that represents the scale of dBZ values. The key is read in, a slice is taken and unique color values are extracted so there are no repeats. First, the rain rate and dBZ arrays are reshaped to match that of the radar key. Then, we create an array of all possible BGR values and assign the known BGR values from the key and their associated rain rates to the array. All remaining BGR values are interpolated so that all possible BGR values have an associated dBZ and rain rate. 

4. Converting Radar Images to Totals

     Each image is re-read and the base map (consisting of just the county lines and black background from the empty radar image) is subtracted from each. The images are resized to account for the header and footer of the radar images, and then mapped to aggregate rainfall amounts using the array created in the previous step. Once each image has been mapped, they are all added and the final array is adjusted to account for the fact that each radar image is about ~5 to ~6 minutes of time and the rainfall rates used are for hourly rates. 

5. Visualizing Result

     The final result is displayed using matplotlib, and a colorbar is added to show the total amounts in inches. 

     
## Usage

The bulk of the project is within an .ipynb file to provide insight into particular cell results and visualizations. Directory: Weather_Radar/radar/Map_Detection.ipynb.

The screenshotted, sequential radar images along with the empty radar image and the result of the pyglet gif are also placed in the radar folder. 

To get started, first clone the repository. This will download a copy of the repository in your current working directory. 

```python
$ git clone https://github.com/cgp9900/Weather_Prediction.git
```
After this, navigate to the .ipynb file and run the code.
