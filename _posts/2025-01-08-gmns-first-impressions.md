---
title: "GMNS First impressions: lessons from MATSim and OMX"
layout: post
thumbnail: /images/blog/conn-avenue-dc.jpg
---

![Connecticut Avenue in Washington, DC](/images/blog/conn-avenue-dc.jpg)
_GMNS is the "General Modeling Network Specification" which is under development [here](https://github.com/zephyr-data-specs/GMNS)_


### GMNS: uninformed first impressions

I don't know any of the history or the path that was walked to arrive at GMNS 0.9. And I haven't coded anything -- no read/parse/write of these files yet. So this is at best an uninformed opinion about GMNS. But I do know a bit about networks generally. So here we go.

Having a CSV file with a WKT field for geometry makes it trivial to load this into generic non-modeler tools like GIS. Big win!!

I could easily write a CSV reader that parses just the nodes, links (and optional geometry tables for nice curvy links) -- thus I could build a GMNS viewer in SimWrapper in less than a day. Seriously! Coming soon.

### and some concerns...

Spec looks really decent... a lot of thought has gone into the design and I love that there is a JSON-compatible schema. But (yes it's a "but" to me) it is clearly inspired by GTFS "bag of disparate files" design. This is human-friendly but really sucks for performance, visualization, sharing, .... ugh. All the same problems of those old 90s multifile "shapefiles" that we escaped from ages ago.

- crs in config file
- WKT text strings for geometry embedded OR in separate file?
- long/lat floats as 10 digit strings, tons of repetition and yet not compressible :-(
- separate files means multiple remote file requests and ease of accidentally leaving things out by end user

The MATSim experience with large CSV files is that CSV is **absolutely untenable** for large networks. We have million-link MATSim simulations that literally crash the browser tab when we just try to load them from CSV text format.

But GMNS doesn't have to be text: it explicitly calls out "optional database" with an SQLite example! With all the same SQLite problems that we identified in our exploration of sqlite:

- enormous file size
- impossible to compress (ok those are the same thing)
- doesn't stream (at least CSVs can be streamed!)
- user needs to know they should add indexes or else get O(n) performance
- even bigger files once all the needed indexes are added
- SQLite performs pretty poorly in the browser (better now with WebAssembly but it's still quite complex)

For these reasons sqlite is a terrible fit for our large MATSim networks -- ten times larger than a gzipped XML Matsim network. Non starter. 😢

In discussion at the committee meeting, people talked about **converting GMNS to something that can be imported** for Aequilibra or Polaris (not sure which). I fear this is an admission of failure: the network format is not usable as-is. I'm curious why they didn't write a native reader for Polaris instead of converting? Maybe I misunderstood this part of the discussion.

### What made OMX so successful?

[OMX](https://github.com/osPlanning/omx), the open matrix data format for transport modeling, leverages the battle-tested HDF5 library for the underlying file format. Choosing a well-known, globally-used and supported container saved us from having to solve a lot of problems related to compression, data layout, processor cache optimization, multi-language API support, etc. I don't think there is any way we could have written a raw format better than HDF5.

I remember at the beginning of the OMX discussions some voices were very adamant that it be a text-only format for "archival purposes" and in case of technological apocalypse. Luckily saner voices won out. If I pretend I was a product manager type, OMX needed to be both **performant** and **easy to use**. Text formats were not going to cut it. Unfortunately here we are with text-based GMNS now...

Anyway -- network data is not matrix data and a tabular data format requires a different technology. So HDF5 doesn't work but....

### given what we now know about Apache Avro: would it work here?

We literally just this past year chose [Apache Avro](https://avro.apache.org/docs/) as a file container format for MATSim networks. Just like OMX leverages HDF5, MATSim leverages Apache Avro for its next-gen network format.

Avro can be thought of as an evolution of the "Protobuf" wire protocol. Unlike protobuf, Avro defines both _records_ and a _file format_ for those records. Avro:

- is a binary file format that compresses exceedingly well
- is lightning fast for both reading and writing, like shockingly fast 🚀
- has language bindings for every language imaginable
- has the data schema built in to the header of every file -- that means the very nicely described GMNS JSON schemas could be **part of the datafile itself** - eliminating entire classes of errors. You literally cannot write a record that does not conform to the schema you have specified. And unlike protobuf, there is no compilation step since the schema is part of the file itself instead of defined in an external definition doc.
- works best with **tabular data** where the set of columns are the same for every record. This was not ideal for MATSim where e.g. bike links have different attribute sets than auto links! Lots of NaN and empty fields were needed. 👎

For MATSim networks we kinda turned Avro "sideways": normally you would place one row of data per record, one record after another, like rows in a database. But for our needs we found it actually works better to group data columnwise a la UrbanSim or Pandas: the file has just a single record in the container, and that record contains a columnwise array for each network attribute. This enables insane compression and allows for very fast indexing into the data structure.

One big problem to solve is how to represent the proliferation of tables in GMNS: I am not sure how to handle multiple tables in an Avro file -- might be trivial? might be impossible/bad idea? Needs investigation.

### changesets

For Network Wrangler, networks are built as changesets that mutate the network in stepwise operations. Could each changeset be embedded in the file as a record? i.e., a *change record?* Like git commits or layers in Photoshop. If this were possible, it might open up some really interesting visualization capabilities and probably be good for debugging too.

I've no idea if this is possible or even relevant. I think right now NetWrangler is a toolset that produces a single output file that is the end result of all of the mutations. (right?) This would be a different thing. I wonder if we want to embed the changes in the file itself; Sijia also mentioned this in the committee meeting. Something to ponder.

### in summary

I don't know if Avro is actually the solution, but implementing Avro for MATSim networks helped us learn a lot about what works and what doesn't **in practice**.

Next steps are to (1) get the basic viewer up and running and then (2) think about the **real end user experience** of working with these GMNS networks.

Thanks for listening!

