# Detection Method and Insolation Flux in Exoplanets

## Project Scaffold

| Element                               | Your Plan                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Topic / Question**                  | Does discovery method influence the insolation flux of detected planets? If so, are our catalogs of potentially Earth-like worlds shaped by search technique rather than true planetary distribution?                                                                                                                                                                                                                                                                     |
| **Hypothesis**                        | **H₀:** Discovery method has no influence on insolation flux. The mean (or median) insolation flux of planets found by the Transit method is equal to that of planets found by the Radial Velocity method. Any observed difference is due to random chance.<br><br>**H₁:** Discovery method does influence insolation flux. The mean (or median) insolation flux of planets found by the Transit method differs from that of planets found by the Radial Velocity method. |
| **Outcome / Metric / Test Statistic** | `pl_insol` (Insolation Flux [Earth Flux]); difference in means between Transit and Radial Velocity groups                                                                                                                                                                                                                                                                                                                                                                 |
| **Units of Analysis**                 | Individual exoplanet entries from the NASA Exoplanet Archive                                                                                                                                                                                                                                                                                                                                                                                                              |
| **Data Source(s)**                    | [NASA Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=PS) — PS_2026.04.26_08.19.28.csv                                                                                                                                                                                                                                                                                                                        |
| **Why this data works**               | After filtering to one row per planet (`default_flag == 1`) and dropping missing insolation values, we have 916 planets — 795 Transit and 121 Radial Velocity — enough for both permutation testing and bootstrap resampling                                                                                                                                                                                                                                              |
| **Uncertainty Metric**                | Bootstrap confidence interval for the difference in mean/median insolation flux between the two discovery methods                                                                                                                                                                                                                                                                                                                                                         |
| **Null Hypothesis**                   | The type of detection technique astronomers use does not change the amount of starlight received by the planets they find. Any difference in insolation flux between Transit and Radial Velocity planets is due to random chance.                                                                                                                                                                                                                                         |

## 1. Research Question

For this project, I wanted to explore the [NASA Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/index.html) dataset. As of May 2, 2026, there are 6,278 confirmed planets that have been curated by NASA. Ultimately, I saw that most planets are usually discovered through two means: Transit, or Radial Velocity. Of the 6,278 planets archived by NASA, 4,640 were discovered through Transit, with 1,180 being discovered through Radial Velocity, which combined accounts for nearly 93% of all confirmed planets.

These two methods use different approaches for discovering planets. Transit looks for planets that come between us and their star, observed as a small dip in the star's brightness; this works best for planets orbiting extremely close to their star, which means they probably receive more starlight than Earth does. Radial Velocity measures a star's wobble caused by the gravitational pull of an orbiting planet, which tends to favor planets that are heavier and/or closer to their star, since both make the wobble stronger and easier to detect.

11:25 AMClaude responded: I then began to wonder about life beyond our planet Earth.I then began to wonder about life beyond our planet Earth. Could there be a statistically discernible difference in the planets discovered between these two methods? I saw that there's a measure called insolation flux for each planet, which is simply how much starlight a planet receives relative to Earth (where Earth = `1.0`). Thus, this project seeks to answer if there's a statistically discernible difference in insolation flux for planets discovered between Transit and Radial Velocity, and what that could mean in our search for Earth-like planets.

## 2. Hypothesis

**H₀ (Null):** Discovery method has no influence on insolation flux. The mean insolation flux of planets found by the Transit method is equal to that of planets found by the Radial Velocity method. Any observed difference is due to random chance.

**H₁ (Alternative):** Discovery method does influence insolation flux. The mean insolation flux of planets found by the Transit method differs from that of planets found by the Radial Velocity method.

## 3. Data Description

We use the [NASA Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=PS) Planetary Systems dataset (`PS_2026.04.26_08.19.28.csv`). Each observation is a single exoplanet entry. The full dataset contains 39,803 rows and 92 columns. For this analysis we filter to the Transit and Radial Velocity discovery methods and drop rows with missing `pl_insol` values.

