---
title: "Geo-OSINT Exercise 002: Solving Clues in a Train Station Snapshot"
description: "Explore Geo-OSINT techniques in Exercise 002, where we analyze a train station image to identify its location and the tallest building in the background. Learn step-by-step OSINT methods to uncover location-based information from photos."
date: 2024-11-01
categories: [OSINT ,GEO-OSINT]
tags: [osint, geolocation, image analysis, train station identification, tallest building]
image:  
  path: thbnls/geo-osint-02.png
---

In this exercise, we're going to tackle a classic OSINT (Open Source Intelligence) challenge using only a shared image. The objective is simple yet intriguing: identify the train station and the tallest structure seen in the photograph. Let’s break down how we approached this, step-by-step.

#### Task Breakdown

![Exercise Image](/bimgs/geo-osint-exercise-02/osintexercise002.png)

Provided with a photograph (view the original [here](https://gralhix.com/wp-content/uploads/2024/09/osint-exercise-002-big-picture.png)) depicting a bustling train station surrounded by towering buildings. From this, we needed to answer the following questions:

1. What is the name of the train station?
2. What is the name and height of the tallest building in the photo?

With just these two questions, we set off on our Geo-OSINT adventure!

### Identifying the Train Station

#### Evidence and Verification

![Station Name](bimgs/geo-osint-exercise-02/signage-marked.png)
*Image showing the station’s name on the signage*

![Station Name Result](bimgs/geo-osint-exercise-02/stattion-name-google-map-result.png)
*Google Maps result showing the Flinders Street Station name*

After closely examining the image,  I was able to confirm that the train station seen in the photograph is **Flinders Street Station** in Melbourne, Australia.
### Determining the Tallest Structure

Next, we needed to identify the tallest structure in the photo. 

![2D Image Comparison](bimgs/geo-osint-exercise-02/2d-image-of-buldings.png)
*Comparison of the Google Earth Pro 2D image with the Geo-OSINT exercise image, marking each building*

Using Google Earth Pro, I identified several distinct buildings in the photo, each marked with a letter to help keep track. Here’s a rundown of the findings:

- **A - Arts Centre Melbourne**: 162 meters (531 feet)
- **B - Quay West Suites Melbourne (Quay West Apartments)**: 142 meters (466 feet), 44 floors
- **C - Herald Sun Building**: 85 meters (279 feet)
- **D - IBM Australia Building**: 131 meters (430 feet)
### The “Hidden” Building in the Background

Another skyscraper in the image that didn’t immediately appear on Google Earth’s 2D view.

![Bulding-E](bimgs/geo-osint-exercise-02/bulding-e.png)
*BUlding Marked to show the “hidden” building as "E"*

![Bulding-E Google Earth Pro](bimgs/geo-osint-exercise-02/bulding-e-google-earth.png) 
*Google Earth Pro image showing the "E" building*

To get a better look, zoomed out on Google Earth Pro and found it: **FOCUS Apartments by Central Equity**.

#### Details of FOCUS Apartments:

- **Height**: 166 meters (544 feet)
- **Floors**: 50

This discovery shifted our answer for the tallest building in the image, as **FOCUS Apartments** stands a few meters taller than Arts Centre Melbourne.

### Final Answer

After a thorough investigation, here are the answers to our Geo-OSINT Exercise 002 questions:

1. **Train Station**: Flinders Street Station, Melbourne, Australia
2. **Tallest Structure**: FOCUS Apartments by Central Equity at 166 meters (544 feet)

### Wrapping Up

This Geo-OSINT exercise exemplifies how we can leverage online mapping tools, geolocation techniques, and a keen eye for detail to uncover information about unfamiliar locations. Not only did we solve the mystery, but we also discovered how satellite imagery can sometimes hide details that require additional digging.

Whether you’re just starting out in OSINT or you’re a seasoned pro, these exercises are great for honing your investigative skills. Ready to try the next one?