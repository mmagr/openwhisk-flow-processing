# OpenWhisk backend for dojot flow processing

This repo contains the files used to validate the idea of mapping individual processing nodes'
implementations (switch, set, send mail, etc) as individual openWhisk actions - inspired by
sample architectures as seen at [openWhisk docs](https://github.com/apache/incubator-openwhisk/blob/master/docs/use_cases.md)

That however, proved not to be a good idea - mainly due to high disk IO operations involved
with the booting of the actions' associated containers taking too long, thus leading to
a percieved latency in the order of seconds for the entire flow to be processed.

Those tests were conducted on a very, very simple environment set using [openwhisk-devtools](https://github.com/apache/incubator-openwhisk-devtools)
docker-compose environment with no configuration tuning whatsoever.

For completeness, those tests were run on a dev machine: intel i7 2.7GHz, 16GB of RAM,  1TB HDD (140MB/s)

## Test description

To start off, a couple of dummy actions were created (`_device.js`, `set.js`, `switch.js`) and
configured within openwhisk. Then a "master" action was implemented (`_flow.js`). The idea was to
have the "master" action be called with a flow configuration (e.g. `test.json`) and the message
that triggered the flow, have it parse the flow definition file and trigger the required actions to
fulfill it.

As mentioned before, that proved to be unfeasible since each individual may end up taking too long
to be spawned, executed and return leading individual end-to-end flows to have a perceived latency
of ~3s on average (sometimes as high as 6.5s).