## 4. Methods

We filtered the NASA Exoplanet Archive to one row per planet (`default_flag == 1`), kept only Transit and Radial Velocity discoveries, and dropped rows missing insolation flux (`pl_insol`), leaving 916 planets (795 Transit, 121 RV).

**Permutation test:** We simulated 10,000 permutations by randomly shuffling the discovery method labels and recomputing the test statistic each time. We ran two versions: one using the difference in means (matching our hypothesis) and one using the difference in medians (robust to extreme outliers). The means-based permutation produces a bimodal null distribution because hot Jupiters shuffled into the small RV group can spike the group mean. The medians-based permutation confirms the result with a clean unimodal distribution. We calculated a two-sided p-value for each as the proportion of permuted differences at least as extreme as the observed value.

**Bootstrap confidence intervals:** We constructed 95% CIs for two metrics: the difference in means and the difference in medians. For each, we drew 10,000 bootstrap samples (with replacement, same size as original) independently from each group, computed the statistic, and used the 2.5th and 97.5th percentiles as CI bounds. The difference in means is the primary metric matching our hypothesis. The difference in medians is a secondary metric where the CLT does not apply. There is no closed-form standard error for the median of heavily skewed data, making bootstrapping the only viable approach.

## 5. Results

| Metric                               | Observed Value    | Permutation p-value | 95% Bootstrap CI |
| ------------------------------------ | ----------------- | ------------------- | ---------------- |
| Difference in means (Transit − RV)   | 452.35 Earth flux | 0.0212              | [349, 598]       |
| Difference in medians (Transit − RV) | 75.61 Earth flux  | 0.0025              | [62, 97]         |

Both permutation tests reject the null hypothesis at α = 0.05, with the medians-based test (p = 0.0025) providing stronger evidence than the means-based test (p = 0.0212). Transit-detected planets receive significantly more stellar flux than RV-detected planets. Both bootstrap confidence intervals exclude zero, confirming the result. The typical (median) Transit planet receives about 80 Earth flux, compared to about 4 Earth flux for the typical RV planet.

## 6. Uncertainty Estimation

We used 10,000 resamples for both the permutation tests and the bootstrap confidence intervals. The permutation distribution for the difference in means is bimodal because extreme outliers shuffled into the small RV group can dominate the group mean. The permutation distribution for the difference in medians is unimodal and well-behaved, confirming the result is not driven by a handful of hot Jupiters. Both permutation tests reject the null at α = 0.05. On the bootstrap side, the distribution for the difference in means is right-skewed, reflecting the heavy tail in the underlying insolation data, while the distribution for the difference in medians is tighter and more symmetric. Both 95% CIs exclude zero, giving us confidence that the difference is real and not an artifact of sampling variability.

## 7. Limitations

- We can't tell whether Transit and RV find planets with genuinely different insolation, or whether each method just happens to be better at spotting certain kinds of planets. Transit is biased toward short-period orbits that sit close to their star, so of course those planets receive more flux. That's sort of the whole point of this analysis, but it does mean we're measuring a selection effect, not necessarily a physical one.
- After filtering, Transit planets outnumber RV planets roughly 7:1 (795 vs 121). That imbalance is real and reflects the field, but it means our RV estimates are less precise and the bootstrap CIs for that group are wider.
- Not every planet has a measured `pl_insol` value, and we had to drop those rows. The planets missing that measurement probably aren't a random subset, so our filtered sample may not perfectly represent the full population.
- We used `default_flag == 1` to pick one row per planet, which gives us NASA's "best" parameter set. But "best" is a curatorial choice, and a different set of measurements for the same planet could yield slightly different insolation values.

## 8. References

- NASA Exoplanet Archive — Transit method overview: https://exoplanetarchive.ipac.caltech.edu/docs/transit.html
- NASA Exoplanet Archive — Radial Velocity method overview: https://exoplanetarchive.ipac.caltech.edu/docs/rv.html

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
