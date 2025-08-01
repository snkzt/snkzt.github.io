---
layout: default
title: "The Stress of a Connecting Flight"
permalink: /posts/flight-time-and-gate/
date: 2025-07-09
---

2025-07-09

Sometimes you find yourself needing to run for a connecting flight.  
I'm not a big fan of connecting flights or running.  
As I’ve been travelling to small cities more often recently, I experienced a few instances and even lost an entire day being stuck at an airport due to a flight delay.  
Each time, I was extremely stressed during the original flight and had to run hard upon arrival at the connecting airport.  
Sometimes running helped; other times, it didn’t.

So this time, when I couldn't find a direct flight but only a tight connecting flight, I decided to find a way to prepare myself and access accurate live information so that I wouldn’t waste mental and physical energy on unnecessary stress and running.  


## What information do I need?
- Latest estimate departure and arrival time
- Latest gates (departure and arrival) at the connecting airport
- Location of gates  


## Where to get the information?
First, I visited the airline and the connecting airport webpage to find:

1. Historical data on expected and actual departure and arrival times for the flight  
-> Only found expected departure and arrival time of the flight, no update until end of the flight  
2. Gate ID of today's flight  
-> Only found the one for the original flight and no connecting airport gate provided
3. Location of the gate  
-> Found on the connecting airport web map  

