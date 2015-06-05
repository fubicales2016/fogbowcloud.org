Title: Rational
url: rational
save_as: rational.html
section: arch
index: 0

# Rational

## Why use private IaaS clouds?

From a customer perspective, one of the greatest advantages of the infrastructure-as-a-service (IaaS) cloud computing paradigm is the fact that the customer is no longer required to perform mid/long-term capacity planning of the infrastructure that it will require in the future. S/he can simply allocate as many resources as the current demand requires, increase the capacity when the demand grows, and then release resources when the demand diminishes, at no additional cost for this "elasticity".

Obviously, the capacity planning problem has not disappeared, but has simply moved from the customer’s shoulders, to those of the IaaS providers. On the other hand, from the provider's perspective, IaaS brings a number of advantages that allow for more efficient management of the installed capacity, and consequently a reduced total cost of ownership. One of the main advantages is the possibility of reducing the overall capacity needed to support the aggregated demand of all customers through the consolidation of servers. This is because it is unlikely that all applications will experience peak demand at the same time. In addition, large IaaS providers also benefit from the economies of scale that arise when the number of customers served is big, and consequently a sizable infrastructure is needed.

## Why federate these clouds?

Small and medium private IaaS deployments can still take some advantage of the benefits of server consolidation, but they normally experience a smaller utilization of their physical resources, which represents costs that are not amortized by the utility associated with the execution of applications. The smaller utilization comes from the fact that with less applications to serve, it is more unlikely that peak demand from part of the applications always coincide with low demand from the others, leading to a smaller potential for server consolidation.

Federation of private IaaS, by which a number of IaaS providers work together for their mutual benefit, has been advocated as a way to increase the infrastructure utilization, and as a consequence increase the efficiency of the federation members. In a federation the utilization can be increased mainly because the larger, and potentially more diverse, aggregated workload is likely to require less resources than the aggregated capacity of the IaaS providers, were they to work isolatedly. Members that are currently experiencing a low utilization of their resources can serve peak loads experienced by other members, thus allowing the local capacity of each member to be reduced. A smaller capacity leads to a higher utilization.

## Why fogbow?

By deafult, fogbow provides a very lightweight business model for the federation of IaaS providers, based on the exchange of resources (VMs, storage, etc.) between the members of the federation. This <b>B</b>artering <b>O</b>f <b>W</b>ares (and that explains the <i>bow</i> in fogbow) is performed with no need for sophisticated billing, auditing and customer relations services. Of course, other even simpler or more sophisticated business models may well be introduced as new needs are identified by the comunity engaged in developing the software.

Ok, but what about the <i>fog</i>, in fogbow? Different from a rainbow that displays a bright continuous spectrum of colors, a fogbow has only very weak colors, with a red outer edge and a bluish inner edge. This is because the fog water drops are much smaller than those in the rain. Fogbow targets the federation of small and medium size IaaS providers, rather than large ones. Moreover, although it is possible to experience its distinct "edges" (<b>F</b>ederation, <b>O</b>pportunism and <b>G</b>reenness) in separate, they often provide a better effect when appreciated as a whole.

Fogbow has also been designed to make it simple to be extended. This is achieved by the use of an architecture based on plugins, and enforcing a high test coverage of the source code. New functionalities can be easily included by implementing additional plugins, while tests give higher confidence to developers that the system will continue to work even if new contributions are constantly added.

