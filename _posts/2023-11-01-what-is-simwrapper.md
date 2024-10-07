---
title: 'SimWrapper: what is it exactly, and how can I start using it?'
layout: post
thumbnail: /images/projects/simwrapper.jpg
---

I get a lot of questions about SimWrapper, the open-source dashboard and data visualization portal specifically designed to help users of MATSim and ActivitySim see their outputs and understand what their models are trying to tell them.

Hopefully this short intro will help you "get it" and understand what it's all about!

## What is SimWrapper?

SimWrapper is a website, available to anyone at https://simwrapper.github.io. It is a tool that allows you to inspect the outputs of transport microsimulation models - originally for MATSim, but now also used for ActivitySim and other agent- and activity-based models around the world.

From the main SimWrapper website, you can navigate to the output folders on your local machine using Google Chrome or MS Edge; or if your data is stored in the cloud it can be configured to access remote data, too. All of this is explained in detail in the [SimWrapper documentation](https://simwrapper.github.io/docs).

You can create dashboards that summarizes CSV tabular datasets and create typical charts and graphs of things like trip origins, trip lengths, activies by time of day or mode of travel, etc. All of things that transport planners are typically interested in are supported.

But beyond simple charts, SimWrapper also has a powerful 3-D mapping engine based on [Deck.gl](https://deck.gl) so you can see your results -- for a single run or for comparisons between runs. It's easy to set up network volume plots, look at public transit ridership by route, and so on. Large datasets are handled well but the size of datasets is limited by your memory and graphics card limits.

Finally, SimWrapper configurations can be stored in small text files in "YAML" format next to your model output files, and these configurations can be used to define standard modeling dashboards for projects. You only need to configure things once and then the dashboards are available for all output folders inside a scenario.

## Who is using it? Can I see some examples?

Originally developed at Technische Universität Berlin, SimWrapper is used for a wide variety of projects in Germany and Europe. Here are just a few typical example dashboards:

- [Berlin Open Scenario](https://simwrapper.github.io/site/public/de/berlin/berlin-v6.3/output/berlin-v6.3-10pct)
- [Leipzig "NaMaV" project](https://vsp.berlin/simwrapper/public/de/leipzig/projects/namav/v1.3.1/drt-city-wide)

Beyond Germany, SimWrapper is also used for ActivitySim and BEAM models worldwide:

- [VTA BEAM website](https://simwrapper.github.io/beam/files/vta-scen_30pct_Berryessa_S2S_updated__2024-01-17_21-46-26_wug) - Santa Clara Valley Transportation Authority,Silicon Valley, CA
- [iMove Australia](https://imove.billyc.cc/explore) -- CompassIoT data exploration tool

## Great, I have some model outputs. How do I get started?

Start with the [documentation site](https://simwrapper.github.io/docs) to learn about SimWrapper basics.

SimWrapper works best with tabular data, such as CSV files. But other file formats are also supported, especially native MATSim outputs. If you can convert your data files to CSV, you can use SimWrapper!

Note: if your simulation outputs are on your local laptop or desktop computer, your web browsers normally blocks access to local files. This is a good thing! But SimWrapper wants access to that output folder. On Google Chrome and MS Edge, you can grant access to your local output folders by clicking on **Open Local Folder...** from the main SimWrapper page. If you are using Firefox or Safari, you need to [read this](https://simwrapper.github.io/docs/file-management#local-folders-on-your-computer). _We recommend Edge and Chrome since they support this expanded local folder access._

### MATSim

MATSim has a SimWrapper "contrib" which post-processes simulation outputs into a format that is carefully designed to work well with SimWrapper. Follow [the instructions here](https://github.com/matsim-org/matsim-libs/tree/master/contribs/simwrapper) to add one line of code to your MATSim run script, and SimWrapper outputs will be created automatically.

### ActivitySim

The ActivitySim repo has some scripts which might be useful -- [see here](https://github.com/ActivitySim/activitysim/blob/ecb00b545a703497be712fafe021c0e47c5b73a4/docs/users-guide/visualization.rst#L4) for the SimWrapper autosummarization scripts for ActivitySim

You will probably need to write some additional post-processing scripts to massage your ActivitySim tabular data into formats specific to SimWrapper, depending on what you want to see.

## Where to get help

SimWrapper documentation:

- Main documentation <https://simwrapper.github.io/>
- Getting Started guide <https://simwrapper.github.io/docs/guide-getting-started>

On GitHub:

- Q&A Site for help: <https://github.com/orgs/simwrapper/discussions>
- Issue tracker for bugs: <https://github.com/simwrapper/simwrapper/issues>

I hope this helps you get started!

..Billy
