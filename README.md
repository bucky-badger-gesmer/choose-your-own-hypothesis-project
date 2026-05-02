# Detection Method and Insolation Flux in Exoplanets

## Project Scaffold

| Element                               | Your Plan                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Topic / Question**                  | Does discovery method influence the insolation flux of detected planets? If so, are our catalogs of potentially Earth-like worlds shaped by search technique rather than true planetary distribution?                                                                                                                                                                                                                                                                     |
| **Hypothesis**                        | **H₀:** Discovery method has no influence on insolation flux. The mean (or median) insolation flux of planets found by the Transit method is equal to that of planets found by the Radial Velocity method. Any observed difference is due to random chance.<br><br>**H₁:** Discovery method does influence insolation flux. The mean (or median) insolation flux of planets found by the Transit method differs from that of planets found by the Radial Velocity method. |
| **Outcome / Metric / Test Statistic** | `pl_insol` (Insolation Flux [Earth Flux]); difference in means (or medians) between Transit and Radial Velocity groups                                                                                                                                                                                                                                                                                                                                                    |
| **Units of Analysis**                 | Individual exoplanet entries from the NASA Exoplanet Archive                                                                                                                                                                                                                                                                                                                                                                                                              |
| **Data Source(s)**                    | [NASA Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=PS) — PS_2026.04.26_08.19.28.csv                                                                                                                                                                                                                                                                                                                        |
| **Why this data works**               | The dataset contains 39,803 exoplanet entries with discovery method, insolation flux, and equilibrium temperature — enough observations for both Transit (35,713) and Radial Velocity (2,885) groups to support resampling-based inference                                                                                                                                                                                                                                |
| **Uncertainty Metric**                | Bootstrap confidence interval for the difference in mean/median insolation flux between the two discovery methods                                                                                                                                                                                                                                                                                                                                                         |
| **Null Hypothesis**                   | The type of detection technique astronomers use does not change the amount of starlight received by the planets they find. Any difference in insolation flux between Transit and Radial Velocity planets is due to random chance.                                                                                                                                                                                                                                         |

## 1. Research Question

What are you investigating, and why does it matter?

## 2. Hypothesis

State your null and alternative hypotheses clearly and succinctly.

## 3. Data Description

We use the [NASA Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=PS) Planetary Systems dataset (`PS_2026.04.26_08.19.28.csv`). Each observation is a single exoplanet entry. The full dataset contains 39,803 rows and 92 columns. For this analysis we filter to the Transit and Radial Velocity discovery methods and drop rows with missing `pl_insol` values.

## 4. Methods

Summarize how you analyzed the data:

- The test statistic for your permutation test
- How you simulated or resampled under the null hypothesis
- The metric(s) for which you created bootstrap confidence intervals
- Why the CLT does not apply to at least one metric

## 5. Results

Present your main findings:

- Key summary statistics and visualizations
- Observed test statistic and p-value (if applicable)
- Bootstrap confidence intervals for relevant metrics

## 6. Uncertainty Estimation

Discuss your resampling results:

- How many resamples you used
- What the bootstrap or randomization distributions looked like
- How you interpret the interval estimates

## 7. Limitations

Briefly note any limitations in data, assumptions, or methods, including sources of bias or missing data.

## 8. References

List all datasets, tools, libraries, or papers you cited.

---

## Appendix: Data Dictionary

Below is a guide to the columns in the NASA Exoplanet Archive dataset.

#### Planet Identification & Status

