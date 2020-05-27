This is a project to design an open source FIE-spec wireless fencing system, at a reasonable price. Also part of this project is a (initially basic) scoreboard and tournament management system

Current SITREP:  The basic radio idea works - onwards towards a functional prototype. I have an idea of using some commodity ISM transceivers (at very low power or attenuated) to send and detect the radio signals. These transceivers have a feature called RSSI (Received Signal Strength Indicator) which could be used to measure the property described below. This would eliminate a large amount of circuitry, although there may be issues with RSSI sampling rate and resolution etc. 

# Why Wireless Fencing
## Cost 
Fencing is expensive: Equipment for the Fencer isn't too bad (£250 will get you sorted with equipment for Sabre - the most expensive weapon). Equipment for *fencing* (i.e. the fencing club)
can be extremely expensive. To fence properly you need Spools (About £300 per Fencer), a scoring machine (Anywhere from £300 for a toy scoring box to several thousand to a nice one), and some cables to connect everything together (Maybe another £200). On top of this, if you want to host a competition you'll want a conductive piste to Fence on (This will cost at least a few thousand new).

So, we've spent all this money, we have a working Fencing system - life is good. Unfortunately, Fencing is a sport: All the spools and cables take a beating over time. They break. The sheer volume of wiring used represents a threat to a working fencing system. 

If we can eliminate the wires, we can eliminate a large proportion of the cost presented above (a conductive piste is unfortunately non-negotiable unless one replaced it with some kind of camera system) *and* reduce the risk of damage via enthusiastic fencing. 

## Freedom
Depending on the particular spool and wire in use, there may be a noticeable tugging on the Fencer. This is less than ideal - obviously, eliminating the wires eliminates the resistance to the Fencers movement. This is now the norm at high-level international competitions (Perhaps best emphasized by the London 2012 Olympics).

## Some Realistic Discussion of Wireless Fencing
The Elephant in the room, is this: A Traditional electric system is (in principle) simple. In terms of design, one doesn't have to worry about difficulties of detection or latency. If your Equipment is good, it'll work. This is unfortunately not quite the case with Wireless fencing yet. The technology works, but it is still (relatively speaking) in its infancy. 

### Open Source Hardware
As I shall explain below, all existing systems on the market use the same detection method (Capacitive Sensing), but there are potentially better alternatives. On top of this, it may be possible to better implement this tried and tested method - this design will be fully, open, and sufficiently flexible as to allow new firmware to be developed after shipping i.e. the touch detection can improve as more data is collected and analyzed.
# How does a wired Fencing System work?
As every fencer knows, a fencing bodywire has three pins. In my experience, not everyone knows which pin does what or how the electronics work exactly. 
Firstly, the pins (at the spool end) are usually labeled A, B, and C such that the smallest (15 mm) spacing is between A and B.

For foil the connections are specified as follows (from the FIE Material Rules): 
```
At the spool end the three-pin male plug, which must comply with the
conditions of manufacture and assembly laid down in Article m.55, will be
attached to the wires in the following manner:
    — the pin at 15 mm from the centre pin to the conductive jacket;
    — the central pin to the wire in the weapon;
    — the pin at 20 mm from the centre pin to the foil earth circuit or the
      conductive piste.
```

For Epee:
```
At the spool end, a three-pin male plug is connected to the wire as follows:
    a) the pin 15 mm from the centre pin to whichever wire is most directly connected
       to the pointe d’arrêt;
    b) the centre pin to the other wire on the épée;
    c) the pin 20 mm from the centre pin to the épée’s earth circuit and to the
       conductive piste.
```

Without going into the exact details of the circuitry and timing, hits are detected when different connections are broken and made. The FIE Material Rules also contain specifications of the exact resistances of each electrical component used.

For Foil, a valid touch occurs when current through the C line stops and current flows through the opponent's Lame (their A line). Off-targets occur when the C line current stops but no current flows to either the strip or the Lame. If current is flowing to the the strip then no light is displayed (off-target hits can only be scored against the other fencer rather than the floor).

For Epee, rather than having a continuous current like in Foil there are two wires going to the Tip. When the Tip Depresses, the A and B lines are connected - as long as the current cannot pass through the grounded elements (the guard, the piste etc.) a valid hit is indicated. 
# Wireless Fencing Methods
## Why is wireless fencing difficult?
Throughout the previous description, you'll note that this relies on electricity flowing from one end to another. Electricity will only flow back to where it came from. See the problem yet? 
It would be nice to just fire electricity down the wires and look for it on the other side but unfortunately this is not possible. 