I found flight information aggregators that provide live and historical data for flights.
- [FLIGHTSTATS BY CIRIUM](https://www.flightstats.com/v2/flight-tracker/search) provides Gate information.
- [FlightAware](https://www.flightaware.com/) shows history in a nice table format.

## Departure and Arrival time
While checking several flight information aggregators, I noticed that there are two different types of times used for both departure and arrival.  
For example, when website X shows 18:11 as departure, Y shows 18:21.  
According to each site, it depends on the specific time they are referring to.  
X uses **Gate times** and Y uses **Runway times**.  
Some use both.  

- **Gate times**: When an aircraft starts and stops moving (Gate-OUT/Gate-IN, Off-block/On-block)  
- **Runway times**: When wheels of an aircraft are off and touch the ground (Wheels-off/Wheels-on)  

As my upcoming flight is with Lufthansa, I turned to Lufthansa's [developer site](https://developer.lufthansa.com/page) (account creation required) to find out what they use for their departure and arrival time.  

According to [IATA Worldwide Airport Slot Guidelines (WASG) Edition 3](https://www.iata.org/contentassets/4ede2aabfcc14a55919e468054d714fe/wasg-edition-3-english-version.pdf), `Planned operating times are based on the planned Off-block (departure) and On-block (arrival) times`. Runway times are supplemental information.  

Lufthansa also follows IATA [Movement-message (MVT) spec](https://xwiki.avinor.no/display/AASRV/IATA+messages+and+explanations), and uses Gate times for its departure and arrival time ([API doc](https://developer.lufthansa.com/docs/read/api_details/notifications/FlightUpdate)).  
So the departure and arrival times on our tickets refer to when the aircraft starts and stops moving.  

When calling the [FlightStatus](https://developer.lufthansa.com/docs/read/api_details/operations/Flight_Status_Response) endpoint, it also provides gate information as long as the information is set.

`flightstatus` response for `GET /operations/flightstatus/{flightNumber}/{date}`
![LH2221_flightstatus](/assets/images/LH2221_flightstatus.png)



## Data flow for Lufthansa flight information
Trigger: A gate, delay, or time change occurs

Data flow: Airline Operation Control Centre ([AOCC](https://www.eurocontrol.int/sites/default/files/2025-04/eurocontrol-prc-apoc-study.pdf)) confirms the event and encodes the event in an industry-standard IATA operations message like [MVT/AIDX](https://www.iata.org/contentassets/badbfd2d36a74f12b021c9dd899ecbad/type_b_messaging_whitepaper_v2dot1_14_june_2024.pdf) and transmits it ➜ Lufthansa IOCC (Integrated Operations Control Centre) ingests the message ➜ LH ops DB update ➜ [FlightUpdate](https://developer.lufthansa.com/docs/read/api_details/notifications/FlightUpdate)(you need an account) MQTT push ➜ Subscribers such as user email/SMS/app receive the latest information 


## My flights for this time
```
Schedule
                        Departure  Gate   Arrival  Gate 
Origin airport          17:55      -      19:40    Kxx
Connecting airport      20:30      Kxx    21:20    -
```
For the last 5 days, the original flight arrives at the connecting airport earliest at 20:00, latest at 21:30.  

![LH2221_history_5d](/assets/images/LH2221_history_5d.png)


## How much time do I actually have?
We also need to consider:
- `Gate Close time (Final call)`: About 15 minutes before the departure (At least for Lufthansa)
    As my connecting flight departs at 20:30, I need to be at the gate by 20:15.

- `Deplaning time`: About 10 - 15 minutes
    The size of the plane and the number of the passengers affect this, but in this case, we’re only considering Schengen short-haul flights.  
    Even in the best-case scenario, it takes around 10 minutes, as I usually don’t book a luxury spot like a front-row seat.  
    So if we arrive like the last 5 days, the happy case is 20:00, I will be off the plane around 20:10.

- `Gate to gate time`: Totally depends on airport to airport  
    For my case, most of the original flights arrived on the same floor as the connecting flights' gate  
    which makes the moving time maximum 5 minutes from side to side.  
    In the worst-case scenario, the departure and arrival gates may be in different buildings, requiring a train ride to transfer between them.  
    This makes the gate to gate time about 15 minutes even if the transport goes smoothly.  
    For the happy case, arriving at the departure gate by 20:15, and worst case at 20:25.  

Therefore, the required connection time is approximately 10 - 30 minutes.  

When I called Lufthansa about my concern of missing a connecting flight based on the last several days flight history, they kept encouraging me by saying "you have 50 minutes for layover, so no worries." 
I then mentioned that the “50 minutes” refers to gate times — that is, the scheduled departure and arrival times of the plane — which do not account for gate closing, deplaning, or the time needed to get from one gate to another.    
What could they say? Nothing. "It will be alright dear customer. Why are you so pessimistic?"  
Why? Because I had a traumatic experience with you that ruined my family time at Christmas two years ago — an experience that still haunts me and almost made me go bald from the stress! (Of course I didn't say that. But hey.)

Anyway. What I can do is to check the latest departure and arrival gate and time once the plane lands at the connecting airport, to determine whether rushing is necessary or not.


## How did it go?
TL;DR: I made it to the connecting flight by running. I was the last to board.
```
Actual
                        Departure  Gate   Arrival  Gate 
Origin airport          18:25      -      20:00    G38
Connecting airport      20:28      K27    20:19    -
```

I was lucky that the original flight arrived by 20:00, and since my seat was sixth from the front (without any extra payment), deplaning took only a few minutes. Checking past combinations of departure and arrival gates also helped me estimate where to run and how long it might take.  
I caught the inter-terminal train just before the doors closed and arrived at the gate at 20:14.  
I was the last boarding passenger — even though they said it was a fully booked flight and offered free check-in for carry-on luggage.  
After that full-power run, it was nice to have all three seats to myself in a quiet cabin.


## In case a delay causes you to miss your flight
Under certain conditions, several types of compensation may be provided.  
For example, with Lufthansa, you can find about it on [the website](https://www.lufthansa.com/fr/en/passenger-rights) or talk to someone at the gate, help desk if there's any at the airport.

Ultimately, it’s best to allow plenty of buffer time when a direct flight isn’t an option.  
Also, the weather around MUC and FRA is highly changeable and affects flights a lot comparing to other European airports.
Avoiding these airports is one way to reduce the risk of unexpected issues.



<[Home](https://snkzt.github.io/)>