| Column            | Description                   |
| ----------------- | ----------------------------- |
| `pl_name`         | Planet Name                   |
| `hostname`        | Host Name (star system)       |
| `pl_letter`       | Planet Letter                 |
| `hd_name`         | HD ID                         |
| `hip_name`        | HIP ID                        |
| `tic_id`          | TIC ID                        |
| `gaia_dr2_id`     | Gaia DR2 ID                   |
| `gaia_dr3_id`     | Gaia DR3 ID                   |
| `default_flag`    | Default Parameter Set         |
| `sy_snum`         | Number of Stars               |
| `sy_pnum`         | Number of Planets             |
| `sy_mnum`         | Number of Moons               |
| `cb_flag`         | Circumbinary Flag             |
| `soltype`         | Solution Type                 |
| `pl_controv_flag` | Controversial Flag            |
| `pl_refname`      | Planetary Parameter Reference |

#### Discovery & Detection

| Column            | Description                                |
| ----------------- | ------------------------------------------ |
| `discoverymethod` | Discovery Method                           |
| `disc_year`       | Discovery Year                             |
| `disc_refname`    | Discovery Reference                        |
| `disc_pubdate`    | Discovery Publication Date                 |
| `disc_locale`     | Discovery Locale                           |
| `disc_facility`   | Discovery Facility                         |
| `disc_telescope`  | Discovery Telescope                        |
| `disc_instrument` | Discovery Instrument                       |
| `rv_flag`         | Detected by Radial Velocity Variations     |
| `pul_flag`        | Detected by Pulsar Timing Variations       |
| `ptv_flag`        | Detected by Pulsation Timing Variations    |
| `tran_flag`       | Detected by Transits                       |
| `ast_flag`        | Detected by Astrometric Variations         |
| `obm_flag`        | Detected by Orbital Brightness Modulations |
| `micro_flag`      | Detected by Microlensing                   |
| `etv_flag`        | Detected by Eclipse Timing Variations      |
| `ima_flag`        | Detected by Imaging                        |
| `dkin_flag`       | Detected by Disk Kinematics                |

#### Orbital Parameters

| Column            | Description                             |
| ----------------- | --------------------------------------- |
| `pl_orbper`       | Orbital Period [days]                   |
| `pl_orbpererr1`   | Orbital Period Upper Unc. [days]        |
| `pl_orbpererr2`   | Orbital Period Lower Unc. [days]        |
| `pl_orbperlim`    | Orbital Period Limit Flag               |
| `pl_orbsmax`      | Orbit Semi-Major Axis [au]              |
| `pl_orbsmaxerr1`  | Semi-Major Axis Upper Unc. [au]         |
| `pl_orbsmaxerr2`  | Semi-Major Axis Lower Unc. [au]         |
| `pl_orbsmaxlim`   | Semi-Major Axis Limit Flag              |
| `pl_orbeccen`     | Eccentricity                            |
| `pl_orbeccenerr1` | Eccentricity Upper Unc.                 |
| `pl_orbeccenerr2` | Eccentricity Lower Unc.                 |
| `pl_orbeccenlim`  | Eccentricity Limit Flag                 |
| `pl_orbincl`      | Inclination [deg]                       |
| `pl_orbinclerr1`  | Inclination Upper Unc. [deg]            |
| `pl_orbinclerr2`  | Inclination Lower Unc. [deg]            |
| `pl_orbincllim`   | Inclination Limit Flag                  |
| `pl_orbtper`      | Epoch of Periastron [days]              |
| `pl_orbtpererr1`  | Epoch of Periastron Upper Unc. [days]   |
| `pl_orbtpererr2`  | Epoch of Periastron Lower Unc. [days]   |
| `pl_orbtperlim`   | Epoch of Periastron Limit Flag          |
| `pl_orblper`      | Argument of Periastron [deg]            |
| `pl_orblpererr1`  | Argument of Periastron Upper Unc. [deg] |
| `pl_orblpererr2`  | Argument of Periastron Lower Unc. [deg] |
| `pl_orblperlim`   | Argument of Periastron Limit Flag       |

#### Planet Physical Properties

