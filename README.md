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
- Sentinel-1 images for each month were downloaded from the Copernicus Open Access Hub, selecting dates closest to the start of each month. XML annotation files containing precise geolocation co-ordinates for each pixel were used to georeference the datat and define the region of interest. VH polaisiation backscatter was processed to detect surface changes related to groundwater variations.

![image alt](https://github.com/DomWeisser/Analysis-of-the-seasonality-impacts-on-Aquifers-in-the-Amazon-using-Sentinel-1-and-GRACE-data/blob/9f00de95f05ca22448960c83c9c252efbaf5a450/Images/monthly_backscatter_roi.png)

Sentinel-1 VH polarisation backscatter is primarily influenced by surface roughness, vegetation structure and moisture content, with ttypical seaonal patterns in the Amazon showing higher values during the dry season (May-October) when exposed vegetation branches and moderate soil drying create optimal scattering conditions, and lower values during the wet season (November-April) when saturated soils and dense foilage reduce radar returns. My graph for 2019 above broadly follows this expected pattern, with backscatter rising through the dry season months and declining toward the wet season onset, but two critical outliers reveal extreme hydrological conditions. The February peak likely reflects extensive river flooding and floodplain inundation that created strong water-surface interactions during peak wet season, while the dramatic decrease in August/September suggests severe drought that exceeded normal dry season thresholds, causing river levels to drop critically and veegtation to die back so extensively that the landscape lost its typical radar scattering propoerties.

### GRACE & GLDAS Data
- The GRACE (Gravity Recvoery and Climate Experiment) satellite data provides measurements of Earth's gravitational field variations, which reflect changes in mass distribution including terrestrial water storage. The GRACE-FO Level-3 mascon (mass concentration) data represents total water storage (TWS) anomalies - the deviations from long-term average conditions - expressed as equivalent water thickness in cm. To isolate groundwater storage changes from the total water storage signal, a water balance approach was employed following the equation:

GWSA = TWS - (SM + SWE  + CWS)

| Acronym | Full Name |
| --- | --- | 
| GWSA | Groundwater Storage anomalies |
| TWS | Total water storage anomalies (GRACE) |
| SM | Soil moisture (GLDAS) |
| SWE | Snow Water Equivalent (GLDAS) | 
| CWS | Canopy Water Storage (GLDAS) |

GRACE-FO data at 0.5* spatial resolution and GLDAS at 0.25* spatial resolution were interpolated to a common grid of 0.01 degrees (WHY) using bilinear interpolation to increase spatial resolution and be closer to Sentinel-1 resolution. 

![image alt](https://github.com/DomWeisser/Analysis-of-the-seasonality-impacts-on-Aquifers-in-the-Amazon-using-Sentinel-1-and-GRACE-data/blob/96a7f93e4fe03898c53e6f7633d760f3eb8fff61/Images/groundwater_timeseries_roi_2019.png)

The graph represents change detection rather than absolute volume calculations, tracking how groundwater storage increases or decreases relative to baseline conditions, where positive values indicate groundwater gain and negative values indicate depletion. The data reveals a pronounced seasonal cycle that closely follows the Amazon's distinct wet and dry seasons. From January to June, groundwater storage levels progressively increase from severe depletion (-0.83 m) to the least depleted state (-0.21 m), with the peak recovery occurring in June. This timing aligns with established hydrogeological principles, as there is typically a one to two-month lag between peak rainfall and groundwater recharge due to infiltration and percolation processes. From June onwards, groundwater levels steadily decrease throughout the dry season, reaching maximum depletion in November (-1.04 m) before showing signs of recovery in December (-0.99 m) as early wet season precipitation begins to influence groundwater levels. This pattern demonstrates the rapid response characteristics of the Alter do Chão Aquifer system, which is consistent with the relatively shallow depth and high permeability of Amazonian aquifers. The observed seasonal amplitude of approximately 0.83 meters between peak recovery and maximum depletion underscores the aquifer's sensitivity to climatic variability and its dependence on consistent seasonal precipitation for sustainable groundwater storage.

## 3.3 Interpolation of Well Data
The in-situ well measyrements provide ground-truth satellite-based groundwater estimates, but their sparse spatial distribution necessitates interpolation to create continous surfaces across the region of interest. 

I implemented Inverse Distance Weighting (IDW) interpolation to spatially extrapolate groundwater level measurements from the 4 RIMAS wells across the whole region of interest at 0.01 degree resolution. IDW applies distance-weighted averages of known data, with closer wells receiving greater influence

$w_i = \frac{1}{(d_i + 0.01)^p}$








# Assumptions 
- Sentinel-1 images not always 1st/last of the month

# Results
The analysis reveals distinct seasonal patterns in groundwater storage with a rough 1-2 month delay. 


# References
[1] - Groundwater dynamics and hydrogeological processes in the Alter do Chão Aquifer: A case study in Manaus, Amazonas – Brazil




