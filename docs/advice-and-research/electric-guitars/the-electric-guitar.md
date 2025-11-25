# The Electric Guitar

The electric guitar is a guitar instrument that produces an acoustic noise when strings are plucked, but usually is used
to produce an electrical noise through an electromagnetic circuit.

Purchase a guitar based on the following factors:

- How it feels to play
- What it's tone sounds like ([see below](#guitar-tone)).
- How it looks, which is very subjective, so I won't cover here.

Personally, I believe tone and feel is most important, and getting looks right is secondary.

## Guitar Tone

Based off of Jim Lill's experiments and my own guesses (observation & rudimentary knowledge of physics), I'm going to
say that the guitar affects tone in the following ways:

- Play style / capability.
- Strings.
- Pickup type (Single Coil vs Humbucker vs P90).
- Pickup position (Bridge, Middle, Neck).
- Pickup height / distance to strings.
- Pickup slant angle.
- Tuning.
- Scale length (distance between guitar bridge and nut).
- Any electronics built into the guitar (even if they're just present in the circuit).

Some people will argue the wood used in the guitar will affect tone. I've watched enough comparison videos through
decent headphones and speakers to think: "If there's a difference, I either can't hear it, or it's small enough that I
don't think it really matters".

Other things like [pedals](pedals-and-pedalboards.md) and [amplifiers & cabinets](amplifiers-and-speaker-cabinets.md)
also affect tone.

TODO: Go into some / all of these aspects and describe how different choices affect tone.

## Guitar Play Feel

How a guitar feels to play I find is affected by:

- Weight
- Scale length.
- Neck width & shape.
- Fret height & style.
- Shape of body.
- Wood finish.
- Height of strings.
- String type.
- Position of electronics on the body (do they get in the way when playing).
- Ease of use of electronics.
- Bridge type (affects weight, ease of restringing and ability to use a whammy bar (often misnamed a "tremolo", since
  it's actually vibrato)).
- Presence of "whammy" (vibrato) bar.
- Tuning peg position (affecting ease of tuning).
- Presence of a locking nut (makes tuning more reliable).

TODO: Go into some / all of these aspects and describe how different choices affect feel.

## Glossary

| Term                  | Meaning                                                                                                 |
|-----------------------|---------------------------------------------------------------------------------------------------------|
| Body                  | The main large piece of wood that rests on your own body and holds the pickups, electronics and bridge. |
| Neck                  | The long bit of wood with metal frets in it where you choose what pitch/note you want to play.          |
| Frets                 | The pieces of metal on the neck that make choosing the note you want to play easier.                    |
| Fretboard             | An (optional) piece of extra wood built into the neck, dedicated for the fretboard.                     |
| Head / Headstock      | The far end of the guitar, where tuning pegs and brand logos are found.                                 |
| Bridge                | The metal device on the body that holds the strings in place at the body end.                           |
| Nut                   | The piece of (usually plastic) at the end of the neck that holds your strings in place at the neck end. |
| Tuning Pegs           | The metal devices on the headstock that allow you to tune each string.                                  |
| Pickups               | The electronic devices that "pick up" (actually creates) the guitar signal from the strings.            |
| Strings               | The long strands of metal (some of them made differently) that produce the tone.                        |
| Pots / Potentiometers | The "knobs" on the guitar that allow controlling things like volume, tone or even blend of pickups.     |
| Jack                  | The electronic output socket on the guitar.                                                             |
| Tonewood              | The wood of the guitar that supposedly affects the tone of the sound produced (I'm sceptical of this).  |

## Basic electric guitar physics

- String is plucked / strummed / struck.
- String wobbles.
- String is made of metal and so has an effect on nearby electromagnetic forces.
- Pickup is a variable reluctance sensors (sort of "reverse electromagnet").
- Pickup detects movement of metal strings and creates an alternating current (AC).
	- This alternating current will have a frequencies (how often the current changes direction) and two amplitudes (how
	  big the voltage change is and how big the current change is).
	- In electric guitar circuitry (pedals, amplifiers, etc.), we mostly care about frequency and the amplitude of the
	  voltage changes - which we can represent as a sine-wave.
	- Variances in the sine wave (harmonies, fluctuations) give the signal a "tone" (rather than just sounding like a
	  pure sine-wave sound).
	- We often refer to this alternating current output from the pickup as the "signal".
- Electrical and electronic components affect this signal in different ways to produce a different tone and amplify the
  signal for it to be heard through speakers.
