| System title | Fuel system |
|---|---|
| Purpose of system | To store fuel in tanks and to transfer fuel from tank to tank whilst measuring the quantity of fuel on board, and to provide a continuous flow of fuel to the engines. Fuel is often used as a thermal sink for aircraft heat loads, both on- and off-engine, e.g. fuel-cooled oil cooler. |
| Description | A collection of fuel tanks, fuel gauge probes, interconnecting pipes, and couplings, together with pumps, valves, fuel gauge probes, and level sensors. |
| Safety/integrity aspects | Safety critical system. Some architectures may dictate multiple transfer paths and multiple lane control electronics. Intrinsic safety to be considered because of fire or fuel vapour explosion risk leading to the need for nitrogen inerting systems, particularly on composite airframes. Safety considerations apply to ground refuelling and maintenance to reduce the risk of fuel/air vapour explosion. |
| Key integration aspects | Control can be integrated in the utility/vehicle management system or the IMA. Integration with FCS for management of aircraft centre of gravity. Heat exchangers to make use of fuel as a heat sink for engine oil, hydraulic loads, and avionics cooling. |
| Key interfaces | Propulsion system, ground refuelling, air-to-air refuelling, pilot’s displays, and warning system. |
| Key design drivers | Range/endurance, gauging accuracy, safety. |
| Modelling | 3D modelling (e.g. Catia) to model tanks shapes. Computational fluid dynamics to model fuel flow and slosh characteristics. FloMaster, Bond Graph models available, Matlab. |
| References | Moir and Seabridge, (2008), Langton et al. (2009, 2010), Langton (2010), Schutte Jeff et al. (2016), Bomanj and Hendricks (2016), Freeh (2016). |
| Sizing considerations | Main components boost pumps, transfer pumps, gauge probes, transfer and shut off valves, fuel pipes, tanks, fuel mass. Fuel as a source of cooling for engine oil, hydraulic fluid, and avionics cooling. |
| Future considerations | Environmental (green) issues are driving consideration of alternative fuels such as hydrogen, liquid/natural gas, and bio-fuels. These will influence installation interfaces and cockpit indications as well as storage at airfields. Impact on intrinsic safety. In-flight refuelling has been discussed to reduce take-off mass and fuel burn. |

| System title | Electrical power generation and distribution |
|---|---|
| Purpose of system | To provide a source of regulated AC and DC power to the aircraft systems via bus-bars and circuit protection devices. |
| Description | AC generators powered by engine off-take, generator control units, batteries, bus bars and feeders, and load protection devices (fuses, circuit breakers, electrical power controllers). |
| Safety/integrity aspects | Safety critical, multiple redundant system, failure propagation protection. |