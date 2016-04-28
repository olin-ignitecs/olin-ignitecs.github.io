# Arduino basic functions
<div id=1></div>
## Preface
<div id=2></div>


### Background knowlege
<div id=3></div>

An Arduino is a device that you can program with your computer to interface with the 
real world. It can run motors, blink lights, communicate with sensors, and talk to 
other devices. With a couple addons, they can do some incredible things, like run 
robots or even your home.

Arduinos are programmed in a subset of the `C` programming language. C is a 
*low-level language* meaning it takes longer to understand and it's a bit harder to 
progam in than a *high-level language* like Python. However, it does have some 
advantages that make devices like the Arduino possible. `C` is very close to 
hardware, meaning it's very good at controlling the hardware to do exactly what you 
want. It's also *very* fast, and takes very few resources to run, which is why cheap 
platforms like the Arduino exist, as they can use less powerful hardware to run.

### Additional resources
<div id=4></div>

For more complex and comprehensive resources, there are some excellent 
[books](https://hassanolity.files.wordpress.com/2013/11/the_c_programming_language_2.pdf) 
and [online tutorials](http://c.learncodethehardway.org/book/).
If you're ever stuck, or something is not working the way you expect it to, the 
[Ardino forums](http://forum.arduino.cc/) are a great place to search if someone has 
had your issue before (chances are they have) or to ask for help py posting your own 
thread.

## What you need to know to write `Arduino C`
<div id=5></div>

### Comments
<div id=6></div>

First off, we have the code comment. It is *not* actual code, it's just a marker 
that tells the computer to ignore some text. This is useful to write some plain old 
english, often to explain what some bit of code is so when you look at it later, you 
don't forget. Like little notes to yourslf. It can also be used to remove code when 
you want to keep it around, but not have it run.
There are two kinds of comments, single and multi-line. Single line comments start 
with `\\` and make everything after them until the end of the line a comment.
Multiline comments can span multiple lines, and start with a `*/`, and end with a 
`*/`. This is usually for commenting out large blocks of text.

For example:

    /*
    A novice was trying to fix a broken Lisp machine by turning the power off and 
on.
    Knight, seeing what the student was doing, spoke sternly: “You cannot fix a 
machine by just power-cycling it with no understanding of what is going wrong.”
    Knight turned the machine off and on.
    The machine worked.
    */

    // This too, is a comment

### Variables
<div id=7></div>

Variables are ways of storing data in a symbol or label. You can think of it like a 
box that holds something, for example an integer. Variables have to be declared 
before they can be used, along with their data type, so the computer knows what kind 
of data to put in the box.

For example, if I want to keep track of how many cows I own, I can declare a new 
variable `cows`, and set it to the number of cows I have, say 5. I can do this with:

    int cows;
    cows = 5;

or in one line with

    int cows = 5;

The `int` at the beginning of the line tells the computer that `cows` can only hold 
something of type `int`, which are integers in `C`. It will fail and the computer 
will complain with an error message about mismatched types if we give it something 
that's not an integer.

Another important thing about variables is that they can *vary*. It's in the name! 
So now if I get another cow, I have to tell the computer that. I can do that very 
simply with:

    cows = 6;

It's important to note that whenever the computer now sees the word `cows` in code, 
it will interpret it as the variables value. So if we tell the computer to print 
(teach them how to print maybe?) the value of `cows`, it will tell us 6.

### Arduino specific functions
<div id=8></div>

#### `pinMode`
<div id=9></div>

Now that we know a little bit about variables and comments, we can learn about 
functions that the Arduino uses. 

One of the first you'll encounter is `pinMode`. `pinMode` in general usage is 
combined weth a variable like this:

    int LED_pin = 13;
    pinMode(LED_pin, OUTPUT);

As you can see, we made a new variable `LED_pin` and assigned it to the integer 
`13`. We then took that variable and told `pinMode` to use it, as well as the word 
`OUTPUT`, with the second line. The word `OUTPUT` is a *constant*, which is the same 
as a variable in that it can hold some data, but different in that it can't change, 
and you tell it to hold a value a little differently.
The `pinMode` word there is actually a **function call**, a concept which we'll get 
into a litte later, that takes two **arguments**, in this case a number (supplied by 
the variable `LED_pin` in this case) and a constant, either `INPUT` or `OUTPUT`.

This function sets up the pin whose number is the first argument to be an input or 
an output. That's why we used the variable `LED_pin` there, to make it easier to 
make it easier to understand, read, and most importantly reuse when we need to use 
it again.

It matters if the pin is an output or an input for us to be able to use two other 
functions, `digitalWrite` and `digitalRead`, respectively.

#### `digitalWrite` and `digitalRead`
<div id=10></div>

First off, a warning. **`digitalWrite` can only be used if the `pinMode` for that 
pin has been set to `OUTPUT`, and `digitalRead` can only be used if it has been set 
to `INPUT`** 

This is the purpose of `pinMode`, to constrain if we can read or write to that pin.

With that said, let's explain what they both do. Both functions interact with pins 
that have been previously been told if they are an input pin or an output pin with 
`pinMode`. `digitalWrite` is called like this:

    int LED_pin = 13;
    pinMode(LED_pin, OUTPUT);
    digitalWrite(LED_pin, HIGH);
    digitalWrite(LED_pin, LOW);

We can see here that it takes a pin number and a constant, `HIGH` or `LOW`. This 
sets the pin on or off, respectively. Now if we hook pin 13 up to the long lead of 
the LED, and the short lead to ground, we wold see......nothing! That's because C is 
really fast, so it turns the LED on and then off after a really short period of time 
because both statements run pretty fast. This is why functions like `delay` exist, 
explained in the next section.

`digitalRead` can be called like this:


    int button_pin = 12;
    int LED_pin = 13;

    int val; //did not assign a value to this variable, because the value will be 
assigned later
    
    pinMode(button_pin, INPUT);
    pinMode(LED_pin, OUTPUT);

    val = digitalRead(button_pin)
    digitalWrite(LED_pin, val); 

The key line in this is the `val = digitalRead(button_pin)`, which assigns the state 
of the button (pressed or not) to `val`, and then sets the LED to that value. 

This means, for example, that if we had the button pulled down to ground with a pull 
down resistor and when it's pushed, it will complete the circuit with a 5 volt 
source. If we pushed the button and ran this code, the LED would stay on, but if we 
didn't push the button, it wouldn't turn on.

##### Optional
<div id=11></div>

Interestingly enough, this code gives us some insight into what `HIGH` and `LOW` 
actually are. since we declared `val` as only being able to hold integers, and we 
set the value of `LED_pin` with `digitalWrite`, which takes either `HIGH` or `LOW`, 
we can infer that `HIGH` and `LOW` are actually integers!

Sure enough, `HIGH` is 1 and `LOW` is 0. Binary numbers!

#### `delay`
<div id=12></div>

`delay` makes it possible to make the program run *slower* by making the program 
pause. We need it to run slower because `C` can run much faster than we can 
percieve. This is a good thing, since it's much easier to slow things down than 
speed them up.

`delay` is called like this:

    int LED_pin = 13;
    pinMode(LED_pin, OUTPUT);
    digitalWrite(LED_pin, HIGH);
    delay(1000);
    digitalWrite(LED_pin, LOW);

so it's the same example as before, but now you can see the LED actually blink! It 
stays on for a second (`delay` takes a number of millieconds to do nothing) and then 
turns off.

### Higher order functions and constructs
<div id=13></div>

#### `if`, `else`, and `else if` statements
<div id=14></div>

`if` statements are a crucial part of computer logic. It's what allows the computer 
to decide what course of action to take. A quick example of this in the context of 
the concepts already introduced is

    int LED_pin = 13;
    int delay = 1000; // A once second delay
    
    pinMode(LED_pin, OUTPUT);

    if (delay >= 1000) {
        delay = 500;
    }
