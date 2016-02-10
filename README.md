# ofxParameterTwister

Use MidiFighter Twister to quickly tweak parameter groups

# Mission Statement

This shall be a simple, drop in - drop out, utility to control and tweak parameters using the MidiFighter Twister.

ofxParameterTwister maps an ofParameterGroup of up to 16 floats or bool parameters to a MidiFighter Twister Controller device. The mapping is bi-directional, so if you update your parameters using a gui, for example, you will see the values update on the twister instantly.

The emphasis is on ease and pleasure of use.

# Use
	
.h file:

	pal::Kontrol::ofxParameterTwister mTwister;
	
	ofParameter<float> 	mParam1 {"First  Param", 0.f, 0.f, 1.0f};
	ofParameter<bool> 	mParam2 {"Second Param", false};
	ofParameter<float> 	mParam3 {"Third  Param", 0.f, 0.f, 1.0f};

	ofParameterGroup params {
		"Parameters",
		mParam1,
		mParam2,
		mParam3,
	};

.cpp file:

	mTwister.setup();

	// automatically sets up twister to track up to 16 parameters in params
	// you can use setParams to hot-swap parameter groups into the twister.
  
	mTwister.setParams(params);	

	// in update()

	mMidiController.update(); // any parameters currently tracked by twister will get updated here from midi values

	// in draw();

	ofSetColor(ofColor::white);
	ofDrawBitmapString(mParam1, 10, 10);
	ofDrawBitmapString(mParam2, 10, 30);
	ofDrawBitmapString(mParam3, 10, 40);



## Goals: 

* include & use this addon writing less than 10 lines of code 
* accept `ofParameterGroup` of up to 16 paramters
	* assign an `ofParameterGroup` to the addon object, and these parameters become midi-controlled
	* allow hotswapping of `ofParameterGroup` s
* parameter type is auto-detected and auto-mapped:
	1) `float` --> map to rotary control
	2) `bool`  --> map to switch control
* current parameter state shows on the midiFighter twister, and state is maintained in sync throughout.
* unused parameters controllers indicator LEDs are kept in distinctly different state compared to active ones.
* upper banks shall be used to fine-tune parameters, in deltas of 1/2 range max over 128, 1/4 range over 128, 1/16 range over 127, so each higher bank should give you double precision.
* the addon is self-contained
* the addon is confirmed running on Windows
* the addon is confirmed running on OS X 

## Midi message structure:

Input: We're expecting our midi messges to arrive as CC messages.

'CC' stands for continous controller.

CC messages have the (most significant) first half-byte ("nibble") set to B, so anything xB... is a continous controller message.

byte 0 .. controller message / channel number

rotary controller values arrive on channel 0,
switch controller values arrive on channel 1.

The least significant, or the second half-byte is the channel
on which the signal comes in.

CC messages have two parameter bytes:

byte 1 .. controller id
byte 2 .. controller value

# Wishlist

|Priority | Task|
|---------|-----|
|B        | re-eastablish midi connection if lost|
|B        | make sure to not crash when midi connection is lost|

# Dependencies

* openFrameworks >= 0.9.2
* On OS X, make sure to link your app against the *CoreMIDI.framework* (do this in "Build Phases -> Link Binary With Libraries")

# Known isssues

There is a reported firmware issue in the current (and previous) version of the MidiFighter Twister hardware which affects "button"="switch" behaviour.

When button activated state is written back to the device using CC commands, this appears not to update the internal button state if the button has been programmed to toggle.  

I reported the issue on February 5, 2016, and am told DJ Techtools are looking into it. 

# License 

<pre>
     _____    ___     
    /    /   /  /     ofxParameterTwister
   /  __/ * /  /__    (c) ponies & light, 2016. 
  /__/     /_____/    poniesandlight.co.uk

  ofxParameterTwister
  Created by @tgfrerer Feb 2016.
  
 Permission is hereby granted, free of charge, to any person obtaining a copy
 of this software and associated documentation files (the "Software"), to deal
 in the Software without restriction, including without limitation the rights
 to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 copies of the Software, and to permit persons to whom the Software is
 furnished to do so, subject to the following conditions:
 
 The above copyright notice and this permission notice shall be included in
 all copies or substantial portions of the Software.
 
 THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 THE SOFTWARE.
</pre>

<pre>

    RtMidi: realtime MIDI i/o C++ classes
    Copyright (c) 2003-2014 Gary P. Scavone
    RtMidi WWW site: http://music.mcgill.ca/~gary/rtmidi/

    For details on RtMidi's license see RtMidi.h
</pre>
