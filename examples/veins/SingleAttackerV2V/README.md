# SingleAttackerV2V

A single attacker performs DoS and floods the network. Attack originates from a malicious vehicle actor. 

An attacker is defined in `SingleAttackerV2VScenario.ned`. 

Attack traffic is defined in `erlangen.rou.xml` as `vtype1`, `route1`, & `flow1`. 

`omnetpp_savs.ini` further defines `attacker` parameters (such as `applType`) and also associates the `vtype1` traffic definied in the .xml file with the `attacker` node. 
#### *This association is crucial to successfully spawning different kinds of traffic in scenarios.* ####

#

The `applType` of the `attacker` is defined as `TraCIV2Vp`. This is a copy of the `TraCIDemo11p` implementation, except `TraCIV2Vp` repeats its response (10000 times) to received WSMs to flood. 
https://github.com/sendecomp/veins/tree/master/src/veins/modules/application/traci

