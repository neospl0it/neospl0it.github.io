---
title: "GEO-OSINT Exercise 001: Locating in Kiffa, Mauritania"
description: "Discover how OSINT techniques can uncover precise locations from minimal clues. This case study explores geospatial tools and visual analysis to accurately trace coordinates based on an image from a tweet."
date: 2024-12-26
categories: [OSINT, GEO-OSINT, Case Study]
tags: [geo-osint, osint, geolocation, kiffa, mauritania, google earth pro, visual analysis, ethical osint]
image:  
  path: assets/img/thumbnails/geo-osint-01.png
---

## Description

Discover how OSINT techniques can uncover precise locations from minimal clues. This case study explores geospatial tools and visual analysis to accurately trace coordinates based on an image from a tweet.

## Task Briefing

Our mission originates from a GEO-OSINT exercise featured on [Gralhix](https://gralhix.com/list-of-osint-exercises/osint-exercise-001/), conducted by **Sofia Santos**. The objective is to identify the exact location in a photo sourced from a tweet. All relevant information for our investigation is contained within the image.

![Tweet Screenshot](/assets/img/bposts/geo-osint-exercise-01/tweet-image.png)
*Tweet Post Screenshot*

Access the original photo without the Twitter border [here](https://gralhix.com/wp-content/uploads/2023/08/osint-exercise-001-big-picture.jpeg).

- **Tweet Post Date**: February 20, 2013
- **Translated Caption**: "Translated from Arabic by Google"
- **Location Mentioned**: Kiffa, Mauritania

The provided clues narrow our investigation down to **Kiffa**.

## Understanding Kiffa: A Geographic Overview

Kiffa is a significant town located in the far south region of Mauritania and serves as the administrative center within the local Assaba Region. Geographically, Kiffa is positioned at **16.63°N 11.4°W**, approximately 600 kilometers from the coast, at the western edge of the Aoukar sand sea in southern Mauritania.  
— *Source: Wikipedia*

## Image Analysis

To identify the location, we analyzed three main elements within the image: **building style**, **vegetation**, and **road infrastructure**.

![Image Analysis](/assets/img/bposts/geo-osint-exercise-01/image-analysis.png)
*Tweet image analysis highlighting visible structures, vegetation, and infrastructure*

### 1. Building Style

- **Structures**: The buildings exhibit a simple, functional design, likely constructed from concrete or clay. This practical design is characteristic of rural areas with limited resources.
- **Layout**: The image suggests that the road extends past the buildings into an open area, possibly indicating the town’s edge.
- **Signage**: The presence of Arabic script on a building aligns with Mauritania’s official language.

### 2. Vegetation

- **Sparse Trees**: The few bushy trees imply an arid or semi-arid environment.
- **Ground Conditions**: The dry, sandy soil aligns with desert-like conditions.

### 3. Infrastructure

- **Road Condition**: The paved road appears to be underutilized, a typical feature of quieter, rural areas. A slight incline or dip hints at a nearby slope or small hill.
- **Power Lines**: Basic power lines indicate a developed, yet simple, infrastructure.

Based on these visual clues, we concluded that this area could be on the edge of Kiffa, near the town’s boundaries.

## Geospatial Investigation in Kiffa

Using **Google Earth Pro** and **Historical Imagery** from around **2013** (the date of the tweet), we identified potential locations in Kiffa that matched our visual analysis.

### Initial Search in Google Maps

![Kiffa Map Overview](/assets/img/bposts/geo-osint-exercise-01/google-map-kiffa.png)
*Google Maps view of Kiffa, Mauritania, showing main roads extending from the town*

Kiffa is notable for its extensive road network. Four main roads extend outward from the town. Although only satellite imagery is available, we utilized it to search for areas where the landscape and buildings matched the tweet image.

**Clue**: The road extending past the buildings into an open area hints at a location on the edge of town.

The underutilized paved road suggests it may be in a quieter, rural setting. Additionally, the visible incline or dip could indicate a nearby hill or slope.

![Marked Land](/assets/img/bposts/geo-osint-exercise-01/marked-land.png)
*Google Earth Pro view of Kiffa, Mauritania, with areas marked (1 & 2) highlighting the main roads extending outward from the town*

Two of these primary routes exhibited the necessary elements, such as vegetation and the presence of a nearby tree—a crucial detail, as trees in such regions are uncommon and serve as stable visual markers.

### Matching Visual Elements Using Google Earth Pro

Upon zooming in on the marked area (Land 1), we found features aligned with our analysis hints.

![Land-1 Marked](/assets/img/bposts/geo-osint-exercise-01/land-1-hints.png)
*Google Earth Pro view of Kiffa, Mauritania, highlighting Land 1 with marked analysis hints/clues*

After narrowing down our options, we identified a promising location along one of the primary routes outside Kiffa.

- **Building**: Located near an open area
- **Tree**: Present in the vicinity
- **Slope**: Suggestive of a nearby bridge

> **Note**  
> Trees are crucial in a GEO-OSINT investigation because their locations do not change over time.
{: .prompt-info }

### Verifying with Historical Imagery

![GEO-OSINT](/assets/img/bposts/geo-osint-exercise-01/final-image.png)
*Close-up in Google Earth Pro showing road layout and buildings in Kiffa that match tweet image details.*

We utilized **Historical Imagery** from 2013 in Google Earth Pro to analyze the area further. The layout, buildings, and tree placements aligned well with the tweet image.

We also changed the view to 2019 for better sunlight, allowing us to observe power line shadows more clearly.

In closer inspection, all primary features—including buildings on the town’s edge, sandy terrain, trees, and the simple road layout—matched perfectly. By adjusting the sunlight angles across different years, we found that additional elements such as power lines and building shapes corresponded exactly with the details in the tweet’s image.

## Final Coordinates and Location Verification

With all elements confirmed, we pinpointed the exact coordinates as follows:

- **Coordinates**: [16.6239°N, 11.4035°W](https://earth.google.com/web/search/16.623894863947807,+-11.403535487008023/@16.60912585,-11.39783417,121.47785726a,183.05809358d,35y,171.79648006h,0t,0r/data=Cj4iJgokCUY5fAz2vjBAERSO2DerdjBAGYihTLybmyXAIV-oLFQT3ibAKhAIARIKMjAxOS0wMi0yMBgBQgIIAUICCABKDQj___________8BEAA).

This pinpointed location includes all matching elements: buildings on the town’s periphery, the characteristic tree, and road features consistent with the terrain.

## Conclusion: Ethical GEO-OSINT in Practice

This case study demonstrates the power of OSINT techniques when applied carefully and ethically. By leveraging historical imagery, environmental clues, and structural patterns, we confirmed the location without compromising privacy.

> **Privacy Reminder**  
> All steps in this exercise were conducted using publicly available data in a way that respects privacy and adheres to responsible OSINT practices. This is essential in any location-based analysis.
{: .prompt-warning }