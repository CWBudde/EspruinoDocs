<!--- Copyright (c) 2018 Gordon Williams, Pur3 Ltd. See the file LICENSE for copying permission. -->
Troubleshooting
=============

<span style="color:red">:warning: **Please view the correctly rendered version of this page at https://www.espruino.com/Troubleshooting. Links, lists, videos, search, and other features will not work correctly when viewed on GitHub** :warning:</span>

* KEYWORDS: Troubleshooting,Trouble,Problems,Help,Broken,Not Working

What follows is a quick list of potential problems and solutions. If your problem isn't covered here, please post in the [Forum](http://forum.espruino.com).

**Do you have a Bluetooth LE device like [Puck.js](/Puck.js) or [Pixl.js](/Pixl.js)?** Check out the [Bluetooth specific troubleshooting page](/Troubleshooting+BLE)

* APPEND_TOC

Getting Started
===============

## My board doesn't seem to be recognized by my computer

### On the Espruino Pico/WiFi Boards

Hold down the button, and then plug the board in while keeping it held (then release after connecting):

* **If Red and Green light brightly for a fraction of a second, then they start 'pulsing'** everything is fine - you're now in bootloader mode
* **If Red flashes briefly when plugging in**, the button wasn't pressed, or isn't working.
* **If nothing lights up at all**, there is a bad USB cable/connection, so no power
* **If Red and Green stay lit brightly** the bootloader has started, but no USB connection could be made. This could be due to a bad cable (try plugging the board in directly), or if that doesn't work it could be drivers - see the next troubleshooting headings.
* **Green lights for a second, then red flashes and both go out** - the board has booted into Espruino without loading saved code. This happens when the button is held down for >2 secs after booting. To enter the bootloader, you need to release the button less than a second after applying power.


### On the Original Espruino Board

Hold down the RST button. Do the blue and red lights glow dimly? If not, there's a problem with your USB cable as power isn't getting to Espruino.

