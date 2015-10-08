### Introduction ###
A class for drawing lines based on sound input & output.
Amplitude and pitch detection (input & output)  on SC.

### Code ###
```
/* IZ - AB 091228
A class for drawing lines based on sound input & output. 
Amplitude and pitch detection (input & output)  on SC.

Drawing is done on openframeworks.

Usage:
With this class we want to detect frequency and amplitude from input and output
send the informations through OSC messages. 
How we make this:
			i.  init: 	-	define server
						-	define address
						-	channel
						- 	initTrigID
						-	makeResponders	
							-	makeAudioResp,	
			ii. start:	- 	SynthDef	"SendAmpPitch"
							-	detect amp and freq 200 times/sec
							-	send messages
						-	play "SendAmpPitch"	

Sketchdraw.start;
x = Synth("SendAmpPitch");
x.set(\)
y = Synth(\SendAmpPitch, [, \chan, \ampTrig, 3, \freqTrig, 4]);
*/

Sketchdraw {
	classvar default;
	var <server;	// the scserver that runs the listening process
	var <synthListen;		// the listening process
	var <synthPlay;		// the produce process
	var <addr;		// the address (p5, of ...) for sending the data for drawing
	var <chan = 0;	// the channel that we detect
	var <responders;	//	responders 

	*default {
		if (default.isNil) { default = this.new };  // greate default
		^default;
	}

	*start { this.default.start; }
	
	*stop { this.default.stop }
	
	*new { | server, addr, chan = 0 |
		^super.new.init(server, addr, chan);	//new.init
	}
	
	// start initialize	
	init { | argServer, argAddr, argChan = 0 |
		server = argServer ?? { Server.default };  //define server
		addr =  argAddr ?? { NetAddr("127.0.0.1", 12345); }; //localhost, p5 port
		chan = argChan;
		this.makeResponders;	// call makeResponders
	}
	// end initialize

	makeResponders {
		responders = [
			this.makeAudioResp		//call makeAudioResp
		];
	}
	
	makeAudioResp {
		^OSCresponder(server.addr, '/tr',{ arg time,responder,msg;
//			[time, responder, msg].postln;
			switch(msg[2], 	//Help -> control structures
				//ampTrigID, { addr.postln.sendMsg("/amp", *msg[2..]); },  // Assignment Statements  
				//ampTrigID, { addr.sendMsg("/amp", *msg[2..]); },  // Assignment Statements  
				//freqTrigID, {  addr.sendMsg("/freq", *msg[2..]); }       // implement this for many channels
				//old way
				//ampTrigID, { addr.sendMsg("/amp", msg[3]); },  // temp usage for one chan
				//freqTrigID, {  addr.sendMsg("/freq", msg[3]); }   // temp usage for one chan
				1, { addr.sendMsg("/ampIn", msg[3]); },  // temp usage for one chan
				2, { addr.sendMsg("/freqIn", msg[3]); },   // temp usage for one chan
				3, { addr.sendMsg("/ampOut", msg[3]); },  // temp usage for one chan
				4, {  addr.sendMsg("/freqOut", msg[3]); }   // temp usage for one chan

			);
		});
	}
	
	
	
	start {
		responders do: _.add;	//.add all the responders
		if (not(server.serverRunning)) { server.boot };
		server.doWhenBooted {	// doWhenBooted very good method
			synthListen = SynthDef("SendAmpPitch",{ | chan = 8, ampTrig = 1, freqTrig = 2 |
				var trig, in, amp, freq, hasFreq;
				trig = Impulse.kr(200);
				//in = SoundIn.ar(chan);
				in = In.ar(chan);
				amp = Amplitude.kr(in, 0.001, 0.001);
				#freq, hasFreq = Pitch.kr(in, ampThreshold: 0.02, median: 1);
				SendTrig.kr(trig, ampTrig, amp);  //sending back the message from server to client
				SendTrig.kr(trig, freqTrig, freq);  //sending back the message from server to client
			}).send(server);

//			// from http://igoumeninja.org/files/sc/last0912041920.html
//			synthPlay = SynthDef("SendAmpPitch",{ | chan = 8, ampTrig = 1, freqTrig = 2 |
//				var trig, in, amp, freq, hasFreq;
//				trig = Impulse.kr(200);
//				//in = SoundIn.ar(chan);
//				in = In.ar(chan);
//				amp = Amplitude.kr(in, 0.001, 0.001);
//				#freq, hasFreq = Pitch.kr(in, ampThreshold: 0.02, median: 1);
//				SendTrig.kr(trig, ampTrig, amp);  //sending back the message from server to client
//				SendTrig.kr(trig, freqTrig, freq);  //sending back the message from server to client
//			}).send(server);

		};
	}

	stop {
		responders do: _.remove;
		synthListen.free;
	}
}


```