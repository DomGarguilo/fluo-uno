#### Steps to get things working
1. make two clones of the fluo-uno repository
	1. one will be used to stand up a 2.1.2 instance with no config changes and the other will be used to stand up a 3.1.0 instance with config changes
2. in the 3.1.0 clone, with the config changes run...
	1. `source <(./bin/uno env)`
	2. `./bin/uno fetch accumulo` to build from 3.1.0 source
	3. `./bin/uno setup accumulo` to populate the install dir
	4. `./bin/uno kill` to tear down the instance
4. in the 2.1.2 clone, `fetch` and `setup` normally. leave this instance running. be sure to run `source <(./bin/uno env)` beforehand.
5. back in the 3.1.0 clone run...
	1. `source <(./bin/uno env)` (might not be needed here actually)
	2. `./install/accumulo-3.1.0-SNAPSHOT/bin/accumulo init --instance-name uno3 --password secret`
		1. this creates the instance volumes in the already running hadoop install and the second set of ZK nodes in the new instance
		2. can peak at the hadoop dirs here and the ZK nodes to verify
	3. start up the rest of the components via `accumulo-service`
		1. `./install/accumulo-3.1.0-SNAPSHOT/bin/accumulo-service manager start`
		2. `./install/accumulo-3.1.0-SNAPSHOT/bin/accumulo-service monitor start`
		3. `./install/accumulo-3.1.0-SNAPSHOT/bin/accumulo-service tserver start`
		4. `./install/accumulo-3.1.0-SNAPSHOT/bin/accumulo-service gc start`
7. each instance has a monitor running so can check those out
	1. http://localhost:6995/ for 3.1.0
	2. http://localhost:9995/ for 2.1.2