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

## Residential Solar (whole-house battery)

<pack scale=0.90>

```mermaid
graph LR;
 subgraph Sources["Energy Sources"]
  direction LR;
  Grid["Main North American Electrical Grid"]<-->Inverter
  SolarPanels<-->Inverter["DC-to-AC Inverter and management"]-->ElectricPanel["Main 200Amp Panel"]
  Battery<-->Inverter
 end
 subgraph House
  Panel["200 Amp Panel"]-->Load1
  Panel-->Load2
  Panel-->Load3
 end
 Sources-->|120v and 60hz|House
```

- In normal operation, the DC-to-AC inverter gets its frequency and voltage from the North American Grid.
  - The battery will charge during the day (and once full, the excess will go back onto the grid).
  - The battery will discharge at night (and once at its minimum value, power will be drawn from the grid).
- During a blackout, the inverter will disconnect from the grid and generate its own frequency and voltage references.
  - The solar panels will continue to operate -- unless the battery is full.
  - If the battery is full, the inverter will stop converting DC-to-AC and the house will run purely off the battery. Once the battery is down to 90% (or whatever), the solar panels will start generating power again.
- If you're away during a summer blackout, your A/C is likely to drain the battery before you return.

</pack>

---

## Residential Solar (subpanel)

<pack scale=1.00>

```mermaid
graph LR;
  direction LR;
  subgraph Unprotected
   direction TB
   Grid["Main North American Electrical Grid"]<-->Panel
   Panel["200 Amp Panel"]-->AC["Air Conditioning"]
   Panel-->Load2["Most lighting"]
   Panel-->Load3["Electric Water Heater"]
  end
  SolarPanels<-->Inverter["DC-to-AC Inverter and management"]<-->CriticalLoad
  Inverter<-->Unprotected
  Battery<-->Inverter
  subgraph CriticalLoad["Critical Load Sub-grid"]
   direction TB
   SubPanel["60 Amp Critical Load Panel"]-->Furnace["Furnace and Blower"]
   SubPanel-->Den["Den and Turtle"]
   SubPanel-->WiFi
   SubPanel-->Refrigerators
   SubPanel-->Kitchen["Kitchen outlets"]
  end
```

- In normal operation, priority of solar power use:
  - 1st -- Sub-panel load
  - 2nd -- Recharging battery
  - 3rd -- Unprotected panel load
  - 4th -- Excess to Grid

</pack>

---

```mermaid
mindmap
 Solar
  No battery
   Super simple wiring
   No power during blackouts
   Only manual load shifting
   Highest grid power use
  Whole house battery
   Simple wiring
   Least grid power use
   Optimized for short blackouts
    Unless you manually shut down all noncritical loads
  Critical load battery
   Complicated and inflexible wiring
   Most resilient during long blackouts
```
