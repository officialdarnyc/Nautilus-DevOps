# What is a Firewall

A firewall is a system designed to prevent unauthorized access to or from a private network. You can implement a firewall in either hardware or software form, or in a combination of both. Firewalls prevent unauthorized internet users from accessing private networks connected to the internet, especially intranets. All messages entering or leaving the intranet (the local network to which you are connected) must pass through the firewall, which examines each message and blocks those that do not meet the specified security criteria.

Several types of firewalls exist:

1. Packet filtering: The system examines each packet entering or leaving the network and accepts or rejects it based on user-defined rules. Packet filtering is fairly effective and transparent to users, but it is difficult to configure. In addition, it is susceptible to IP spoofing.

2. Circuit-level gateway implementation: This process applies security mechanisms when a TCP or UDP connection is established. Once the connection has been made, packets can flow between the hosts without further checking.

3. Acting as a proxy server: A proxy server is a type of gateway that hides the true network address of the computer(s) connecting through it. A proxy server connects to the internet, makes the requests for pages, connections to servers, etc., and receives the data on behalf of the computer(s) behind it. The firewall capabilities lie in the fact that a proxy can be configured to allow only certain types of traffic to pass (for example, HTTP files, or web pages). A proxy server has the potential drawback of slowing network performance, since it has to actively analyze and manipulate traffic passing through it.

4. Web application firewall: A web application firewall is a hardware appliance, server plug-in, or some other software filter that applies a set of rules to an HTTP conversation. Such rules are generally customized to the application so that many attacks can be identified and blocked.