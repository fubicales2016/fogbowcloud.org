Title: Rational
url: rational
save_as: rational.html
section: arch
index: 0

# Rational

## Why use IaaS clouds?

From a customer perspective, one of the greatest advantages of the infrastructure-as-a-service (IaaS) cloud computing paradigm is the fact that the customer is no longer required to perform mid/long-term capacity planning of the infrastructure that it will require in the future. The customer can simply allocate as many resources as the current demand requires, increase the capacity when the demand grows, and then release resources when the demand diminishes, at no additional cost for this "elasticity".

Obviously, the capacity planning problem has not disappeared, but has simply moved from the customer’s shoulders, to those of the IaaS providers. On the other hand, from the provider's perspective, IaaS brings a number of advantages that allow for more efficient management of the installed capacity, and consequently a reduced total cost of ownership. One of the main advantages is the possibility of reducing the overall capacity needed to support the aggregated demand of all customers through the consolidation of servers. This is because it is unlikely that all applications will experience peak demand at the same time. In addition, large IaaS providers also benefit from the economies of scale that arise when the number of customers served is big, and consequently a sizable infrastructure is needed.

## Why federate these clouds?

Small and medium private IaaS deployments can still take some advantage of the benefits of server consolidation, but they normally experience a smaller utilization of their physical resources, which represents costs that are not amortized by the utility associated with the execution of applications. The smaller utilization comes from the fact that with less applications to serve, it is less likely that peak demands from part of the applications will always coincide with low demand from the others, leading to a smaller potential for server consolidation.

Federation of private IaaS, by which a number of IaaS providers work together for their mutual benefit, has been advocated as a way to increase the infrastructure utilization, and as a consequence increase the efficiency of the federation members. In a federation the utilization can be increased mainly because the larger, and potentially more diverse, aggregated workload is likely to require less resources than the aggregated capacity of the IaaS providers, were they to work isolatedly. Members that are currently experiencing a low utilization of their resources can serve peak loads experienced by other members, thus allowing the local capacity of each member to be reduced. A smaller capacity leads to a higher utilization.

Federation is also a possible solution to host applications that require a geographically distributed deployment, be it to address dependability requirements, or simply to better serve a geographically distributed demand. In this case, capacities can be shared within the federation to increase the geographical dispersion of the resources that each IaaS provider can make avaiable to its customers.

Finally, federation is also important when there is a need to provide local users with a special purpose resources that are not available locally, <I>e.g.</I> GPGPUs.

## Why fogbow?

Fogbow is a middleware to federate IaaS clouds. It provides an API that implements the <a href=http://occi-wg.org/ target="_blank">OCCI</a> standard, and extensions to this standard to address federation functionalities, such as membership management and asynchronous instantiation of resources. These asynchronous requests, called <I>orders</I>, provide a more appropriate way to issue the remote creation of resources in a federation, and avoid failures due to timeouts and resource pre-emption. The middleware also provides extra functionalities useful for the federation of private clouds, such as a reverse tunnelling service to allow access to VMs with only private IPs, and mechanisms to deploy private clouds backed up by desktops exploited opportunistically and with power save capabilities. Finally, fogbow can be used as a simple way to seamlessly integrate in the federation resources that can be acquired from public IaaS providers, <I>i.e. cloudbursting</I>.

By deafult, fogbow provides a very lightweight business model for the federation of IaaS providers, based on the exchange of resources (VMs, storage, etc.) between the members of the federation. Barter is performed with no need for sophisticated billing, auditing and customer relations services. This business model was first designed and evaluated in the context of peer-to-peer desktop grids and implemented by the <a href="http://www.ourgrid.org/" target=_blank>OurGrid middleware</a>. For a complete description and assessment of this implementation, refer to <a href="http://www.ourgrid.org/4.2.6/index.php/pt/research" target=_blank>OurGrid’s publications</a>. In particular, have a look at <a href="http://www.sciencedirect.com/science/article/pii/S0743731507000706" target=_blank>"Automatic grid assembly by promoting collaboration in peer-to-peer grids"</a>. Of course, other even simpler or more sophisticated business models may well be introduced as new needs are identified by the comunity engaged in developing the software.

Fogbow has been designed to make its extension a simple task. This is achieved by the use of an architecture based on plugins, and enforcing a high test coverage of the source code. New functionalities can be easily included by implementing additional plugins, while tests give higher confidence to developers that the system will continue to work even if new contributions are constantly added.
