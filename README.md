# Groundwater Monitoring and Analysis in the Alter do Chao Aquifer for 2019

# Project Overview/Description
The Amazon region, despite it's abundant surface water, relies heavily on groundwater, with over two-thirds of its urban population depending on this resource for domestic and industrial use. This reliance is largely due to the shallow depth to groundwater, generally good water quality, relatively low treatment costs and the logistical challenges of developing and maintaining surface water infrastrucutre in remote, densely forested areas. The Alter do CHao Aquifer (ACA) is one of Brazil's largest and most strategically important freshwater resources. 

The ACA extends beneath parts of the Amazonas, Para and Amapa states and comprises sedimentary formations with substantial hydrogeological potential. Despite it's significance, the seasonal and long-term fluctuations of the ACA remain poorly understood, especially in relation to climate variablitiy and increasing anthropogenic stressors. The Amazon basin is characterised by it's pronounced climatic seasonlity, which manifests through alternating period of intense rainfall (November-April) and severe drought (May-October). These cycles are becoming more extreme and frequent due to climate change, especially under the influence of interannual climatic drivers such as the El Nino Souther Oscillation (ENSO). Such climatic variability, coupled with land cover modifications and increasing groundwater absorbtion, threatens the sustainability of the aquifer system. Notably, recent multi-year droughts (2005, 2010, 2015 and 2023) have highlighted the vulnerability of groundwater storage to prolonged precipitation deficits. 

Recent studies have documented substantial changes in water storage across the Amazon basin due to both natural climate variability and intensifying land use pressures (Pokhrel et al., 2021; Chen et al., 2022). Multi-year droughts in 2005, 2010, 2015, and 2023 have underscored the vulnerability of the region’s hydrological systems, raising concerns over the resilience of groundwater reserves under prolonged precipitation deficits and increased water demand. Nevertheless, quantifying changes in groundwater storage remains challenging due to limited in-situ monitoring, especially in remote areas like the ACA.
In response to these data limitations, satellite gravimetry missions such as GRACE and its follow-on GRACE-FO have provided a valuable means to infer terrestrial water storage changes at regional to continental scales. When combined with ancillary datasets and hydrological models, GRACE data can be used to isolate groundwater storage anomalies (GWSA) and assess temporal variability in aquifer systems.

This project employs GRACE-FO Level-3 data to estimate groundwater storage changes in the ACA, focusing on a single hydrological year (2019) to provide a temporally constrained yet spatially explicit case study of aquifer behaviour under typical climatic conditions. The methodology uses Python-based geospatial and statistical techniques to preprocess, mask, and analyze GRACE-derived water storage anomalies across the ACA extent. While temporally limited, this analysis offers insight into seasonal groundwater trends and contributes to a growing body of remote-sensing-based assessments of Amazonian hydrology.

 

## 1. Study Area
The study area comprises the Alter do Chão Aquifer (ACA), a significant transboundary aquifer located beneath parts of the Brazilian Amazon, including the states of Amazonas, Pará, and Amapá. The aquifer lies within the sedimentary basins of the Solimões and Amazonas rivers, primarily composed of unconsolidated and semi-consolidated sandstones and siltstones, which confer high porosity and permeability. Given its geographic location in the central and eastern Amazon basin, the ACA is influenced by distinct wet and dry climatic seasons and widespread forest cover, which limits the development of surface hydrological infrastructure. For geospatial analysis, the aquifer extent was defined using a polygon shapefile from ANA (Agência Nacional de Águas e Saneamento Básico), which was projected to EPSG:4326 to align with other datasets.

The Alter do Chao aquifer is located in tge Amazon region of Brazil. It serves as a critical water resource for both human populations and ecological systems in the area. The aquifer is characterised by:
- Predominantly sandy formations
- Strong seasonal precipitation patterns
- Limited ground-based monitoring infrastructure

## 2. Datasets
This study integrates several geospatial and remote sensing datasets:
- GRACE-FO Level-3 Mascon TWS Anomalies (JPL RL06M):
Monthly gravity anomalies were obtained from NASA's Jet Propulsion Laboratory for the year 2019. These data are gridded at 0.5° × 0.5° spatial resolution and provided in NetCDF format. 
- GLDAS-2.1 Noah Land Surface Model:
Monthly reanalysis data providing soil moisture, snow water equivalent and canopy water storage at 0.25° spatial resolution. These components are used to isolate groundwater signals from GRACE total water storage measurements.
- Sentinel-1 SAR Ground Range Detected (GRD):
C-band synthetic aperture radar (SAR) data in VH polariation, processed to Ground Range Detected at 10m spatial resolution. Monthly acquisitions cover the same geographic region throughout 2019, providing consistent temporal sampling of surface backscatter characteristic.
- In-situ Well measurements:
Groundwater level measurements from the RIMAS network operated by the Brazilian Geological Survey. Four wells contain monthly groundwater depth measurements throught 2019 within the study region.