| Column          | Description                                           |
| --------------- | ----------------------------------------------------- |
| `pl_rade`       | Planet Radius [Earth Radius]                          |
| `pl_radeerr1`   | Radius Upper Unc. [Earth Radius]                      |
| `pl_radeerr2`   | Radius Lower Unc. [Earth Radius]                      |
| `pl_radelim`    | Radius Limit Flag                                     |
| `pl_radj`       | Planet Radius [Jupiter Radius]                        |
| `pl_radjerr1`   | Radius Upper Unc. [Jupiter Radius]                    |
| `pl_radjerr2`   | Radius Lower Unc. [Jupiter Radius]                    |
| `pl_radjlim`    | Radius Limit Flag                                     |
| `pl_masse`      | Planet Mass [Earth Mass]                              |
| `pl_masseerr1`  | Planet Mass Upper Unc. [Earth Mass]                   |
| `pl_masseerr2`  | Planet Mass Lower Unc. [Earth Mass]                   |
| `pl_masselim`   | Planet Mass Limit Flag                                |
| `pl_massj`      | Planet Mass [Jupiter Mass]                            |
| `pl_massjerr1`  | Planet Mass Upper Unc. [Jupiter Mass]                 |
| `pl_massjerr2`  | Planet Mass Lower Unc. [Jupiter Mass]                 |
| `pl_massjlim`   | Planet Mass Limit Flag                                |
| `pl_msinie`     | Planet Mass\*sin(i) [Earth Mass]                      |
| `pl_msinieerr1` | Planet Mass\*sin(i) Upper Unc. [Earth Mass]           |
| `pl_msinieerr2` | Planet Mass\*sin(i) Lower Unc. [Earth Mass]           |
| `pl_msinielim`  | Planet Mass\*sin(i) Limit Flag                        |
| `pl_msinij`     | Planet Mass\*sin(i) [Jupiter Mass]                    |
| `pl_msinijerr1` | Planet Mass\*sin(i) Upper Unc. [Jupiter Mass]         |
| `pl_msinijerr2` | Planet Mass\*sin(i) Lower Unc. [Jupiter Mass]         |
| `pl_msinijlim`  | Planet Mass\*sin(i) Limit Flag                        |
| `pl_cmasse`     | Planet Mass\*sin(i)/sin(i) [Earth Mass]               |
| `pl_cmasseerr1` | Planet Mass\*sin(i)/sin(i) Upper Unc. [Earth Mass]    |
| `pl_cmasseerr2` | Planet Mass\*sin(i)/sin(i) Lower Unc. [Earth Mass]    |
| `pl_cmasselim`  | Planet Mass\*sin(i)/sin(i) Limit Flag                 |
| `pl_cmassj`     | Planet Mass\*sin(i)/sin(i) [Jupiter Mass]             |
| `pl_cmassjerr1` | Planet Mass\*sin(i)/sin(i) Upper Unc. [Jupiter Mass]  |
| `pl_cmassjerr2` | Planet Mass\*sin(i)/sin(i) Lower Unc. [Jupiter Mass]  |
| `pl_cmassjlim`  | Planet Mass\*sin(i)/sin(i) Limit Flag                 |
| `pl_bmasse`     | Planet Mass or Mass\*sin(i) [Earth Mass]              |
| `pl_bmasseerr1` | Planet Mass or Mass\*sin(i) Upper Unc. [Earth Mass]   |
| `pl_bmasseerr2` | Planet Mass or Mass\*sin(i) Lower Unc. [Earth Mass]   |
| `pl_bmasselim`  | Planet Mass or Mass\*sin(i) Limit Flag                |
| `pl_bmassj`     | Planet Mass or Mass\*sin(i) [Jupiter Mass]            |
| `pl_bmassjerr1` | Planet Mass or Mass\*sin(i) Upper Unc. [Jupiter Mass] |
| `pl_bmassjerr2` | Planet Mass or Mass\*sin(i) Lower Unc. [Jupiter Mass] |
| `pl_bmassjlim`  | Planet Mass or Mass\*sin(i) Limit Flag                |
| `pl_bmassprov`  | Planet Mass or Mass\*sin(i) Provenance                |
| `pl_dens`       | Planet Density [g/cm**3]                              |
| `pl_denserr1`   | Planet Density Upper Unc. [g/cm**3]                   |
| `pl_denserr2`   | Planet Density Lower Unc. [g/cm**3]                   |
| `pl_denslim`    | Planet Density Limit Flag                             |

