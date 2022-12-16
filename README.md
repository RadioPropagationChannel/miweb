# Radio Propagation Channel 
Radio Propagation Channel provides provides a MATLAB&reg; implementation of the ITU-R P. recommendation and other methods to characterize and modell the radio propagation channel.

## Documentation
The documentation resides in [docs](./docs) folder.

Examples of use cases can be found in the [examples](./examples) folder2.

## Get Started
This repository requires [MATLAB](https://www.mathworks.com/products/matlab.html) (R2018b and above).

The files are organized in the following folders:
* [itr_r_p_series](./itu_r_p_series): ITU-R P. recomendation implementation.
** [data] (./itu_r_p_series/data): Recomendation data files.
** [examples](./itu_r_p_series/erxamples): ITU-R P. implementation examples.
* [Others](./others): Others ....

## Usage
The following code example shows the usage of ITU-Rpy. More examples can be found in the examples folder.

```matlab:Code(Display)
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% Usage of ITU-R-P Series 
% Computing the gaseous attenuation statistics for slant paths at a single 
% location.
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% Recommendation ITU-R P 676-12 :  Attenuation by atmospheric gases
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%
% ITU-Rpy Quick Start example converted from python by: 
%     Fernando Machado, Fernando P. Fontan, Vicente Pastoriza-Santos
% Institution: atlanTTIC, University of Vigo
% Contact: fpfontan@tsc.uvigo.es, fmachado@uvigo.es, vpastoriza@uvigo.es
% 
% References:
%    [1] Inigo del Portillo, "ITU-Rpy: A python implementation of the 
%        ITU-R P. Recommendations to compute atmospheric attenuation in 
%        slant and horizontal path", 
%        https://github.com/inigodelportillo/ITU-Rpy/
%
%    [2] Recommendation ITU-R P.676-12.
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

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

%% Define unavailabilities vector
% The method to compute the total atmospheric attenuation in 
% Rec. ITU-P 618-13 is only recommended for unavailabilities (p) 
% between 0.001% and 50%
%unavailabilities = logspace(-1.5, 1.5, 100);
unavailabilities = logspace(-1, 1.7, 10);

%% Compute the attenuation components for the unavailabilities
Ag  = zeros(1,length(unavailabilities));
VT  = zeros(1,length(unavailabilities));
RHO = zeros(1,length(unavailabilities));
%, A_c, A_r, A_s, A_t = [], [], [], [], []
for i = 1:length(unavailabilities)
    % This takes account of the fact that a large part of the cloud attenuation
    % and gaseous attenuation is already included in the rain attenuation
    % prediction for time percentages below 1%. Eq. 64 and Eq. 65 in
    % Recommendation ITU 618-12
    p = unavailabilities(i);
    p_c_g = max(0, p);

    %% Estimate the total columnar water vapour content
    V_t = total_water_vapour_content(lat, lon, p_c_g, hs);
    VT(i) = V_t;

    %% Estimate the surface water vapour density
    rho = surface_water_vapour_density(lat, lon, p_c_g, hs);
    RHO(i) = rho;
    
    %%  Compute the atmospheric attenuation
    %Ar(i) = rain_attenuation(lat, lon, f, el, hs, p, R001, tau, Ls)
    tic;
    Ag(i)  = gaseous_attenuation_slant_path(f, el, rho, P, T, V_t, hs, 'approx');
    toc
    %Ac(i) = cloud_attenuation(lat, lon, el, f, p_c_g)
    %As(i) = scintillation_attenuation(lat, lon, f, el, p, D, eta, T, H, P, hL)
    %A(i)  = Ag(i) + np.sqrt((Ar(i) + Ac(i)) ** 2 + As(i) ** 2)
end %for

% Plot the results
figure(10)
clf
semilogx(unavailabilities, Ag, 'b')
hold on
%semilogy(fs, att_dry, 'r')

xlabel('Percentage of time attenuation value is exceeded [%]')
ylabel('Attenuation [dB]')
legend({'Gaseous attenuation'})%,'Dry atmosphere'},'Location','northwest')

grid on
title({'Slant path attenuation exceeded the [%] of the time', ...
       sprintf('Location: %f ºN, %f ºE',lat,lon)})
```

## References
[1] Inigo del Portillo, "ITU-Rpy: A python implementation of the ITU-R P. Recommendations to compute atmospheric attenuation in slant and horizontal path", 
 [https://github.com/inigodelportillo/ITU-Rpy/](https://github.com/inigodelportillo/ITU-Rpy/).
