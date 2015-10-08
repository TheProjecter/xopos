### Intro ###
  * For the sound i use open source software named supercollider (http://supercollider.sourceforge.net/).
  * SuperCollider communicate with openframeworks through OSC protocol.
### Notes ###
  * The concept is to make the figure capture from camera sound interactive.
  * For this i use noise synth which produce noise, depending from the blod centroid velocity. It's  not very good the result. The problem is that is not recognizable the sound result. Also we have big changes at the position of the centroid of the blob(appear/disappear) and this produce big velocity (loud noise).
  * It will be interesting to collect the value of blob area (surface) and put in a Synth.
  * Next step is activate another two noise synth (with different characteristics) interactive with the left and the rigth limit of the blob.
### Synths ###
  * Stagones (inspired from batuhan(http://www.batuhanbozkurt.com/))
```
(
SynthDef( "stagones",
{ arg freq = 40, tempo = 1.5, out = 0, pan =0, amp=0,
mulDelay = 2, roundStart = 2e-3, roundEnd = 4e-3;
var source;
source = AllpassC.ar( SinOsc.ar(freq).tanh, //AllpassC.ar(in, ...)
0.4, // maxdelaytime,
TExpRand.ar(2e-4, 0.4,
Impulse.ar(tempo)).round([roundStart, roundEnd]), //delaytime,
mulDelay,
0.1); // mul,
Out.ar( out, Pan2.ar(source, 0, amp));

}).send(s);
)
```
### Debugging ###
From sketch class at openframeworks
```
void Sketch::sound(float xL, float yL) {
	for (int i=0; i<stoixeia; i++){
		if (i==0){
			deltaX[i] = (xL - xi[i]);
			deltaY[i] = (yL - yi[i]);
			ofxOscMessage m;
			m.setAddress( "/ampXar" );
			
			delta = ofMap(ABS(deltaX[0])+ABS(deltaY[0]), 0, 540, 0, 0.9  );
			m.addFloatArg( delta);
			sender.sendMessage( m );			
		}
		else {
			deltaX[i] = (xi[i-1]-xi[i]);
			deltaY[i] = (yi[i-1]-yi[i]);
		}
		
		deltaX[i] *= elastikotita[i];    // create elastikotita effect
		deltaY[i] *= elastikotita[i];
		epitaxinsiX[i] += deltaX[i];
		epitaxinsiY[i] += deltaY[i];
		xi[i] += epitaxinsiX[i];// move it
		yi[i] += epitaxinsiY[i];
		epitaxinsiX[i] *= aposbesi[i];    // slow down elastikotita
		epitaxinsiY[i] *= aposbesi[i];
	}
}
```
We calculate the velocity of CENTROID. After that we map this value
```
delta = ofMap(ABS(deltaX[0])+ABS(deltaY[0]), 0, 540, 0, 0.9  );
```
and we send it at supecollider (the value 540 come test (`printf(" %f \n", ABS(deltaX[0])+ABS(deltaY[0]));	`)).


### Code ###
```
x = Synth(\Xaraktiki);
(
SynthDef("Xaraktiki",
{ arg amp = 0, pan = 0, out = 0, panlevel = 0;
var source;
var panned_source;
source = FreeVerb.ar(HPF.ar(BrownNoise.ar(amp, 0), MouseX.kr(100,1000)), 0.2, 0.25);
panned_source = Pan2.ar(source, MouseX.kr(-0.9, 0.9));
Out.ar( out, panned_source);
}
).send(s);
)
OSCresponder(nil, "/ampXar", { | time, resp, message |x.set("amp", message[1]);}).add;
```