#### Planet Environment

| Column         | Description                        |
| -------------- | ---------------------------------- |
| `pl_insol`     | Insolation Flux [Earth Flux]       |
| `pl_insolerr1` | Insolation Flux Upper Unc.         |
| `pl_insolerr2` | Insolation Flux Lower Unc.         |
| `pl_insollim`  | Insolation Flux Limit Flag         |
| `pl_eqt`       | Equilibrium Temperature [K]        |
| `pl_eqterr1`   | Equilibrium Temperature Upper Unc. |
| `pl_eqterr2`   | Equilibrium Temperature Lower Unc. |
| `pl_eqtlim`    | Equilibrium Temperature Limit Flag |

#### Transit Parameters

| Column             | Description                                     |
| ------------------ | ----------------------------------------------- |
| `pl_tranmid`       | Transit Midpoint [days]                         |
| `pl_tranmiderr1`   | Transit Midpoint Upper Unc. [days]              |
| `pl_tranmiderr2`   | Transit Midpoint Lower Unc. [days]              |
| `pl_tranmidlim`    | Transit Midpoint Limit Flag                     |
| `pl_tsystemref`    | Time Reference Frame and Standard               |
| `ttv_flag`         | Data show Transit Timing Variations             |
| `pl_imppar`        | Impact Parameter                                |
| `pl_impparerr1`    | Impact Parameter Upper Unc.                     |
| `pl_impparerr2`    | Impact Parameter Lower Unc.                     |
| `pl_impparlim`     | Impact Parameter Limit Flag                     |
| `pl_trandep`       | Transit Depth [%]                               |
| `pl_trandeperr1`   | Transit Depth Upper Unc. [%]                    |
| `pl_trandeperr2`   | Transit Depth Lower Unc. [%]                    |
| `pl_trandeplim`    | Transit Depth Limit Flag                        |
| `pl_trandur`       | Transit Duration [hours]                        |
| `pl_trandurerr1`   | Transit Duration Upper Unc. [hours]             |
| `pl_trandurerr2`   | Transit Duration Lower Unc. [hours]             |
| `pl_trandurlim`    | Transit Duration Limit Flag                     |
| `pl_ratdor`        | Ratio of Semi-Major Axis to Stellar Radius      |
| `pl_ratdorerr1`    | Ratio of Semi-Major Axis to Stellar Radius Unc. |
| `pl_ratdorerr2`    | Ratio of Semi-Major Axis to Stellar Radius Unc. |
| `pl_ratdorlim`     | Ratio of Semi-Major Axis to Stellar Radius Flag |
| `pl_ratror`        | Ratio of Planet to Stellar Radius               |
| `pl_ratrorerr1`    | Ratio of Planet to Stellar Radius Unc.          |
| `pl_ratrorerr2`    | Ratio of Planet to Stellar Radius Unc.          |
| `pl_ratrorlim`     | Ratio of Planet to Stellar Radius Flag          |
| `pl_occdep`        | Occultation Depth [%]                           |
| `pl_occdeperr1`    | Occultation Depth Upper Unc. [%]                |
| `pl_occdeperr2`    | Occultation Depth Lower Unc. [%]                |
| `pl_occdeplim`     | Occultation Depth Limit Flag                    |
| `pl_rvamp`         | Radial Velocity Amplitude [m/s]                 |
| `pl_rvamperr1`     | Radial Velocity Amplitude Upper Unc. [m/s]      |
| `pl_rvamperr2`     | Radial Velocity Amplitude Lower Unc. [m/s]      |
| `pl_rvamplim`      | Radial Velocity Amplitude Limit Flag            |
| `pl_projobliq`     | Projected Obliquity [deg]                       |
| `pl_projobliqerr1` | Projected Obliquity Upper Unc. [deg]            |
| `pl_projobliqerr2` | Projected Obliquity Lower Unc. [deg]            |
| `pl_projobliqlim`  | Projected Obliquity Limit Flag                  |
| `pl_trueobliq`     | True Obliquity [deg]                            |
| `pl_trueobliqerr1` | True Obliquity Upper Unc. [deg]                 |
| `pl_trueobliqerr2` | True Obliquity Lower Unc. [deg]                 |
| `pl_trueobliqlim`  | True Obliquity Limit Flag                       |

