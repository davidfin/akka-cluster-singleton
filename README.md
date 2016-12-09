# akka-cluster-singleton

This project was developed as part of the advanced akka training course by LightBend.

## Cluster Singleton
Used when there is a requirement for exactly one instance of a certain type of actor in the cluster. 
In this case, there must be only one instance of the playerRegistry Actor. A cluster singleton manager is started as follows: 

```scala
system.actorOf(
      ClusterSingletonManager.props(
        PlayerRegistry.props,
        PoisonPill,
        ClusterSingletonManagerSettings(system).withRole("player-registry")),
      "player-registry-singleton-manager")
```

A cluster singleton proxy is created in order to route messages to the current singleton actor and buffer the messages if 
it is not available. 

``` scala
system.actorOf(
  ClusterSingletonProxy.props(
    s"/user/player-registry-singleton-manager",
    ClusterSingletonProxySettings(system).withRole("player-registry")),
    "player-registry-proxy")
```

## Exercise
In this exercise we use a Cluster Singleton to make the PlayerRegistry strictly consistent and highly available. 

1. Reference the cluster singleton in GameEngine.scala 
2. create a clusterSingletonManager and ClusterSingletonProxy in the PlayerRegistryApp.scala
3. playerRegistry is now an ActorRef not an ActorSelection, Tournament.scala must be updated. 

### command aliases (ge, pr, sr, ge2, pr2, sr2 and sj)
```scala
ge  // runs the game engine on port 2551
pr  // runs the player registry on port 2552
sr  // runs the scores repository on port 2553
ge2 // runs the game engine on port 2554
pr2 // runs the player registry on port 2555
sr2 // runs the scores repository on port 2556
sj  // runs the shared journal on port 2550
```
