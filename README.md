## Logsearch for Cloud Foundry

A Logsearch addon that customises Logsearch to work with Cloud Foundry data

It consumes the syslog component log streams AND the doppler firehose stream, to provide log dashboards for 2 distinct user groups:

* **cf-users** (those deploying apps to the CF cluster) see a tenanted view of the doppler firehose data stored in Logsearch for just the apps in spaces they are members of.  They access the data via an app deployed on CF, and are required to authenticated against the CF's UAA component using the same credentials they would use to interact via the `cf` CLI tool.
The video below shows this in action:

[![Logsearch for Cloudfoundry - CF User view](https://cloud.githubusercontent.com/assets/227505/7177797/848e43a4-e421-11e4-912a-8803c1864cc1.png)](https://youtu.be/M-ODQwm98YM)

* **cf-operators** (those responsible for operating the CF cluster) can see data for all cf-user apps as well as data from all underlying CF components and the NATs message bus.
The video below shows this in action:

[![Logsearch for Cloudfoundry - CF Operator view](https://cloud.githubusercontent.com/assets/227505/7177840/d32fa890-e421-11e4-9127-dd2ce2ef36b9.png)](https://youtu.be/gWfoHCQUixM)

### Roadmap

* cf-cli plugin - A plugin for the cf-cli to enable searching of an applications logs - eg:

        cf log-search APP "type:RTR AND url:index.html"


### Getting Started

### Adding to existing Cloud Foundry + Logsearch deployments

This has been tested on cf-release v205, v207 and logsearch-boshrelease v19.

0.  Deploy the `ingestor_cloudfoundry` job to your existing logsearch deployment.

  * `bosh upload release https://logsearch-for-cloudfoundry-boshrelease.s3.amazonaws.com/boshrelease-logsearch-for-cloudfoundry-4.tgz`
  * See [logsearch-for-cloudfoundry-boshrelease/templates](https://github.com/logsearch/logsearch-for-cloudfoundry/tree/master/logsearch-for-cloudfoundry-boshrelease/templates) for example manifest files
  * Include `logsearch-for-cloudfoundry/logstash-filters-default.conf` log_parsing rules
  * `bosh deploy`
  * All app logs from your CF deployment should now be forwarded into your logsearch cluster. 
  * `bosh run errand push-kibana` will deploy Kibana4 and some sample dashboards to your CF cluster

_Sorry for the terse instructions; we're still finalising exactly how this will be deployed, and are delaying writing better instructions until that has happened_

### Developing

0. INSIDE your Logsearch-workspace,

```
git clone git@github.com:logsearch/logsearch-for-cloudfoundry.git ~/src/logsearch-for-cloudfoundry
cd ~/src/logsearch-for-cloudfoundry
bin/install_dependancies
bin/test
```

0. Make a failing test under `test/`
0. Run the tests `bin/test`
0. Make tests pass by writing code under `src/`
0. Ensure tests are green.
0. Create PR!
