# Radio Propagation Channel 
Radio Propagation Channel provides provides a MATLAB&reg; implementation of the ITU-R P. recommendation and other methods to characterize and modell the radio propagation channel.

## Documentation
The documentation resides in [docs](./docs) folder.

Examples of use cases can be found in the [examples](./examples) folder2.

## Get Started
This repository requires [MATLAB](https://www.mathworks.com/products/matlab.html) (R2018b and above).

The files are organized in the following folders:
* [itr_r_p_series](./itu_r_p_series): ITU-R P. recomendation implementation.
    * [data] (./itu_r_p_series/data): Recomendation data files.
    * [examples](./itu_r_p_series/erxamples): ITU-R P. implementation examples.
* [Others](./others): Others ....

## Usage
The following code example shows the usage of ITU-Rpy. More examples can be found in the examples folder.

```matlab:Code(Display)

%% Ground station coordinates (Boston)
lat = 42.3601;  % ºN
lon = -71.0942; % ºE

%% Satellite coordinates (GEO, 77 W)
lat_sat = 0;    % ºN
lon_sat = -77;  % ºE
h_sat = 35786;  % km

%% Compute the elevation angle between satellite and ground station
el = elevation_angle(h_sat, lat_sat, lon_sat, lat, lon);

%% Set the link parameters
f = 22.5; % Link frequency (GHz)
D = 1.2;  % Antenna diameters (m)

%% Estimate the ground station altitude
hs = topographic_altitude(lat, lon);

%% Surface mean temperature
T = surface_mean_temperature(lat, lon);

%% Estimate the surface Pressure
P = standard_pressure(hs);
```

## References
[1] Inigo del Portillo, "ITU-Rpy: A python implementation of the ITU-R P. Recommendations to compute atmospheric attenuation in slant and horizontal path", 
 [https://github.com/inigodelportillo/ITU-Rpy/](https://github.com/inigodelportillo/ITU-Rpy/).
