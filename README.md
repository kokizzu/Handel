# Handel

Handel is a procedural programming language for writting songs in browser. 

The Handel Interpreter interprets Handel programs and plays compositions in browser, thanks to [Tone.js](https://tonejs.github.io/).

Try the Handel Web Editor here: [Handel Web Editor](https://ddj231.github.io/Handel-Web-Editor/)

See the language documentation (including latest features): [Handel Documentation](https://ddj231.github.io/Handel-Documentation/)

Join the [Handel forum](https://groups.google.com/g/handel-pl) to ask questions and showcase compositions. Also check out the [Contributing](./Contributing.md) guidelines.

*soli deo gloria*

# Installation

Add the below to your html document: 
```
<script src="https://unpkg.com/handel-pl"></script>
```

You're all set!

**Alternatively** 

Install Handel with the following: 

```
npm i handel-pl
```

And import Handel with the following:

```
import * as Handel from 'handel-pl';
```


# Usage

## Example Handel Snippet

```
start
    chunk example 
        play E3, C3, G3 for 2b
    endchunk
    run example with sound piano, loop for 5 
finish
```

See the Examples folder [here](./Examples/) for example Handel programs and inspiration.

## Example Using Handel In Browser

```
function clicked(){
    Handel.RunHandel(`
        start
            chunk example using somePlayable 
                play somePlayable 
                rest for 1b
            endchunk
            save myPlayable = Eb3 for 1b
            run example using myPlayable with sound piano, loop for 5 
        finish
    `)
}
document.addEventListener("click", clicked);
```

Note that you pass the Handel code into the **RunHandel** function ```Handel.RunHandel(someHandelCode)```.

To compile to midi, pass a config object to the RunHandel function with **outputMidi** set to true. 

```
const config = {outputMidi: true};
Handel.RunHandel(`start play E4 for 1b finish`, config);
```

Additionally, you can use the **StopHandel** function to stop a running Handel program. ```Handel.StopHandel()```


# Getting started

Handel programs are contained within the **start** and **finish** keywords. Below is a complete Handel program:

```
start
    play E4 for 1b
finish
```

The program above only plays 1 note. But it's a start!


# Let's play something

You can be play notes and chords using the play command. Below is an example program that plays a note, then a chord:

```
start
    play C#3 for 1b
    play E3, G3, A4 for 1b
finish
```

Note the syntax above. A **play** command begins with the **play** keyword, then a note or chord (a list of notes separated by commas) follows.

Lastly play commands need a duration. The play commands above end with 'for 1b'. This states how long the particular note or notelist (chord) should be held.  

Phew! We're getting somewhere.


# Let's rest

Similar to the play command, a rest can played using the rest command. Below is an example program that rests for 1 beat then plays a note for 2 beats.

```
start
    rest for 1b
    play G5 for 2b
finish
```


# But are there Variables?

tl;dr
Here is a code snippet showing variables in Handel 

```
start
save mynotelist = Cb3, D3
save myduration = for 1b
save myplayable = E4, F4, G3 for 3b

save myotherplayable = mynotelist for myduration

play myplayable
rest myduration
play myotherplayable
finish
```

You can declare Variables in Handel. Variables store three builtin types in Handel: Notelists, Notegroups, Durations, Playables.

A **Digit** is a positive or negative integer.

A **Notelist** is a single note name, or a list of note names separated by commas.

For example:

```
Bb3
G#2, A2
```

A **Notegroup** is a group of notelists (or conceptually an array of notelists). Each notelist is separated by a vertical line.

For example:

```
Bb3|G#2, A2
```

A **Duration** is the keyword **for** followed by a beat.

Here are some example durations: 

```
for 1b
for 2b
for 16b
for 32b
```

Lastly, we've already seen **Playables** above. Playables are a note or notelist (chord) followed by a duration.
Here are some example playables.

```
Bb3 for 1b
D#6, E#6, G3 for 1b
```

*no promises that the above chord sounds pleasing to the ear :p*

Finally variables!

To store a notelist, playable or a duration use the **save** keyword, followed by a variable name, an equal sign and a notelist, playable, duration (or another variable which stores on of these values).

Variable names must contain only lowercase letters, and no numbers. Variable names must also not be any of the reserved keywords in Handel. eSee the [Reserved Keywords](https://ddj231.github.io/Handel-Documentation/docs/keywords)). 

Below is an example program using variables.

```
start
    save mynote = E2
    save myplayablenote = mynote for 2b
    save myrest = for 2b

    play myplayablenote
    rest myrest
    play myplayablenote
    rest myrest
finish
```

When saving variables, Handel now also provides expressions for **generating random numbers** and for **evaluating expressions**.

To generate a random number when setting a variable, use the ```randint``` keyword, followed by a range ```start to end```

To evaluate an expression use the ```eval``` keyword followed by a mathematical expression (note division is integer division).

Here is an example of the syntax:

```
save somerandomdigit = randint -5 to 5
save someint = eval 5 * 5 / (1 + 1) % 6
```

OK! So far so good!

# Variable Reassignment
Handel now supports variable reassignment. Variables can be reassigned using the ```update``` keyword.

For example:

```
save mynotelist = B3
update mynotelist = Bb3
```

## Reassignment and Shifting values

All variables in Handel can be shifted using the ```rshift``` and ```lshift``` keywords. You can think of this as the equivalent of ```+=``` and ```-=``` respectively. 

For variables storing digits, this shifting is exactly the equivalent of addition and subtraction.

For variables storing notelists and notegroups, shifting changes the notes, left or right by a number of semitones.

The following example reassigns (or shifts) ```mynotelist`` left by one semitone. Then right by two semitones.

```
start
    save mynotelist = B3 

    update mynotelist lshift 1
    play mynotelist for 1b

    save somenum = randint 1 to 5
    update mynotelist rshift somenum 
    play mynotelist for 1b
finish
```

Variables storing **Durations** and **Playables** can also be shifted. 

Shifting a duration increases or decreases its beat value.

Shifting a playable (which contains a notelist) increases or decreass its notelist.

For example:

```
start
    save duration = for 1b
    save playable = E3, G3 for 2b
    
    update duration rshift 1 
    update playable lshift 2 

    play playable
    rest duration
    play playable
finish
```

# Blocks loops

Handel supports block loops.  Block loops begin with the **block** keyword and end with the **endblock** keyword and a ```loop for digit``` or ```loop while condition``` customization.

Here is an example two block loops in Handel.
```
start
    block 
        play C3, E3, G3 for 1b 
        play D3, F3, A3 for 1b 
    endblock loop for 2 

    save note = C2
    block 
        play note for 1b 
        update note rshift 1
    endblock loop while note lessthan C3
finish
```

Block loops are blocking (no pun intended), and should not be confused with Handel's procedures (chunks).

More on procedures below.

# Conditionals (if - else blocks)
Though booleans are not built in types in Handel, Handel now supports conditonals. ```> < >= <= ==```

The syntax for an if - else block is as follows.

```
start
    if E4 > Cb3 then
        play E4 for 1b 
    else
        play Cb3 for 1b
    endif

    save mydigit = 5
    if mydigit == 5 then
        play C2 for 5b
    endif
finish
```

The above plays E4 for 1 beat. Note that else blocks are optional.

# Procedures (I thought this was a procedural programming language?)

Procedures in Handel are called chunks. You can conceptualize a **chunk** as a song track. When ran,
chunks play at the same time as other run chunks and the global track. Chunks must begin with the **chunk**
keyword and end with the **endchunk** keyword.

Below is an example program with a kick drum and a piano, playing together.

```
start
    chunk backbeat using myplayable
        play myplayable 
    endchunk

    chunk mykeys 
        play E3, G3, A3 for 1b
        play G3, A2, C3 for 1b
        play F3, A3, C3 for 1b
        play D3, F2, A3 for 1b
    endchunk

    run mykeys with sound piano, loop for 2

    save myplayable = A1 for 1b

    run backbeat using myplayable with sound kick, loop for 8
finish
```

Both the 'backbeat' chunk and the 'mykeys' chunk above play together (not one after the other). This
behavior allows multitrack songs to be created with Handel. 

Note that each chunk has its own scope.


# More on procedures (chunks) and their syntax


## Procedure declaration (creating chunks) 

As noted above you can create chunks with the **chunk** keyword. The name of the **chunk** (the chunk name) follows the keyword.

This chunk name must be all lowercase letters, no numbers and cannot be one of Handel's reserved keywords. (See the [Reserved Keywords](https://ddj231.github.io/Handel-Documentation/docs/keywords)). 

After the chunk name, you can optionally add parameters. A list of comma separated parameters can follow the **using** keyword.

Together you get the following: `chunk somechunkname using someparam, anotherparam`

After the optional parameter list, you can add a body to the chunk. This is a function body (what you would like to happen when the chunk is ran).

Lastly the chunk must be ended with the **endchunk** keyword.


## Running Procedures 

There are two ways to run a chunk in Handel. 

You can run a chunk using the **run** keyword. This conceptually creates a new song track, and plays the chunk synchronously with all running chunks.

You can also run a chunk using the **call** keyword. This runs the chunk in place (in the songtrack the chunk is called in).

The syntax for running a chunk is the **run** or **call** command followed by the name of the chunk. 

If the chunk has parameters, a you must use the ```using`` keyword followed by a matching number of comma separated arguments. 

Here is an example running two chunks. One chunk requires arguments the other does not.

```
start
    chunk playtwo using argone, argtwo
        play argone
        play argtwo
    endchunk

    chunk noargs
        play C4 for 1b
        call playtwo using E4 for 1b, Cb6 for 1b 
    endchunk

    run noargs with sound piano
finish
```

The run command is used to run noargs in its own conceptual song track. 

Within (```noargs```), 1 note plays, and the playtwo chunk is called in place.

Note that saved variables (containing any built-in type in Handel), digits, playables, durations, can be used as arguments when running a chunk.

OK! Now to configuring a run of a chunk.


## Configuring a run of a chunk

You can configure a run of chunk by adding the **with** keyword and a comma separated list of customizations to the end of a run command. (customizations cannot be used with the call command as the chunk is being played in place)

There are three main customizations: **bpm**, **sound**, and **loop**.

You can use **bpm** keyword to set the bpm of a run of a chunk.

For example ```bpm 120```

You can use the **sound** keyword to set the instrument of a run of a chunk.

For example ```sound piano```

The current available sounds to choose from are: piano, synth, casio, kick, snare, hihat

You can use the **loop** keyword to set the amount of times the run of a chunk shoud loop for.

For example ```loop for 10```

All together you can configure a run of a chunk as follows:

```
start
    chunk withargs using somechord 
        play somechord 
    endchunk

    run withargs using E3, G3, F3 for 1b with bpm 100, loop for 8, sound piano 
finish
```

Above we've got a chord, played with a piano, looping 8 times, with a bpm of 100!

(see the [Language Features reference](https://ddj231.github.io/Handel-Documentation/docs/language-features) for additional customizations)

## Custom Instruments
Handel allows custom instruments to be loaded into Handel Programs. Instruments can be created and added to a run of a Handel program as follows.

```
let myinst = Handel.MakeInstrument({
    A1: 'https://tonejs.github.io/audio/casio/A1.mp3', 
    A2: 'https://tonejs.github.io/audio/casio/A2.mp3'
})
let config = {}
config.instruments = {funkyinst: myinst} 
Handel.RunHandel(`
    start
        load funkyinst as funky 
        chunk example 
            play E4 for 4b
        endchunk
        run example with sound funky
    finish
`, config)
```

The ```MakeInstrument``` function wraps Tone.js's sampler constructor. It takes a urls object as its argument. This urls object, maps note names matched to their location (locally or not). One or more mappings can be used.

After making an instrument above, we add it to our config object and run the Handel program with that config.

Within the Handel program we load the instrument as follows: ```
load configInstrumentName as nameOfInstrumentWithinHandel``` 

**Note**: this feature makes your Handel program less portable but gives you the freedom of using arbitrary instruments in your Handel program.

# Inspiration

[Sonic Pi](https://sonic-pi.net)

[Tone.js](https://tonejs.github.io)
