# Cypheroth
 Automated, extensible toolset that runs cypher queries against Bloodhound's Neo4j backend and saves output to csv.


<p align="center">
  <img src="./img/cypheroth.png" alt="cypheroth"/>
</p>


### Description

This is a bash script that automates running cypher queries against Bloodhound data stored in a Neo4j database.

I found myself re-running the same queries through the Neo4j web interface on multiple assessments and figured there must be an easier way. 😅

The list of cypher queries to run is fully extensible. The formatting example below shows how to add your own.

Please share any additional useful queries so I can add them to this project!

### Prereqs

* The `cypher-shell` command comes bundled with Neo4j, and is required for this script to function
  * _If Neo4j is installed and `cypher-shell` is not found, you may have an outdated version of Neo4j_
  * _The latest version can always be found at [this location](https://neo4j.com/download-center/)_
  * _On Kali, upgrade to the latest version using [Neo4j's Debian repository](https://debian.neo4j.org)_
* Optional: If the `ssconvert` command is present, the script will combine all .csv output to sheets within a .xls file
  * _Install the gnumeric toolset with apt or brew to gain access to ssconvert_

### Cypher Queries

The current query set requests the following information:

* Full User Property List
* Full Computer Property List
* Full Domain Property List
* Full OU Property List
* Full GPO Property List
* Full Group Property List
* Computers with Admins
* Computers without Admins
* Groups with Computers and Admins
* Group Admin Info
* Users that are not AdminCount 1, have generic all, and no local admin
* Users that are admin on 1+ machines, sorted by admin count
* Kerberoastable users sorted by total machine admin count
* Kerberoastable users and computers where they are admins
* Computers that members of the Domain Users group can RDP to
* Computers where users which can Return, if they belong to adm or svr accounts
* Computer names where each domain user has derivative Admin privileges to
* Users with paths to High Value groups
* Every computer account that has local admin rights on other computers
* Find which domain Groups are Admins to what computers
* What permissions does Everyone/Authenticated users/Domain users/Domain computers have
* All users with SPN in Domain Admin group, with enabled status and unconstrained delegation status displayed

To add additional queries, edit `queries.txt` and add a line using the following format:

`Description;Cypher Query;Output File`

Example: `All Usernames;MATCH (u:User) RETURN u.name;usernames.csv`

### Troubleshooting

If you are running an oudated version of `cypher-shell` you may receive the following error:

```
DateTime is not supported as a return type in Bolt protocol version 1.
Please make sure driver supports at least protocol version 2.
Driver upgrade is most likely required.
```

To fix, [update Neo4j](https://neo4j.com/download-center/) to the latest version.

If you're running this remotely, you might get a Connection Refused message. To
fix, ensure neo4j is listening remotely. This can be changed by uncommenting the following line and adding an IP address (0.0.0.0) for all interfaces, in neo4j.conf. Be aware of the security implications of this!
```
dbms.connector.bolt.listen_address=0.0.0.0:7687
```

### Author
Chris Farrell ([@seajay](https://twitter.com/seajay))

### Acknowledgments

* This tool wouldn't exist without BloodHound - developed by [@_wald0](https://twitter.com/_wald0), [@CptJesus](https://twitter.com/CptJesus), and [@harmj0y](https://twitter.com/harmj0y).
* Shoutout to the [Bloodhound Slack](https://bloodhoundgang.herokuapp.com) `#cypher_queries` channel for assistance
* Big ups to [@TinkerSec](https://twitter.com/TinkerSec) - the bones of this project were straight up copy/pasted from his [procdump script](https://github.com/tinkersec/scratchpad/blob/master/BashScripts/grabDump.sh) 🙃
* Many thanks to [@awsmhacks](https://twitter.com/awsmhacks) and [@haus3c](https://twitter.com/haus3c) for collecting useful cypher queries ([here](https://github.com/awsmhacks/awsmBloodhoundCustomQueries) and [here](https://hausec.com/2019/09/09/bloodhound-cypher-cheatsheet/))
