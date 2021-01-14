# CSL-Demos
Cities: Skylines - Algorithm demos

## Supply Chain Coloring demo - [SCC Demo - Oil City.crp](Saves/SCC%20Demo%20-%20Oil%20City.crp)

This is a gamesave of a city which uses only the Oil natural resource, which would be very tedious to achieve without using the [Supply Chain Coloring mod](https://steamcommunity.com/sharedfiles/filedetails/?id=2325122732) ([source](https://github.com/drok/SupplyChainColoring))

This is an exercise in extreme micromanagement; a typical player would not do this. However, the same technique can be used to tune a city's industrial production, eg, to slightly change the balance of industial prodution in order to fix traffic congestion problems, which are typical in Cities: Skylines.

## Transfer Broker demo - [Broker Demo.crp](Saves/Broker%20Demo.crp)

This gamesave demonstrates some issues with the vanilla Transfer Manager algorithm, which are addressed in the [Transfer Broker mod](https://steamcommunity.com/sharedfiles/filedetails/?id=2332314382) ([source](https://github.com/drok/TransferBroker)):

### Problem 1: Cargo delivered from the "wrong" warehouse

The district marked *"Problem 1: Cargo"* shows an Engineered Wood Plant receiving raw materials from one Small Log Yard (named "Slow Near Yard") instead of another, named "Fast Far Yard". The path to the "slow" yard is long and circuitous, even though it is physically closer, while the path to the "fast" yard is much shorter, although it is physically further away. Distance is simulated with two long delay loops/roads. Some locations are marked on the loop where a shortcut would change the effect of the algorithm

Technically, the "slow" yard is prefered due to ID sequencing, not because of proximity, because the plant is within both yards' basic service radius, but even so, this district demonstrates that the vanilla match-making algorithm is unaware of network routing.

The effect is of this problem is that the Engineered wood plant occasionally runs out of raw materials. Without using the Transfer Broker mod, a player would normally fix this problem by building additional roads, or shortcuts for the raw materials to be delivered to the Plant. There are labelled locations on the map where installing a shortcut would change the behaviour of this troubled supply chain:

#### Sustained Production Shortcut

A shortcut here would help avoid running out of raw materials, but would cause work stoppage due to "not enough customers". This is because the Plant's delivery route to export is long enough that the Plant uses all its trucks for delivery, then it has to stop producing when it can send no more delivery trucks, and its output buffer fills up.

#### Timber Warehouse

Next to the Plant there is a Warehouse Yard for Planed Timber (named "Timber Warehouse"), and initially turned **off** in the gamesave. Turning this **on** allows the plant to deliver to the Warehouse instead of direct to export, which avoids it running out of delivery trucks. Since the Plant has 8 trucks and the Warehouse has 10, together they can service a Timber supply line that would require up to 18 trucks. The length of a delivery line can be thought of in units of trucks. This "unit" of measurement encopasses the effects of production rate, truck capacity, delivery distance, road speed and traffic congestion.

#### Plant Output Warehousing Balance Shortcut

On the Rockwood Delay loop there is a location marked "Plant Output Warehousing Balance". A shortcut here would shorten the Planed Timber delivery route just enough that it can deliver Timber continuously with its 8 trucks without production stoppage. In its saved state, the road network is tuned to require just over 8 trucks to deliver Timber continuously, which forces production stoppage.

#### Prefer Imports to Slow Yard

With the Transfer Broker mod installed, raw logs would be delivered from the "Fast Far Yard" instead of the "Slow Near Yard", since it matches based on trip-time (road distance divided by road speed). Turn off the Fast Far Yard, the the choices for Raw materials remain 3: Slow Near Yard, Import, and the local Tree Plantation. The vanilla matchmaker would pick the Slow yard or the plantation, depending on priority and sequencing, because they are both within its basic service radius, and never consider import as an option.

The road network is tuned such that the Transfer Broker would prefer to import Logs because it results in quicker delivery.

Installing a shortcut at the location marked "Prefer Rockwood to Slow Yard" would shorten the delivery time from the Slow Yard by enough that it would become preferred over importing from Rockwood.

### Problem 2: Garbage

The district marked "Problem 2: Garbage" is a minimal community with two garbage incinerators, connected to the rest of the town only with a pedestrian foot path. With these two incinerators disabled, the town would be fully serviced by the 3rd incinerator, by using only 2/27 trucks.

Since the matchmarker does not take pathing into account, it will match the two incinerators with offers from the rest of town, even though trucks from these incinerators cannot travel into town. This causes the incinerators' "Garbage trucks in use" to flash 1/27 briefly and return to 0/27, indicating the pathfinder failed to find a route.

The scenario of an isolated community cannot be addressed by the vanilla matchmaker. The road network would have to be fully connected, so that every house can potentially be serviced by every incinerator, otherwise areas will develop where the garbage piles up. Additionally, this matching algorithm also creates unnecessary traffic congestions as trucks are sent further than neeed.

In this small save, garbage occasionally piles up, but it is *eventually* (accidentally) picked up because the road network has very few places where a truck can turn around, forcing them to travel even further than the matchmaking algo would imply. This causes garbage matched to the isolated incinerators to be picked up by the connected incinerator's trucks, *not deliberately*, but because these trucks have no choice but pass by the piles of garbage on the way to the end of the street, where they can make a U-turn, picking them up in the process.

Note that garbage trucks that finish their pickup in the "Problem 2" district immediately despawn. This is because after picking up from one house, they are matched again with another house, this time in town, but since it is unreachable, the path-finding algorithm requests their despawn. The garbage collected up to that point is teleported directly to the owning Incinerator. One could abuse this by creating a more efficient garbage infrastructure, where garbage trucks pickup everything they can, but never travel through the city on a return path; it should cut down traffic congestion due to garbage collection significantly.
