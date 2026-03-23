# Building Blocks

## Physical Components

You need some switches/routers with connections between them: 

```mermaid
flowchart LR
    A(Switch 1)
    B(Switch 2)
    A ---- B
    B ---- A
```

```mermaid
flowchart LR
    A(Switch 1)
    B(Switch 2)
    C(Switch 3)
    A --- B
    B --- C
    C --- A
```

```mermaid
flowchart LR
    A(Switch 1)
    B(Switch 2)
    C(Switch 3)
    D(Switch 4)
    A --- B --- C --- D --- A
```

```mermaid
flowchart LR
    A(Switch 1)
    B(Switch 2)
    C(Switch 3)
    D(Switch 4)
    A --- B --- C --- D --- A
    B --- D
    A --- C
```

Topology is arbitrary (within reason).

## Logical Components

### Link addressing

Each link between switches is configured with an IP subnet, usually /30 or /31

```mermaid
flowchart LR
    A(Switch 1)
    B(Switch 2)
    A -- 192.168.0.0/31 --- B
    B -- 192.168.1.0/31 --- A
```



### Loopback addressing

Each switch is configured with a loopback IP address (or sometimes two, depending on the platform)

```mermaid
flowchart LR
    A(Switch 1\n172.16.0.0/32)
    B(Switch 2\n172.16.0.1/32)
    A -- 192.168.0.0/31 --- B
    B -- 192.168.1.0/31 --- A
```

### Interior Gateway Protocol

The underlay IGP's function is to enable all of the switches to reach every other switch's loopback address. 

IGP options: 

- **OSPF**: easy button, works for small networks, limited control
- **IS-IS**: also easy, works well for small or large networks, better control
- **BGP**: not quite as easy, most flexible
- **Static routes**: possible for two-node networks with a single logical connection (*n* links in LAG)

### Overlay Routing Protocol — MP-BGP

