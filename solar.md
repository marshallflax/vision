# Residential Solar 101

---

## Nano Grid

```mermaid
graph LR;
 subgraph Generator ["Honda Generator"]
  direction LR;
  FuelInjection-->|Throttle|Combustion-->|Power|SpinningMetal["Spinning Hunk of Steel and Magnets"]
 end
 subgraph House
  Panel-->Load1
  Panel-->Load2
  Panel-->Load3
 end
 Generator-->|120v and 60hz|House
```

- If the load increases (or decreases), the spinning hunk of metal will encounter more (or less) magnetic resistance and start to slow down. The generator will notice this and increase (or decrease) the throttle to restore the 60hz frequency.
- This balance needs to be maintained: if the electrical energy created by the generator exceeds the electrical energy used by the loads, then that additional energy will become heat and damage the generator.

---

## Macro Grid

```mermaid
graph LR;
 subgraph Generator ["North American power transmission grid"]
  direction LR;
  FuelInjection["Natural Gas"]-->|Throttle|Combustion-->|Power|SpinningMetal["Spinning Hunks of Steel and Magnets"]
  Dam-->|Sluice Gates|Water-->|Power|SpinningMetal
  Fission-->|Control Rods|Steam-->|Power|SpinningMetal
  Coal-->|???|Steam
 end
 subgraph House
  Panel-->Transformer1
  Panel-->Transformer2
  Panel-->Transformer3
  Transformer2-->House1
  Transformer2-->House2
  Transformer2-->House3
  House2-->Load1
  House2-->Load2
  House2-->Load3
 end
 Generator-->|120v and 60hz|House
```

- Exactly the same as the Nano Grid, except that the law of large numbers means that the second-by-second variation of many users tends to cancel each other out.
- A mix of slow-to-adjust (e.g. nuclear), medium-to-adjust (e.g. coal), and  fast-to-adjust (e.g. natural gas or hydro) power sources works well.

---

## Residential Solar (without batteries)

```mermaid
graph LR;
 subgraph Sources["Energy Sources"]
  direction LR;
  Grid["Main North American Electrical Grid"]<-->ElectricPanel["Main 200Amp Panel"]
  SolarPanels<-->Inverter["DC-to-AC Inverter"]-->ElectricPanel
 end
 subgraph House
  Panel["200 Amp Panel"]-->Load1
  Panel-->Load2
  Panel-->Load3
 end
 Sources-->|120v and 60hz|House
```

- If the panels create more energy than the house needs, then the additional energy will simply flow onto the main grid.
- The DC-to-AC inverter *must* exactly match the voltage and frequency of the grid -- otherwise the electric fields will cancel each other out and create damaging heat.
- If the grid is down ("blackout") then:
  - No reference frequency and voltage
  - No place for excess energy to go
  - The solar inverter cannot safely function
  - No solar power

---

```mermaid
mindmap
 Solar
  No battery
   Super simple wiring
   No power during blackouts
   Only manual load shifting
   Highest grid feedin
  Whole house battery
   Simple wiring
   Least grid feedin
   Optimized for short blackouts
    Unless you manually shut down all noncritical loads
  Critical load battery
   Complicated and inflexible wiring
   Most resilient during long blackouts
```

---

```mermaid
graph TB;
 PV <--> SubPanel
 SubPanel --> Turtle
 SubPanel --> Furnace
 SubPanel --> Refrigerator
 SubPanel <--> Battery
 SubPanel --> Blender
 SubPanel --> Wifi
 SubPanel --> Den
 Battery <--> MainPanel
 MainPanel --> A/C
 MainPanel --> MostCircuits
 MainPanel --> WaterHeater
 MainPanel <--> Meter
 MainPanel --> CeilingLights
 MainPanel --> LivingRoom
 MainPanel --> Bedrooms
 Meter <--> Grid
```

