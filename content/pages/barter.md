Title: Barter of wares
url: barter
save_as: barter.html
section: arch
index: 4

## The Network of Favors

Fogbow implements the Network of Favors (NoF) incentive mechanism, proposed by Andrade <I>et al.<\I>. In the NoF, the allocation of a certain amount of resources for a given amount of time is called a favor. A federation member keeps a local record of the total amount of resources provided to and received from each other member with which it has interacted in the past, which is updated from time to time, depending on the accounting granularity autonomously defined by each FM. Based on these values, each member calculates a balance of the interactions as the difference between the amount of resources received from a member and the amount of resources provided to that member. In case of resource contention, a member will always prioritize the provision of resources to the members to whom it has the highest debt, i.e. those with which it has the highest value in the balance of previously exchanged resources.