| Dataset | Source | Temporal Resolution | Spatial Resolution | Coverage | Format |
| --- | --- | --- | --- | --- | --- |
| Sentinel-1 SAR | ESA Copernicus | Monthly (2019) | 10m  | Regional ROI | Geo TIFF |
| GRACE-FO | NASA JPL | Monthly (2019)  | 0.5 x 0.5 | Global | NetCDF |
| GLDAS-2.1 Noah | NASA GSFC | Monthly (2019) | 0.25° x 0.25°  | Global | NetCDF |
| Well Measurements | RIMAS (Brazil) | Monthly (2019) | Point-based  | 4 wells in ROI | CSV |

# 3. Methodology & Analysis
## 3.1 Region of Interest
The Region of Interest is equivalent to the spatial extent of the Sentinel-1 images used in this analysis

<table>
  <tr>
    <td><img src="https://github.com/DomWeisser/Analysis-of-the-seasonality-impacts-on-Aquifers-in-the-Amazon-using-Sentinel-1-and-GRACE-data/blob/2e45895f471e3d39f48a10e335ab4ad671138206/roi_coverage_basemap_with_borders_zoom_0.05.png" alt="Monthly mean well depth" width="400"></td>
    <td><img src="https://github.com/DomWeisser/Analysis-of-the-seasonality-impacts-on-Aquifers-in-the-Amazon-using-Sentinel-1-and-GRACE-data/blob/2e45895f471e3d39f48a10e335ab4ad671138206/roi_coverage_basemap_with_borders_zoom_0.90.png" alt="Second image" width="400"></td>
  </tr>
</table>

## 3.2 Data Preprocessing
### In-situ Well Data
- RIMAS dataset came in csv form. Of the 17 wells, 10 had at least one measurement per month in 2019 of which only 4 are contained in my region of interest. This is used as ground truth values. Below is a graph showing the mean monthly well depth values both pure values and normalised values. Changes seen are very small when looking in meters. These graphs demonstrate that well depths in the ROI decrease during the wet season (Nov-Apr) as groundwater levels rise due to recharge, and increase during the dry season (May-Oct) as groundwater levels fall, reflecting the region's distinct hyrdological cycle. This incerse relationship between well depth and groundwater storage occur as well depth is calculated from a baseline from the top of the well.

<table>
  <tr>
    <td><img src="https://github.com/DomWeisser/Analysis-of-the-seasonality-impacts-on-Aquifers-in-the-Amazon-using-Sentinel-1-and-GRACE-data/blob/87ef7b72134a160fe318a030acb7f5dee06e201b/well_depths_monthly.png" width="400"></td>
    <td><img src="https://github.com/DomWeisser/Analysis-of-the-seasonality-impacts-on-Aquifers-in-the-Amazon-using-Sentinel-1-and-GRACE-data/blob/87ef7b72134a160fe318a030acb7f5dee06e201b/well_depths_normalized.png" width="400"></td>
  </tr>
</table>

### Sentinel-1 
Sentinel-1 images for each month were downloaded from the Copernicus Open Access Hub, selecting dates closest to the start of each month. XML annotation files containing precise geolocation co-ordinates for each pixel were used to georeference the datat and define the region of interest. VH polaisiation backscatter was processed to detect surface changes related to groundwater variations.