#### Stellar Properties

| Column        | Description                                   |
| ------------- | --------------------------------------------- |
| `st_refname`  | Stellar Parameter Reference                   |
| `st_spectype` | Spectral Type                                 |
| `st_teff`     | Stellar Effective Temperature [K]             |
| `st_tefferr1` | Stellar Effective Temperature Upper Unc. [K]  |
| `st_tefferr2` | Stellar Effective Temperature Lower Unc. [K]  |
| `st_tefflim`  | Stellar Effective Temperature Limit Flag      |
| `st_rad`      | Stellar Radius [Solar Radius]                 |
| `st_raderr1`  | Stellar Radius Upper Unc. [Solar Radius]      |
| `st_raderr2`  | Stellar Radius Lower Unc. [Solar Radius]      |
| `st_radlim`   | Stellar Radius Limit Flag                     |
| `st_mass`     | Stellar Mass [Solar mass]                     |
| `st_masserr1` | Stellar Mass Upper Unc. [Solar mass]          |
| `st_masserr2` | Stellar Mass Lower Unc. [Solar mass]          |
| `st_masslim`  | Stellar Mass Limit Flag                       |
| `st_met`      | Stellar Metallicity [dex]                     |
| `st_meterr1`  | Stellar Metallicity Upper Unc. [dex]          |
| `st_meterr2`  | Stellar Metallicity Lower Unc. [dex]          |
| `st_metlim`   | Stellar Metallicity Limit Flag                |
| `st_metratio` | Stellar Metallicity Ratio                     |
| `st_lum`      | Stellar Luminosity [log(Solar)]               |
| `st_lumerr1`  | Stellar Luminosity Upper Unc. [log(Solar)]    |
| `st_lumerr2`  | Stellar Luminosity Lower Unc. [log(Solar)]    |
| `st_lumlim`   | Stellar Luminosity Limit Flag                 |
| `st_logg`     | Stellar Surface Gravity [log10(cm/s**2)]      |
| `st_loggerr1` | Stellar Surface Gravity Upper Unc.            |
| `st_loggerr2` | Stellar Surface Gravity Lower Unc.            |
| `st_logglim`  | Stellar Surface Gravity Limit Flag            |
| `st_age`      | Stellar Age [Gyr]                             |
| `st_ageerr1`  | Stellar Age Upper Unc. [Gyr]                  |
| `st_ageerr2`  | Stellar Age Lower Unc. [Gyr]                  |
| `st_agelim`   | Stellar Age Limit Flag                        |
| `st_dens`     | Stellar Density [g/cm**3]                     |
| `st_denserr1` | Stellar Density Upper Unc. [g/cm**3]          |
| `st_denserr2` | Stellar Density Lower Unc. [g/cm**3]          |
| `st_denslim`  | Stellar Density Limit Flag                    |
| `st_vsin`     | Stellar Rotational Velocity [km/s]            |
| `st_vsinerr1` | Stellar Rotational Velocity Upper Unc. [km/s] |
| `st_vsinerr2` | Stellar Rotational Velocity Lower Unc. [km/s] |
| `st_vsinlim`  | Stellar Rotational Velocity Limit Flag        |
| `st_rotp`     | Stellar Rotational Period [days]              |
| `st_rotperr1` | Stellar Rotational Period Upper Unc. [days]   |
| `st_rotperr2` | Stellar Rotational Period Lower Unc. [days]   |
| `st_rotplim`  | Stellar Rotational Period Limit Flag          |
| `st_radv`     | Systemic Radial Velocity [km/s]               |
| `st_radverr1` | Systemic Radial Velocity Upper Unc. [km/s]    |
| `st_radverr2` | Systemic Radial Velocity Lower Unc. [km/s]    |
| `st_radvlim`  | Systemic Radial Velocity Limit Flag           |