Hold down BTN1, and then press and release RST while keeping BTN1 held. The Blue and Red LEDs should now light brightly for a fraction of a second, and the Blue LED should be pulsing. If not, there is some issue with USB. Try another USB cable (it's surprising how often this is at fault, *even if it works for other devices*) and if that doesn't work, see the next troubleshooting headings.

### On a Bluetooth LE device

Check out the [Puck.js troubleshooting page](/Puck.js#troubleshooting)

### On other boards

If you've bought a different board, it won't come pre-installed with Espruino. You'll have to go to the [[Download]] page and follow the instructions there in order to flash the correct software onto it.


## My board doesn't appear as a USB Serial port in Windows

Windows versions before 10 don't come with the correct drivers preinstalled. You'll need to install [ST's VCP drivers](/Quick+Start+USB#plugging-in) first.

**NOTE:** It's not enough to just open the ZIP file and run the installer, you have to then go to the installation directory and run the correct installer as well. See the readme file in the ZIP above for more information.

## My board shows with an error in Windows Device Manager

* If it's a 'no driver' message, please try installing the drivers referenced above.
* Try another USB cable - it could be an electrical problem
* If this happens for you with an older Espruino Pico in Windows 10 [see this thread](http://forum.espruino.com/conversations/328895) - this doesn't apply to any other Espruino boards.

## My board doesn't appear in Windows Control Panel's 'Devices and Printers' page.

If you use many COM port devices in Windows, you may find that the COM port numbers quickly get so high that Windows refuses to add more. If this is the case, you'll have to follow the instructions here: [http://superuser.com/questions/408976/how-do-i-clean-up-com-ports-in-use]

If not, see the first troubleshooting item above.


## In Windows, the COM port appears in the Web IDE, but I can't connect to it

This can sometimes occur if you've reset or unplugged the Espruino board while the Web IDE was connected to it. Chrome can hang onto the serial port and stops Espruino from reconnecting to it.

Try unplugging Espruino and then completely close Chrome (close all windows, not just the Web IDE). However if that doesn't work, try restarting Windows.

If you have never been able to connect, check whether there is an error in 'Device Manager' and see the FAQ item above.

## In Windows, Espruino was working and now it won't connect

See above.

## I can't get my board into bootloader mode

On official Espruino boards you enter bootloader mode by holding the button held down while resetting or powering them up.

On any of the newer boards (WiFi, Puck.js, etc) holding the button down for longer than a second or two at boot will cause the board to exit the bootloader and enter Espruino without loading any of the code that you might have saved to Flash memory (to allow you to easily recover if you wrote code that you didn't intend to). As a result, to enter the bootloader you need to make sure that you release the button as soon after applying power (or resetting) as you can.


## I tried to reflash my Espruino Board, and now it won't work

If you have Windows, check that it's not one of the problems described above.

| Board |   |
|-------|---|
| Pico/WiFi | Try reflashing again by holding down BTN1 as the board is plugged in. You should always be able to get the pulsing Green and Red LEDs. |
| Espruino Board | Try reflashing again by holding down BTN1 and pressing and releasing RST. You should always be able to get the pulsing Blue LED. |

As Espruino itself won't work, the IDE won't know what type of board it is supposed to flash so you'll have to choose the correct board from the list of available boards when flashing.


## My board appears as a mouse or joystick in Windows Control Panel's 'Devices and Printers' page.

This may happen if you are using an ST Discovery board and haven't yet installed the Espuino firmware. Some of these boards are automatically recognized by Windows as a completely different kind of device (because of the 'demo software' that comes installed). Install the firmware as described on the Download page, disconnect the board a reconnect it again.

Using Espruino
==============


## I just upgraded Espruino to the latest version, and now I'm getting `ReferenceError` messages

This is a new feature in Espruino that detects the use of previously undefined variables - it makes it much more likely that your code will work as intended.

In order to fix the error, either create the variable beforehand with `var`, or just assign a value to it. For instance this code worked before Espruino 1v81, but won't work now:

```
setInterval(function() {
  on = !on;
  LED1.write(on);
}, 500);
```

To fix it, simply define `on`:

```
var  on = false;
setInterval(function() {
  on = !on;
  LED1.write(on);
}, 500);
```


## Espruino keeps responding `=undefined` to my commands

This is actually fine - Espruino writes what your command returned, so if you execute a command that doesn't return a value, `=undefined` gets returned.


## I typed a command and Espruino says `Function --- not found!`

Have you got the capitalisation correct?

 JavaScript is case sensitive, so `digitalWrite(LED1,1)` will work, but commands like `DigitalWrite(LED1,1)` or `digitalwrite(LED1,1)` won't.


## I typed `save()` and it succeeded, but my code isn't loaded at power on

You could try typing `dump()` to see if your code has actually been saved. If it hasn't, it's possible that **BTN1** or the pin it is connected to was held down while Espruino boots (as this stops Espruino from loading saved code).

You could try typing `load()` to force Espruino to load its saved program.

If you want to execute certain commands at power-on, add one or more event handlers for the `init` event on `E`:

```
E.on('init', function() {
  // ...
});
```

This will then be loaded every time Espruino starts up. If you create a function called `onInit`, that will be executed too:

```
function onInit() {
 // ...
}
```

But of course there can only be one `onInit` function, so if you copy and paste two bits of code with two `onInit` functions then the second function will overwrite the first.


## Espruino stopped working after I typed `save()`

You might have written some code that stops Espruino from working, and Espruino loads it at power on and breaks itself each time. To stop this:


### On Espruino Pico/WiFi

* Unplug the board from USB
* Plug the board in and immediately press the button (but not *before* you've plugged the board in)
* Wait 2 seconds
* Release the button

If you get pulsing Red/Green LEDs, it's because you actually pressed the button too soon. Try again and leave a bit more of a gap.

### On Original Espruino Board

* Press the **RST** button
* Release the **RST** button and immediately press **BTN1**
* Wait 2 seconds
* Release **BTN1**

If you get a glowing blue LED, it's because you pressed **BTN1** too quickly after pressing **RST**. Try again and leave a bit more of a gap.

### Puck.js

See [the instructions on the Puck.js page](/Puck.js#i-saved-some-code-and-my-puck-js-no-longer-works)

### Finally

This will make Espruino start without loading your saved code. You can then connect with the Web IDE and type `save()` to overwrite your saved program with the 'empty' state that Espruino is now in.

There's [a video of how to do this on the original Espruino board here](https://www.youtube.com/watch?v=N4ueQTHDrcs)


## When powered on, Espruino just shows a pulsing LED (blue on the Original Espruino board, or red/green LEDs on the Pico/WiFi)

This is the Espruino Bootloader. It starts on the Espruino Board when *BTN1* or the pin it is connected to is held down while the reset button is released.

To enter normal mode, just:

| Board |    |
|-------|----|
| Pico/WiFi | Unplug from USB and re-plug, without pressing the button. |
| Espruino Board | Press and release **RST** while **BTN1** is not pressed. |
| Puck.js | Remove and re-insert the battery |

## When I upload code, some characters are being lost

Most likely this is because you're uploading code that is doing calculations
that are taking a long time to finish - and so Espruino is unable to process
the data that is being received quickly enough.

When you upload code to Espruino, it is executed as it is received (allowing
you to upload more code than might otherwise fit into RAM).

In many cases, you will actually want the code to run *at power on* rather
than when you upload (eg. WiFi connection or LCD initialisation). In these
cases you could put your code inside a function called `onInit`:

```
// variables
// function declarations

function onInit() {
  // initialisation of hardware
}

// Call onInit right after upload (for testing)
setTimeout(onInit, 1000);
```

When saving, you may then want to remove the `setTimeout` line,
upload, and then save (unless you're sure that calling `onInit` twice
will not cause problems). See [the page on Saving](/Saving) for more
information.


## I typed `save()` but my connected device doesn't work at power on

Some devices (such as LCDs and WiFi) require their own initialisation code which Espruino can't remember. To do that initialisation at boot time, write a function called `onInit` which contains the initialisation code for your device. After typing `save()`, it will be executed at power on or reset.

See [the page on Saving](/Saving) for more information.


## I typed `save()` but Espruino won't work (or stops working quickly) when powered from a computer (it only works from a USB power supply, battery, or the computer when the Web IDE is running)

This is because you're printing information to the console.

When you are not connected to a computer via USB, Espruino automatically moves the console (left-hand side of the IDE) to the Serial port. However when you are connected to a computer, Espruino writes down USB. **If no terminal application is running on your computer** then your computer won't accept any incoming data down USB, and Espruino can't tell whether that is because the IDE is connected but busy, or because no app is running at all. Espruino won't throw away any of the data that you send down USB, so when Espruino fills up its output buffer, it stops and waits for the computer to accept the data, and this is what causes your program not to work.

To fix this, either remove your `console.log` and `print` statements, or explicitly set the console to be on another device at startup with `function onInit() { Serial1.setConsole(true); }`. The second option will mean that you will no longer be able to program Espruino from USB unless you reset it or you code calls `USB.setConsole();` to move the console back.

A second option is to use simply call `Serial1.setConsole();` (without `true` as an argument). This will move the console to Serial1 *until USB is connected again*. Running this from `onInit` may not work, since `onInit` will likely be called at startup *before the USB connection is made with your PC*. In that case, the console will move to `Serial1`, but just a fraction of a second later, USB will connect and the console will move to USB. Instead, you could do:

```
function onInit() {
  setTimeout(function() { Serial1.setConsole(); }, 1000);
  // ...
}
```

This will call `setConsole` 1 second after boot, by which time USB should have initialised. Assuming your Espruino is battery powered, unplugging and replugging USB will then move the console back to USB, where the board can be programmed.

**Note:** Serial ports are not generally as fast as USB - 9600 baud is only around 1000 characters/second. If your code writes a lot of data to the console then you may find it fills up the output buffer and ends up stalled waiting for characters to send. 

**Note:** We're using `Serial1` and `USB` in the examples here, but depending on the board you have, you may also have access to other devices like `Serial2/3/4` or `Bluetooth`. Check out [the `Serial` class reference](/Reference#Serial) for a full list of options.


<a name="console"></a>
## Espruino works when connected to a computer, but stops when powered from something else

Do you have a Serial device connected to pins `B6`/`B7` on the [Pico](/Pico) or [WiFi](/WiFi), or `A9`/`A10` on the [Original Espruino](/Original)? When disconnected from USB, Espruino's REPL/'console' (what's on the left-hand side of the Web IDE) gets moved over to `Serial1` - which is on those pins. If you've got a [Serial](/USART) device on those pins then it will then stop working.

To fix this, right at the top of your code that runs on initialisation (eg. the `onInit` function or `E.on('init', ...)` event), add the line `USB.setConsole()`, which will force the console onto USB. After `USB.setConsole()`, the console can still automatically move back to `Serial1` if there is some event that forces it (USB connect and disconnect), so you can *force* the console to stay on USB regardless with `USB.setConsole(true)` instead.

Other reasons could be that the power supply you're using isn't supplying enougn current or is too low a voltage.

To debug further, you can take advantage of the console that was mentioned before. Connect a USB-TTL converter to GND and the Serial1 pins (shown above) and connect with the Web IDE at 9600 baud. You can then interact with Espruino as if it was connected to USB - but while it is powered from another source.


## When I type `dump()` the code that is displayed isn't exactly the code that I entered

When you send code to Espruino, it is executed immediately. That means if you say:

```
var foo = 1+2;
```

Then `foo` will forever be set to `3`, and **not** the expression `1+2`. In this case it's not a problem, but you might write `var foo = analogRead(A0);` and then `foo` will be set to whatever the value of A0 was *when you uploaded the program*.

When you type `dump()`, Espruino tries to reconstruct the code that you wrote based on its current state (by adding `setWatch`, `setInterval`, `digitalWrite`, etc) - and sometimes there is not enough information available for it to get it correct.

It's most noticeable for `setInterval` and `setWatch`, which return integers - so Espruino has no way of knowing that a given number goes with a given `setInterval`. In this case:

```
var x = setInterval("print('Hello')", 10000);
```

will turn into:

```
var x = 1;
setInterval("print('Hello')", 10000);
```

To get around this, it's best to put code that you intend to run every time Espruino starts into a function called `onInit()`.

**Note:** The problem with `setInterval` happens because Espruino is trying to turn its internal state back into a human readable form. If you just type `save()` then the correct state will still be saved.


## I've pasted code into the left-hand side of the Web IDE and it doesn't work

There could be several reasons for this, but the likely one is that you have formatted your code in a way that doesn't work well with a command-line interface.

Each time you press enter, Espruino's command-line interface counts brackets to see if the statement you've entered is complete. If it is, it'll try and execute it. For instance:

```
if (true) {
  console.log("Hello");
}
```

That is a complete statement, so when you hit enter at the end it'll be executed immediately. However if you type:

```
if (true) {
  console.log("Hello");
}
else {
  console.log("Oh No!");
}
```

Then now we have a problem. Halfway through, Espruino sees that the first statement is complete and executes, and it's now given a line that starts `else {` that isn't a valid statement.

The easiest way to fix this is to write code in the [K&R style](http://en.wikipedia.org/wiki/Indent_style#K.26R_style):

```
if (true) {
  console.log("Hello");
} else {
  console.log("Oh No!");
}
```

If you're writing code in the right-hand side of the Web IDE, the Web IDE should try and detect the different formatting and insert a special newline character (Alt-Enter) which will fix it for you. If you're using other tools to send data to Espruino then this may not automatically happen for you though.

**Note:** If you use `require(...)` on the left-hand side and the module is not already part of Espruino, you may find that you get a 'Module not found' error. In this case, you should add the `require(...)` command to the right-hand pane of the Web IDE and click `Send to Espruino`. This will automatically load the module into Espruino, allowing you to use it from the left hand side.


## I get `Uncaught Error: Module XYZ not found`

This could be because the module doesn't exist in [Espruino's list of modules](http://www.espruino.com/Modules) - Espruino doesn't directly support inclusion of modules from NPM, because the vast majority have too many dependencies to be used on an embedded target.

However, if you're trying to use example code for Espruino, chances are it's because you typed the command on the left-hand side of the IDE (see the last FAQ item). The majority of modules are loaded on-demand by the IDE (some are built-in) when code is uploaded, but the left-hand side of the IDE is a REPL - a direct connection to the Espruino device. In that case the IDE doesn't have a chance to intercept the command and automatically upload the module for you.


## I'm using an unofficial board and some of the examples don't work

This could be for several reasons:

* The pins and wiring in the examples are designed for the Espruino board. On other boards those pins often conflict with other on-board devices.
* Many of the other boards don't have enough memory for all the functionality of the Espruino Board, so things (such as Waveform, HTTP, and sometimes even Graphics) have had to be removed.
* As we only make any money from the Espruino Boards, we can't afford to spend time implementing functionality on other boards - so even if the board you have has enough memory, the functionality the example is using may still not be implemented.

In short, if you want to be sure that all the functionality you want is implemented, support us and [buy an Espruino board!](/Order)

Electrical
=========

## I've connected a [[Serial]] device like a GPS, but I'm not getting a signal.

For normal [[Serial]] communications, you have two wires - RX and TX. However these are relative to the device - for instance TX on the Espruino transmits data out of Espruino, and TX on a GPS transmits data out of the GPS. That means that for the devices to communicate you must connect RX to TX and vice versa.

[[Serial]] ports also have to have a communications speed (baud rate) set up - you'll need to make sure that each device has exactly the same baud rate or they won't be able to communicate. For devices such as a GPS, you should find the baud rate in the datasheet.

If you're still having problems, look at the next item too.


## I've connected an external device but I'm not getting a reliable signal

This could be because your devices aren't sharing the same ground. For example if you have [[Serial]] communications set up, two wires will get mentioned a lot - RX and TX. However you *also* need to have ground connected, meaning you actually need 3 wires. Ground is required for pretty much everything: [[Serial]], [[I2C]], [[SPI]], [[OneWire]], and even simple logic signals from something like a [[Pyroelectric]] sensor.

## I've connected an I2C device, but I get 'timeout' errors

This is usually one of four reasons:

* There are no pull-up resistors. I2C needs resistors between each of its data lines and 3.3v (or 5v) - these are usually in the 4-10 kOhm range. Many modules already have them in, but if you have a module without one then you need to add them before I2C will work.
* The device's address is incorrect - sometimes people quote the address as being twice as large as it actually is. If in doubt, try dividing the address by two or multiplying it by two.
* The device isn't powered or ground isn't shared (see the point above)
* The SDA and SCL wires are mixed up. SDA should be connected to SDA, and SCL to SCL.


Something else is wrong!
-------------------------

Check out the [Espruino Forums](http://forum.espruino.com/)

Is your device a Bluetooth device? Check out the [Bluetooth Troubleshooting page](/Troubleshooting+BLE)