Detecting a electrical contact without electricity is a nasty problem - but one that can be solved. Another problem (although nowhere near as hard) is dealing with latency correctly, to be compliant with FIE regulations basically requires a resolution of at least 1ms. Depending on the distribution of wireless latency it may necessary to include redundant timing actually in the sensor packs.

## Capacitive Sensing
A Capacitor is a device that stores electrical energy in a electric field. If you charge a capacitor and a resistor to a certain energy, it will discharge with a certain time. One can measure this to infer the size of the capacitor (Technically the RC constant). 

A lump of metal on the end of a stick acts like a capacitor (it's a bit more complicated than this but that's the principle). The capacitance of whatever is on the end of the wire (it could be just a weapon, two weapons touching, a weapon touching the piste etc.) can be measured and then compared against a known value.

This system is how all existing wireless fencing systems on the market work (that I know of), when they work they are good but they are only flawless when you pay thousands of pounds for an olympic-grade system from StM. This system is more effective because they require fencers wear conductive t-shirts (this makes the capacitive fencing easier to determine accurately) - this is feasible at international competitions but not for most clubs (and fencers).

We are currently conducting tests on using this. Existing systems may have already found the limit with this technique but without data we do not know. If this can be made to work it would be the cheapest.
## Radio Frequency Methods - Here be dragons!
Previously I said you can't just send electricity down a wire. That was a lie (Sorry). 

Electricity starts to behave in weird ways at high speeds (Speed in this case meaning frequency, i.e. Your mains electricity is at 60 oscillations per second but your WiFi runs at 2.4 Billion oscillations per second). For example, if you send a pulse down a wire it can be reflected back towards you. Things start to radiate electromagnetic waves (everything becomes an antenna at a certain frequency). We can use this to our advantage in some ways (that I will describe below). It is not yet known whether all of these work (i.e. the theory is sound but fencing equipment isn't exactly common in RF engineering books - the effect may not be measurable via anything remotely cheap or distinguishable from background noise).

### Antenna based detection
Applying a sufficiently high frequency to the weapon will cause it to radiate. This signal can be detected by using the opponents Lame as a receiver. Hopefully the received signal will be sufficiently stronger during direct contact that this can be easily distinguished. If this was the case, then the system could be almost perfectly reliable. Another added benefit would be a much more flexible system when it comes to equipment - it's much easier to determine whether a radio signal is loud than rely on a potentially noisy previously unseen capacitive measurement.

This has not yet been tested (stay tuned). 

### Frequency Domain Reflectometry
How do you know if your antenna works at a given frequency? Preferably without measuring it externally? The answer is by measuring how much of the signal comes back. Some power is radiated, some is reflected back, and some is lost as heat (although this is a very small power unless you are running a broadcast tower for example). The exact proportion of this depends on the frequency of the signal put in the antenna e.g. For example, you want your WiFi antenna to be very sensitive to WiFi signals but not (say) cellular communication signals.

One way of quantifying this is called return loss - this is a logarithmic measure of how much energy comes back relative to how much you put in (0dB means it all came back). A Device called a network analyzer sweeps across the frequency domain and produces a plot of return loss (against said frequency). This can be used to characterize the device under test, i.e. in our case a weapon (or weapon + lame, or weapon + piste etc.).

This is slightly difficult to test as Network Analyzers can *easily* cost as much as a house. Pleasantly, tests with a very cheap Network Analyzer have shown that this effect can be exploited.
There is a measurable difference between weapon and weapon and lame. 

A disadvantage of this technique is that once we have this information, how do we make this identification with a computer - it's not as simple as On/Off like in a wired system or a certain number being high (capacitance or the method previously). 

### Time Domain Reflectometry 
If you send a very sharp (picoseconds to nanoseconds) pulse down a cable, it propagates along wire until it will reflect off the end. By measuring what comes back (like the previous method only done very quickly), you will see the original pulse followed by reflections from imperfections or "crossings" (the technical term here is impedance mismatch) between wires.

When the weapon is on it's own, you would expect to see a series of pulses returning at around 20nS to 40nS indicating reflections from the tip of the blade. When the blade is touching something else there would be significant additional pulses (or more realistically a messy lump) at later. As with the previous method, this data would be used to characterize the weapon and/or lame combination which would then used to distinguish different combinations. 