#### System Position & Photometry

| Column           | Description                             |
| ---------------- | --------------------------------------- |
| `sy_refname`     | System Parameter Reference              |
| `rastr`          | RA [sexagesimal]                        |
| `ra`             | RA [deg]                                |
| `decstr`         | Dec [sexagesimal]                       |
| `dec`            | Dec [deg]                               |
| `glat`           | Galactic Latitude [deg]                 |
| `glon`           | Galactic Longitude [deg]                |
| `elat`           | Ecliptic Latitude [deg]                 |
| `elon`           | Ecliptic Longitude [deg]                |
| `sy_pm`          | Total Proper Motion [mas/yr]            |
| `sy_pmerr1`      | Total Proper Motion Upper Unc. [mas/yr] |
| `sy_pmerr2`      | Total Proper Motion Lower Unc. [mas/yr] |
| `sy_pmra`        | Proper Motion (RA) [mas/yr]             |
| `sy_pmraerr1`    | Proper Motion (RA) Upper Unc.           |
| `sy_pmraerr2`    | Proper Motion (RA) Lower Unc.           |
| `sy_pmdec`       | Proper Motion (Dec) [mas/yr]            |
| `sy_pmdecerr1`   | Proper Motion (Dec) Upper Unc.          |
| `sy_pmdecerr2`   | Proper Motion (Dec) Lower Unc.          |
| `sy_dist`        | Distance [pc]                           |
| `sy_disterr1`    | Distance Upper Unc. [pc]                |
| `sy_disterr2`    | Distance Lower Unc. [pc]                |
| `sy_plx`         | Parallax [mas]                          |
| `sy_plxerr1`     | Parallax Upper Unc. [mas]               |
| `sy_plxerr2`     | Parallax Lower Unc. [mas]               |
| `sy_bmag`        | B (Johnson) Magnitude                   |
| `sy_bmagerr1`    | B (Johnson) Magnitude Upper Unc.        |
| `sy_bmagerr2`    | B (Johnson) Magnitude Lower Unc.        |
| `sy_vmag`        | V (Johnson) Magnitude                   |
| `sy_vmagerr1`    | V (Johnson) Magnitude Upper Unc.        |
| `sy_vmagerr2`    | V (Johnson) Magnitude Lower Unc.        |
| `sy_jmag`        | J (2MASS) Magnitude                     |
| `sy_jmagerr1`    | J (2MASS) Magnitude Upper Unc.          |
| `sy_jmagerr2`    | J (2MASS) Magnitude Lower Unc.          |
| `sy_hmag`        | H (2MASS) Magnitude                     |
| `sy_hmagerr1`    | H (2MASS) Magnitude Upper Unc.          |
| `sy_hmagerr2`    | H (2MASS) Magnitude Lower Unc.          |
| `sy_kmag`        | Ks (2MASS) Magnitude                    |
| `sy_kmagerr1`    | Ks (2MASS) Magnitude Upper Unc.         |
| `sy_kmagerr2`    | Ks (2MASS) Magnitude Lower Unc.         |
| `sy_umag`        | u (Sloan) Magnitude                     |
| `sy_umagerr1`    | u (Sloan) Magnitude Upper Unc.          |
| `sy_umagerr2`    | u (Sloan) Magnitude Lower Unc.          |
| `sy_gmag`        | g (Sloan) Magnitude                     |
| `sy_gmagerr1`    | g (Sloan) Magnitude Upper Unc.          |
| `sy_gmagerr2`    | g (Sloan) Magnitude Lower Unc.          |
| `sy_rmag`        | r (Sloan) Magnitude                     |
| `sy_rmagerr1`    | r (Sloan) Magnitude Upper Unc.          |
| `sy_rmagerr2`    | r (Sloan) Magnitude Lower Unc.          |
| `sy_imag`        | i (Sloan) Magnitude                     |
| `sy_imagerr1`    | i (Sloan) Magnitude Upper Unc.          |
| `sy_imagerr2`    | i (Sloan) Magnitude Lower Unc.          |
| `sy_zmag`        | z (Sloan) Magnitude                     |
| `sy_zmagerr1`    | z (Sloan) Magnitude Upper Unc.          |
| `sy_zmagerr2`    | z (Sloan) Magnitude Lower Unc.          |
| `sy_w1mag`       | W1 (WISE) Magnitude                     |
| `sy_w1magerr1`   | W1 (WISE) Magnitude Upper Unc.          |
| `sy_w1magerr2`   | W1 (WISE) Magnitude Lower Unc.          |
| `sy_w2mag`       | W2 (WISE) Magnitude                     |
| `sy_w2magerr1`   | W2 (WISE) Magnitude Upper Unc.          |
| `sy_w2magerr2`   | W2 (WISE) Magnitude Lower Unc.          |
| `sy_w3mag`       | W3 (WISE) Magnitude                     |
| `sy_w3magerr1`   | W3 (WISE) Magnitude Upper Unc.          |
| `sy_w3magerr2`   | W3 (WISE) Magnitude Lower Unc.          |
| `sy_w4mag`       | W4 (WISE) Magnitude                     |
| `sy_w4magerr1`   | W4 (WISE) Magnitude Upper Unc.          |
| `sy_w4magerr2`   | W4 (WISE) Magnitude Lower Unc.          |
| `sy_gaiamag`     | Gaia Magnitude                          |
| `sy_gaiamagerr1` | Gaia Magnitude Upper Unc.               |
| `sy_gaiamagerr2` | Gaia Magnitude Lower Unc.               |
| `sy_icmag`       | I (Cousins) Magnitude                   |
| `sy_icmagerr1`   | I (Cousins) Magnitude Upper Unc.        |
| `sy_icmagerr2`   | I (Cousins) Magnitude Lower Unc.        |
| `sy_tmag`        | TESS Magnitude                          |
| `sy_tmagerr1`    | TESS Magnitude Upper Unc.               |
| `sy_tmagerr2`    | TESS Magnitude Lower Unc.               |
| `sy_kepmag`      | Kepler Magnitude                        |
| `sy_kepmagerr1`  | Kepler Magnitude Upper Unc.             |
| `sy_kepmagerr2`  | Kepler Magnitude Lower Unc.             |

#### Metadata

| Column         | Description                                    |
| -------------- | ---------------------------------------------- |
| `rowupdate`    | Date of Last Update                            |
| `pl_pubdate`   | Planetary Parameter Reference Publication Date |
| `releasedate`  | Release Date                                   |
| `pl_nnotes`    | Number of Notes                                |
| `st_nphot`     | Number of Photometry Time Series               |
| `st_nrvc`      | Number of Radial Velocity Time Series          |
| `st_nspec`     | Number of Stellar Spectra Measurements         |
| `pl_nespec`    | Number of Eclipse Spectra                      |
| `pl_ntranspec` | Number of Transmission Spectra                 |
| `pl_ndispec`   | Number of Direct Imaging Spectra               |

---

**Reminder:** Your README should be clear enough that someone unfamiliar with your work could understand what you studied, how you analyzed it, and what you found.
