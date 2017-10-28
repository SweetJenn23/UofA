Things that are good to know for hackathons:

**Run the following commands to stop the Hyperledger Fabric:**

`cd /data/linux1/composer-tools/packages/fabric-dev-servers/`

`./stopFabric.sh`

**To remove a container:**

`docker ps -a`

In the left column you’ll see a list of container id’s, use `docker rm #######` for every containerID; paste the containerID from the output of the `docker ps -a` and paste it in place of #######

**To start the Hyperledger Fabric:**

`cd /data/linux1/composer-tools/packages/fabric-dev-servers/`

`./startFabric.sh`

**To start Composer Playground:**

nohup composer-playground >/data/playground/playground.stdout 2>/data/playground/playground.stderr & disown

**To start Composer Rest Server:**

nohup composer-rest-server -p hlfv1 -n journey-deploy -i PeerAdmin -s linux -N always >/data/linux1/playground/rest.stdout 2>/data/linux1/playground/rest.stderr & disown

NOTE: -n is used to specify the containerID; The containerID is the name of the project you gave in Hyperledger Composer

**To stop Composer Playground:**

`ps -ef|grep playground`

The PID will be the numbers before the playground name. 

`kill PID`

**Places to search for coding help or error code issues:**

**Google!!!**

\- Hyperledger Composer docs: <https://hyperledger.github.io/composer/introduction/introduction.html>

\- Hyperledger Composer Stackoverflow: <https://stackoverflow.com/questions/45182776/hyperledger-fabric-hyperledger-composer>

\- Hyperledger Composer Rocketchat: <https://chat.hyperledger.org/channel/composer>