![image alt](https://github.com/DomWeisser/Analysis-of-the-seasonality-impacts-on-Aquifers-in-the-Amazon-using-Sentinel-1-and-GRACE-data/blob/9f00de95f05ca22448960c83c9c252efbaf5a450/Images/monthly_backscatter_roi.png)

The Sentinel-1 backscatter pattern around Manaus reflects the complex interaction between the extensive river network and seasonal hydrology, where the February peak (117.8) likely corresponds to peak flood season when rivers overflow and create large areas of standing water that produce strong radar returns




### GRACE-FO 
- GRACE data was masked to the ACA extent using the shapefile boundary
- Monthly GRACE solutions were processed to extract Terrestrial Water Storage Anomalies (TWSA)
- Data was reprojected to a consistent coordinate reference system (WGS84) and resampled to 0.01° resolution
- Missing values were filled using a combination of spatial interpolation techniques and Gaussian filtering
### GLDAS
- Four soil moisture layers were extracted and summed to represent total soil moisture content
- Snow water equivalent (SWE) and canopy water storage were also extracted
- All GLDAS components were resampled to match the GRACE grid resolution
- Spatial interpolation was applied to ensure consistent coverage


## 3.2 Groundwater Storage Anomaly Calculation
- GRACE-derived TWSA extraction: Monthly total water storage anomalies were extracted from GRACE-FO data.
- GLDAS component isolation: Non-groundwater components (soil moisture, snow water equivalent, and canopy water) were extracted from GLDAS.
- Groundwater Storage Anomaly (GWSA) calculation: GWSA was calculated by subtracting non-groundwater components from total water storage:
GWSA = TWSA - (Soil Moisture + Snow Water Equivalent + Canopy Water)

- Spatial masking: The resulting GWSA grids were masked to the ACA extent using the aquifer shapefile.
- Unit conversion: All water storage components were converted to a consistent unit (mm water equivalent).

## 3.3 Sentinel-1 Backscatter Analysis
- Monthly backscatter extraction: Mean backscatter values were calculated for each month across the ACA extent.
- Temporal trend analysis: Monthly backscatter values were plotted and analyzed for seasonal patterns.
- Correlation with GWSA: SAR backscatter was statistically compared with GRACE-derived GWSA to evaluate radar sensitivity to groundwater changes.
- Seasonal comparison: Wet and dry season backscatter signatures were compared to identify seasonal differences.

## 3.4 Spatial Interpolation of Well Data
- Data preparation: Well data was filtered for each month and prepared for interpolation.
- IDW interpolation: Inverse Distance Weighting interpolation was applied with a power parameter of 2 and a smoothing factor of 0.01.
- Grid creation: A regular grid with 0.01° resolution was created for interpolation output.
- Boundary masking: Interpolation was constrained to the ACA boundary using the shapefile.
- Smoothing: A light Gaussian filter (sigma=1) was applied to reduce interpolation artifacts.

## 3.5 Constrained Inversion for Aquifer Volume Estimation
- Model parameterization: Aquifer parameters (undrained bulk modulus, Skempton's coefficient, effective porosity) were defined based on literature for the Alter do Chao Formation.
- Parameter optimization: Poroelastic parameters were optimized to minimize the difference between predicted and observed water storage changes.
- Two-layer model: A two-layer model was implemented to distinguish between confined and unconfined portions of the aquifer.
- Volume calculation: Monthly groundwater volume changes were calculated using optimized parameters and satellite observations.
- Spatial distribution: Volume changes were calculated for each grid cell and aggregated to estimate total aquifer volume change.

## 3.6 Seasonal Analysis
- Seasonal definition: Amazon wet season (November-April) and dry season (May-October) were defined for analysis.
- Statistical comparison: Monthly and seasonal statistics were calculated for groundwater storage anomalies.
- Transition analysis: Seasonal transitions (wet to dry and dry to wet) were analyzed for magnitude and timing.
- Visualization: Multiple visualization techniques were employed:

Time series plots of monthly groundwater changes
Spatial maps showing groundwater distribution
Seasonal box plots comparing wet and dry season patterns
Transition arrows showing seasonal shifts
Animations of monthly changes throughout the year

## 3.7 Statistical Analysis
- Monthly statistics: Basic statistical measures (mean, median, standard deviation, min, max) were calculated for each month.
- Spatial variation: Coefficient of variation was calculated to quantify spatial heterogeneity in groundwater storage.
- Percentile analysis: Quartiles (Q1, Q3) and interquartile range were calculated to assess distribution characteristics.
- Area calculations: Areas with positive and negative groundwater storage anomalies were calculated for each month.
- Month-to-month changes: Temporal derivatives were calculated to quantify rate and direction of change.
- Seasonal comparison: Statistical significance tests were performed to compare wet and dry season characteristics.


# Assumptions 
- Sentinel-1 images not always 1st/last of the month

# Results
The analysis reveals distinct seasonal patterns in groundwater storage with a rough 1-2 month delay. 


# References
[1] - Groundwater dynamics and hydrogeological processes in the Alter do Chão Aquifer: A case study in Manaus, Amazonas – Brazil




