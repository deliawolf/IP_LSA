# IP_LSA


## Configure an IP SLA Echo Operation

Configure the campus HQ router, HQ, as the IP SLA source to generate ICMP echo traffic across the WAN.

1. On the HQ router, define the IP SLA with the number 1
```
HQ(config)# ip sla 1
```
This step defines the IP SLA operation number. The operation number is an arbitrarily chosen number that uniquely identifies an IP SLA test.

2. Configure IP SLA 1 on HQ to perform ICMP echo tests to the BR IP address of 172.16.22.254.
```
HQ(config-ip-sla)# icmp-echo 172.16.22.254
HQ(config-ip-sla)# exit
HQ(config)#
```
Configuring the test to perform starts with the keyword that specifies the test type. In this example, the test type is icmp-echo. The test type is followed by a list of parameters. In an ICMP echo IP SLA, the only mandatory parameter is the destination IP address. In this example, the destination IP address is that of BR—172.16.22.254.

It is a good idea to configure an IP SLA to the remote router’s loopback interface if there are multiple paths to reach that router.

3. From the global config, schedule IP SLA 1 on HQ to perform an ICMP echo test.
```
HQ(config)# ip sla schedule 1 life forever start-time now
```
The ip sla schedule command schedules when the test starts, for how long it runs, and for how long the collected data is kept.

Router(config)# ip sla schedule operation-number [life {forever | seconds}] [start-time {hh:MM[:ss] [month day | day month] | pending | now | after hh:mm:ss}] [ageout seconds] [recurring]

With the life keyword, you set how long the IP SLA test will run. If you choose forever, the test will run until you manually remove it. By default, the IP SLA test will run for 1 hour.

With the start-time keyword, you set when the IP SLA test should start. You can start the test right away by issuing the now keyword, or you can configure a delayed start.

With the ageout keyword, you can control how long the collected data is kept.

With the recurring keyword, you can schedule a test to run periodically—for example, at the same time each day.

4. On HQ, verify the IP SLA configuration.
```
HQ# show ip sla configuration
HQ# show ip sla statistics
HQ# show ip sla application

```


## Configure an IP SLA Jitter Operation

The IP SLA UDP jitter operation was designed primarily to diagnose network suitability for real-time traffic applications such as VoIP, video over IP, or real-time conferencing.

1. On the HQ router, define the IP SLA with the number 2.
```
HQ(config)# ip sla 2
```
2. Configure IP SLA 2 on HQ to perform a UDP jitter test to the BR IP address of 172.16.22.254.
```
HQ(config-ip-sla)# udp-jitter 172.16.22.254 65051 num-packets 20 
HQ(config-ip-sla)# request-data-size 160
HQ(config-ip-sla)# frequency 30
HQ(config-ip-sla)# exit
```
In this example, 20, 160-byte packets, will be sent 20 milliseconds apart every 30 seconds.
3. From global config, schedule IP SLA 2 on HQ to perform the jitter test.
```
HQ(config)# ip sla schedule 2 start-time now
HQ(config)# exit
```
4. Configure Branch as an IP SLA responder.
```
Branch(config)# ip sla responder
```

5. On HQ, verify the IP SLA configuration.
```
HQ# show ip sla configuration
HQ# show ip sla statistics
HQ# show ip sla application

```
