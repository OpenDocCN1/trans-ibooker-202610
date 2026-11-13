
# Scenario-Based Information Flow

Taking into account the background information described in Part One, as well as the nature of internal and external IOC communication described in this chapter, the following sections will unfold from a full-practice perspective, analyzing three scenarios in detail. At this point, the reader should have acquired sufficient knowledge to understand the impact of airline operational disruptions on passengers, as well as the vast resources, professional skills, and capabilities mobilized by airlines to minimize consequences.

![](xhtml/images/0169.png)

# Weather Scenario: Snowfall at JFK Airport

## Figure 6.1 JFK Weather – Status @ 1700

```
UTC >>> 03 UTC >>> 03
350 seats
16 17 1215 JFK 1400 12 13 1645
707 13 14 1508 >>0230 DXB 0315 1545
840 09 10 11 12 2130 0900 LGW 2130 0650
715 716267 298 0925 CDG 2010 0500
811 329 MAINTENANCE LAX LAX
341 09 10 11 350 21 01 04 05 06
0822 23 00 01 02 07 14 15 16 17
1309 10 11 1213 1509 1411 19 20 19 20
16 17 18 14 15 10 16 17 18
1000 1630 807 277 JFK 03 <<
WBD WBA WBB WBC WBE 03 <<
702 03 DXB JFK 1800 2000 0230 2015
LAX 203 12 LAX 811 135 840 147
0245 21 02 03 04 05 1850
LHR JFK 0130 JFK 22 23 06 07
00 0430 SFO SFO SEA LGW 335
MAINTENANCE LGW 314 DXB 0700
326 1115 852 852 830 290 830 0330
<< ETA 1800
```

**Legend**
- All times in the scenario are in UTC.
- The timeline indicates the current scenario time.
- Prior to the timeline, all flights are operating as scheduled.
- Flight numbers are displayed on a dark background.
- Actual load factors are shown to the right of the flight numbers.
- Flights outside the time range are marked with `<<` or `>>`.
- Time zones for this scenario: LHR and LGW are UTC, CDG is UTC+1, DXB is UTC+4, JFK and CLE are UTC-5, ORD is UTC-6, SEA, SFO, and LAX are UTC-8.
- Fleet utilization does not simulate any existing airline; it is for illustrative purposes only.

![](xhtml/images/bgaa.jpg)

## 6 Weather Scenario: Snowfall at JFK Airport

### Introduction

The airline operates international services using five twin-engine wide-body long-range aircraft (registration/tail numbers `WBA`–`WBE`). Seating capacity is 350 seats. The minimum turnaround time between passenger flights is 90 minutes. Please refer to **Figure 6.1** on the facing page.

### Scenario Description

The current time is 1700 UTC. A blizzard is hitting New York JFK Airport in the USA; conditions have been critical since early morning. However, the airport remains open, and flights are still operating, although some domestic and regional carriers have suspended several flights into and out of JFK. Due to high regional traffic and the requirement for instrument approaches, several aircraft have been instructed to hold around JFK.

`WBB` is operating **Flight 811 (DXB–JFK)**, scheduled to arrive at JFK at 1800. Flight 811 departed DXB with sufficient fuel to hold over JFK for **3 hours**, with an alternate airport of **CLE (Cleveland, located 370 NM west of JFK)**. ATC has just advised the crew that a further **3 hours** of holding at JFK is anticipated, and that they will enter the holding pattern 80 NM east of JFK at 17:40.

![](xhtml/images/bgab.jpg)

### IOC Interaction

The JFK Station Manager has notified the IOC: Flight 811 is cleared to hold east of JFK for a maximum of 3 hours. No further information has been passed at this time.

The IOC now needs to confirm the following:

**a)** What is the cause of the unexpected extended holding time? (The flight plan already allowed for and planned weather holding; the focus is on the underlying reason for the *additional* holding.)

**b)** How are air and ground conditions expected to evolve? (Overall improvement or deterioration? Is the revised arrival time certain or merely possible?)

**c)** How much maximum holding time remains for the aircraft at JFK? (Given the additional holding, the aircraft's endurance is expected to be exceeded, immediately raising diversion concerns.)

**d)** If holding for 3 hours, which alternate airports are available? (Selecting a different alternate may allow the aircraft to continue holding over JFK longer while retaining the possibility of receiving an approach clearance.)

**e)** Are other aircraft being cleared for approach? (If conditions are not deteriorating, this serves as a good predictive indicator of a possible landing.)

**f)** Crew duty hours may also become an issue; a proactive inquiry to Crew Scheduling will determine the location of the nearest reserve crew should a diversion occur.

**g)** Should the aircraft divert immediately with the expectation of returning to JFK later, rather than waiting for the 3-hour hold to expire, then diverting and facing crew duty exhaustion?

**h)** Should the Flight 811 (JFK–LAX) crew remain at the hotel (delayed report) until it is confirmed the aircraft will indeed land? Please refer to **Figure 6.2** on the facing page.

**i)**

![](xhtml/images/0172.png)

### Figure 6.2 JFK Weather – Status @ 1740

```
UTC >>> 03 UTC >>> 03
350 seats
16 17 1215 JFK 1400 12 13 1645
707 13 14 1508 >>0230 DXB 0315 1545
840 09 10 11 12 2130 0900 LGW 2130 0650
715 716267 298 0925 CDG 2010 0500
811 329 MAINTENANCE LAX LAX
341 09 10 11 350 21 01 04 05 06
0822 23 00 01 02 07 14 15 16 17
1309 10 11 121509 1411 19 20 19 20
16 17 18 14 15 10 16 17 18
1000 1630 807 277 JFK 03 <<
WBD WBA WBB WBC WBE 03 <<
702 03 DXB 1800 2000 0230 2015
LAX 203 12 LAX 811 135 840 147
0245 2113 02 03 04 05 1850
LHR JFK 0130 JFK 22 23 06 07
00 0430 ETD 2230 JFK X
SFO SFO SEA LGW 335
MAINTENANCE LGW 314 DXB 0700
326 1115 852 852 830 290 830 0330
<< HOLD TO 2100
```
