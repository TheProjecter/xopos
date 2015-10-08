### Introduction ###
We have to play some buffers.
The samples that i use they are here: http://igoumeninja.org/files/audio/samples/
and you have to put them at this direction:
openframeworks/of/apps/fou/xopos100126/SuperCollider/samples

### Arguments ###
  * t\_trig     : trig the synth                   (startValue = 0)
  * level      : amplitude level                  (startValue = 1)
  * bufnum     : the number of the buffer         (startValue = 0)
  * freqShift  : frequency modulation             (startValue = 1)
  * pitchRatio : pitch modulation                 (startValue = 1)
  * rate       : change the rate of the buffer    (startValue = 1)

### Interaction ###
  * Depending the centroidX :
  * Depending the area of the blob :
  * ...
### SuperCollider Code ###
```
(
SynthDef("PlayBuf",
	{ arg out=0, t_trig=0, amp=0, bufnum, delayTime, freqShift, pitchRatio, rate;
	Out.ar(out,Pan2.ar(
		PitchShift.ar(FreqShift.ar(PlayBuf.ar(1, bufnum, rate, trigger: t_trig, loop: 1.0),freqShift), 0.02, pitchRatio, 0, 0.004), 0, amp));
}).send(s);
)
//-----------------------------------------------------------------------//
//-----------------------------------------------------------------------//
// Control synth
b.set(\amp, 0)
b.set(\bufnum, 5)
b.set(\rate,1)
b.set(\t_trig,1)
s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 3);
//--------------------------------------------------------------------------------------------------//
//--------------------------------------------------------------------------------------------------//
// responders
OSCresponder(nil, "/synthesis",     	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 0);}).add;
OSCresponder(nil, "/gongGranular",  	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 1);}).add;
OSCresponder(nil, "/ice",		{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 2);}).add;
OSCresponder(nil, "/birdGranular",  	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 3);}).add;
OSCresponder(nil, "/clarinetTapes", 	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 4);}).add;
OSCresponder(nil, "/xylophonoGranular", { s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 5);}).add;
OSCresponder(nil, "/tablaGranular", 	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 6);}).add;
OSCresponder(nil, "/ross",  		{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 7);}).add;
OSCresponder(nil, "/sculptor", 		{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 8);}).add;
OSCresponder(nil, "/fluxon",		{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 9);}).add;
OSCresponder(nil, "/pictorAlpha", 	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 10);}).add;
OSCresponder(nil, "/metalGranular",  	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 11);}).add;
OSCresponder(nil, "/gong",  		{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 12);}).add;
OSCresponder(nil, "/purity", 	 	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 13);}).add;
OSCresponder(nil, "/stakato", 	 	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 14);}).add;
OSCresponder(nil, "/clarinet",	  	{ s.sendMsg("/n_set", 1001, "t_trig", 1, "bufnum", 15);}).add;
//--------------------------------------------------------------------------------------------------//
//--------------------------------------------------------------------------------------------------//
//	Buffers
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/sythesis.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/gongGranular.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/ice.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/birdGranular.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/clarinetTapes.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/xylophonoGranular.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/tablaGranular.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/ross.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/sculptor.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/fluxon.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/pictorAlpha.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/metalGranular.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/gong.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/purity.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/stakato.aiff");
Buffer.read(s,"/Users/fou/RealWork/openframeworks/of/apps/fou/xopos100126/SuperCollider/samples/clarinet.aiff");
//--------------------------------------------------------------------------------------------------//
//--------------------------------------------------------------------------------------------------//
```
### C++ openframeworks communicatoion ###
At testApp.cpp we call
```
sketch[0].buffers(1);
```
After that we use switch function and pbufnum value(previous bufnum) for no play the sample many times.
```
//----------------------------------------------------------------------------------------------------//
//----------------------------------------------------------------------------------------------------//
//	Buffers
void Sketch::buffers(int bufnum)	{	// is the same but only one value 
	//printf(" %i \n", bufnum);						
	if (bufnum != pbufnum )	{
		ofxOscMessage m;		
    switch (bufnum){
        case 0:
			m.setAddress( "/synthesis" );
			sender.sendMessage( m );			
            break;
		case 1:
			m.setAddress( "/gongGranular" );
			sender.sendMessage( m );			
            break;
		case 2:
			m.setAddress( "/ice" );
			sender.sendMessage( m );			
            break;
		case 3:
			m.setAddress( "/birdGranular" );
			sender.sendMessage( m );			
            break;
		case 4:
			m.setAddress( "/clarinetTapes" );
			sender.sendMessage( m );			
            break;
		case 5:
			m.setAddress( "/xylophonoGranular" );
			sender.sendMessage( m );			
            break;
		case 6:
			m.setAddress( "/tablaGranular" );
			sender.sendMessage( m );			
            break;
		case 7:
			m.setAddress( "/ross" );
			sender.sendMessage( m );			
            break;
		case 8:
			m.setAddress( "/sculptor" );
			sender.sendMessage( m );			
            break;
		case 9:
			m.setAddress( "/fluxon" );
			sender.sendMessage( m );			
            break;
		case 10:
			m.setAddress( "/pictorAlpha" );
			sender.sendMessage( m );			
            break;
		case 11:
			m.setAddress( "/metalGranular" );
			sender.sendMessage( m );			
            break;
		case 12:
			m.setAddress( "/gong" );
			sender.sendMessage( m );			
            break;
		case 13:
			m.setAddress( "/purity" );
			sender.sendMessage( m );			
            break;
		case 14:
			m.setAddress( "/stakato" );
			sender.sendMessage( m );			
            break;
		case 15:
			m.setAddress( "/clarinet" );
			sender.sendMessage( m );			
            break;
		}
	}	
	pbufnum = bufnum;	
}
```