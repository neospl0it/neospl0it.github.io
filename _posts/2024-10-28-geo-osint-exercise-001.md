---
title: "GEO-OSINT Exercise: Locating Coordinates from a Tweet in Kiffa, Mauritania"
description: "Discover how OSINT techniques can uncover precise locations from minimal clues. This case study explores geospatial tools and visual analysis to accurately trace coordinates based on an image from a tweet."
date: 2024-10-27
categories: [OSINT, Geolocation, GEO-OSINT, Case Study]
tags: [geo-osint, osint, geolocation, kiffa, mauritania, google earth pro, visual analysis, ethical osint]
image:  
  path: thbnls/geo-osint-01.png
---


## Task Briefing

Our mission originates from a GEO-OSINT exercise featured on [Gralhix](https://gralhix.com/list-of-osint-exercises/osint-exercise-001/), by **Sofia Santos**. The objective is to identify the exact location in a photo from a tweet. All relevant information for our investigation is contained within the image.

![Tweet Screenshot](bimgs/geo-osint-exercise-01/tweet-image.png)
*Tweet Post Screenshot*

Access the original photo without the Twitter border [here](https://gralhix.com/wp-content/uploads/2023/08/osint-exercise-001-big-picture.jpeg).

- **Tweet Post Date**: February 20, 2013
- **Translated Caption**: "Translated from Arabic by Google"
- **Location Mentioned**: Kiffa, Mauritania

The provided clues narrow our investigation down to **Kiffa**

## Understanding Kiffa: A Geographic Overview

Kiffa is a large town in the far south region of Mauritania, and the name of an administrative area within the local Assaba Region. Kiffa is located at 16.63°N 11.4°W, some 600 kilometres from the coast and at the western end of the Aoukar sand sea of southern Mauritania. 
                                                                                -- Wikipedia
## Image Analysis

To identify the location, we analyzed three main elements within the image: **building style**, **vegetation**, and **road infrastructure**.

![Image Analysis](bimgs/geo-osint-exercise-01/image-analysis.png)
*Tweet image analysis with visible structures, vegetation, and infrastructur*

### 1. Building Style

- **Structures**: The buildings have a simple, functional style, likely made of concrete or clay. This practical design is typical of rural areas with limited resources.
- **Layout**: The image suggests that the road extends past the buildings into an open area, possibly near the town’s edge.
- **Signage**: Arabic script on a building aligns with Mauritania’s official language.

### 2. Vegetation

- **Sparse Trees**: The few bushy trees suggest an arid or semi-arid environment
- **Ground Conditions**: The dry, sandy soil aligns with desert-like conditions.

### 3. Infrastructure

- **Road Condition**: The paved road appears to be underutilized, a characteristic of quieter, rural areas. A slight incline or dip hints at a nearby slope or small hill.
- **Power Lines**: Basic power lines indicate a developed, yet simple, infrastructure.

With these visual clues, we concluded that this area could be on the edge of Kiffa, near the town’s boundaries.

## Geospatial Investigation in Kiffa

Using **Google Earth Pro** and **Historical Imagery** from around **2013** (the date of the tweet), we identified potential locations in Kiffa that matched our visual analysis.

###  Initial Search in Google Maps

![Kiffa Map Overview](bimgs/geo-osint-exercise-01/google-map-kiffa.png)
*Google Maps view of Kiffa, Mauritania, showing main roads extending from the town*

Kiffa  It Looks Large In Dense also has four main roads extending outward. Only satellite imagery is available, but we used it to search for areas where the landscape and buildings matched the tweet image.

**Clue**: The road extending past the buildings into an open area hints at a location on the edge of town.

The paved road, appearing underutilized, suggests it may be in a less trafficked, rural setting. The visible incline or dip could indicate a nearby hill or slope.

![Marked Land](bimgs/geo-osint-exercise-01/marked-land.png)
*Google Earht Pro view of Kiffa, Mauritania, with Marked as 1 & 2 of main roads highlighted extending outward from the town*

Two of these main routes featured the necessary elements, such as vegetation and a nearby tree—a critical detail, as trees in such regions are uncommon and serve as stable visual markers.

### Matching Visual Elements Using Google Earth Pro

When we zooming on marked as 1 Land  we found something realted to our analysis hints

![Land-1 Marked](bimgs/geo-osint-exercise-01/land-1-hints.png)
*Google Earth Pro view of Kiffa, Mauritania, with Marked of Land 1 With Marked Down of Analysis Hints/clues*

After narrowing down our options, we identified a promising location along one of the primary routes outside Kiffa.

* The Building into an open area
* Tree
* Slight Dip Becasue of bridge

> **Note**
> Trees are the main highlight in a GEO-OSINT Investigation because it does not change place...
{: .prompt-info }

### Verifying with Historical Imagery

![GEO-OSINT](bimgs/geo-osint-exercise-01/final-image.png)
*Close-up in Google Earth Pro showing road layout and buildings in Kiffa matching tweet image details.*

Also Here We Changed To Years ro 2019 For Better Sunlight for get power lines shadow

We used **Historical Imagery** from 2013 in Google Earth Pro to analyze the area. The layout, buildings, and tree placement aligned well with the tweet image. 

In a closer look, all primary features—like the buildings on the town’s edge, the sandy terrain, the tree, and the simple road layout—matched up. When changing sunlight angles across different years, additional elements such as power lines and building shapes matched exactly with the tweet’s image.

## Final Coordinates and Location Verification

With all elements confirmed, the exact coordinates were determined as follows:

- **Coordinates**: [16.6239°N, 11.4035°W](https://earth.google.com/web/search/16.623894863947807,+-11.403535487008023/@16.60912585,-11.39783417,121.47785726a,183.05809358d,35y,171.79648006h,0t,0r/data=Cj4iJgokCUY5fAz2vjBAERSO2DerdjBAGYihTLybmyXAIV-oLFQT3ibAKhAIARIKMjAxOS0wMi0yMBgBQgIIAUICCABKDQj___________8BEAA).

This pinpointed location includes all matching elements: buildings on the town’s periphery, the characteristic tree, and road features consistent with the terrain.


## Conclusion: Ethical GEO-OSINT in Practice

This case study demonstrates the power of OSINT techniques when applied carefully and ethically. By using historical imagery, environmental clues, and structural patterns, we confirmed the location without breaching privacy.

> **Privacy Reminder**  
> All steps in this exercise were conducted using publicly available data in a way that respects privacy and adheres to responsible OSINT practices. This is essential in any location-based analysis.
{: .prompt-warning }


