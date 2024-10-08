---
layout: page
permalink: /moia/
title: Holzkirchen DRT Analysis
---

# Holzkirchen DRT Analysis

<div style="background-color: #ff8; font size: 2.0rem; padding: 1rem; border: 1px solid #33333360;">
Pooling Rate: <b>0.7044</b>
</div>

## Approach

I have analyzed DRT event files at VSP before using Python, so for expediency I chose to use Python for this analysis. MATSim event files can be analyzed in Java using the MATSim frameworks, but for data science tasks I usually prefer R or Python anyway. So let's go!

First, with pen and paper I considered what was needed to get from a raw events file to the desired pooling rate, and after reviewing the attributes of the various event types, I arrived at an approach which only required the `passenger picked up` and `passenger dropped off` events, since those two events have attributes for vehicle ID and request ID on them.

I leveraged the Python [matsim-tools](https://pypi.org/project/matsim-tools/) library, which I wrote :-) and which handles parsing XML and streaming events as a loop nicely.

The algorithm is fairly straightforward: keep two Python dictionaries in memory to track the progress of the simulation. One is keyed on vehicle ID, and includes the set of DRT requests currently being served by that vehicle. The second is keyed on the request ID, and is a simple boolean true/false marking whether that trip has shared a vehicle with passengers from another request.

As the events are streamed, these dictionaries are populated as needed. Pickup events trigger a check to see if the vehicle is currently serving other passengers; if so then those requests are all marked as pooled trips. Dropoff events trigger the housekeeping which removes requests from the tagged vehicle and increments the global counters for total trips.

At the end of the simulation, the code prints out some sanity checks; for example, the list of DRT requests in each vehicle should be empty.

Finally, the pooling ratio is calculated. My code results in 460 pooled trips out of 653 total trips, for a pooling rate of 0.70444. This does **not** take into account the zero-passenger "deadhead" trips where the driver is simply moving the car to pick up a passenger.

The KPIs are printed to the screen and are also output to `KPIs.csv` in case these values are expected to be piped into a larger data analysis framework.

**KPIs.csv:**

```
metric,value
Pooled trips,460
Total trips,653
Pooling rate,0.7044410413476263
```

If this were part of a larger analysis pipeline, I would take the time to write some unit tests and specifically I would create a small test XML file with which to verify that the code works as intended. Given the time constraints and my familiarity with Python scripting for DRT analysis, I did not want to construct a fake XML event file for testing when I could use that time to show some of my other skills instead.

Python has a built in unittest module which is barebones but sufficient; perhaps MOIA has their own testing framework already in use, and I would be very comfortable writing test-driven-development code if that is how you operate.

## Discussion

Based on previous experience with DRT simulations, I initially thought that a pooling rate of 0.7044 was _far too high_ especially for such a small city. So, I spent quite a bit of extra time adding logging/trace statements to my code. Alas, I cannot find any mistakes in the logic, so as far as I can tell, this rate is correct. Congratulations on having such a high pooling rate!

## Other measures

The efficiency of a pooling service can be measured in many ways, and the pooling rate is probably just the beginning of what you might want to look at.

I can imagine several other measures:

**Vehicle utilization:** what percentage of time in the simulation does each vehicle have at least one passenger inside? This could be calculated as a percentage of the total simulation time, or of a 24-hour window, as your simulation is 30 hours long.

Since I only spent about an hour on generating the pooling rate, I decided to also add calculations for the utilization rate into the analysis script. You can view the Git history if you would like to see the version of code for generating the pooling rate without the added analysis for utilization. Your average utilization rate across the whole simulation is **0.0648**.

**Circuitousness:** How far out of a passenger's way are they taken when they pool with other passengers? I believe the `unsharedRideTime` attribute on the DRT request is the shortest-path, and from the event stream the actual time for the passenger can easily be calculated. Less circuitous routes are beneficial to passengers and would make a better service. A similar Python post-processing analysis could then create some statistics on this topic.

**Driver deadheading:** Deadheading, the moving of vehicles without any passengers, could also be calculated easily from the events file. A KPI with the total amount of kilometers traveled with zero passengers would be interesting, and efforts to minimize that KPI would make the service more efficient.

I did not have time to add calculations for these other measures.

## DRT Animation

A live animation of the DRT service is below. This uses [SimWrapper](https://simwrapper.github.io), the open-source final product of my Ph.D. research.

In addition to making a visually compelling visualization of the simulation results, I use this for debugging. The color of the vehicles and routes represents the number of passengers in each vehicle: gray is driver-only, green is one passenger, yellow is two, and so on. When I saw the 0.7 pooling ratio, I thought inspecting the simulation visually would make it obvious that the calculated pooling rate was too high. But one can clearly see in the animation that many, many trips are pooled, up to five passengers at a time in some cases!

For this animation, I modified a Python script from SimWrapper (which I wrote) that postprocesses DRT event files and produces a JSON-formatted text file with the needed data for the vehicle paths, drt requests, and vehicle occupancy. I have included that script in the files I've created here, although please note I did not write the original version explicitly for this challenge.

<div style="height: 640px;width: 100%; border: none; border-radius: 8px">
<iframe
    src="https://simwrapper.github.io/staging/public/de/viz-examples/holzkirchen/viz-vehicles-moia.yaml?embed"
    style="height: 100%;width: 100%; border-radius: 8px"
    title="Holzkirchen">
</iframe>
</div>
<p><b><i>Figure 1. Holzkirchen DRT Animation.</i></b></p>

This is interactive: try sliding the timer to midday when there is more DRT action, enable the DRT requests to see "arcs" connecting origins and destinations, increase the speed or run the simulation backwards, and right-click-and-drag the map to change the 3D view.


## Time spent

Since you are evaluating my work, I also thought it would be helpful to help you know
how long this took.

- 0:30 Plan initial approach (pen/paper)
- 0:45 Writing Python script to get initial pooling rate
- 0:30 Debugging and tracing to see if 0.70 rate was actually correct

These additional tasks were performed after I produced the pooling rate above.

- 0:15 Write utilization rate code
- 0:15 Post-process event file for input to DRT animation
- 0:15 Copy animation files to server & viz setup
- 0:45 Documentation / webpage write-up

**TOTAL TIME: 3.25 hours**

Note, this is a very short script so I did not spend time dividing the code into small methods
and writing unit tests. I thought the time would be better spent adding additional
analyses, as I have done.

## Thank you

I hope this gives you an idea of the breadth of skills I would bring to MOIA. If I have made any mistakes or
omissions in the analysis, I look forward to discussing them with you.

All my best

..Billy
