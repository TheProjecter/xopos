### Introduction ###
xopos project has to do with human figure. The best sensor to catch that is a camera.
### Available arguments ###
  * blob Points
  * centroidX, centroidY of the blob
  * blob area
  * exist blob or not
  * xmin, ymin, xmax, ymax of the blob
  * width, height of the blob
  * length of the blob

### C++ (openframeworks Code) ###
```
// from ofxCvContourFinder.cpp
		blobs[i].area                     = fabs(area);
		blobs[i].hole                     = area < 0 ? true : false;
		blobs[i].length 			      = cvArcLength(cvSeqBlobs[i]);
		blobs[i].boundingRect.x           = rect.x;
		blobs[i].boundingRect.y           = rect.y;
		blobs[i].boundingRect.width       = rect.width;
		blobs[i].boundingRect.height      = rect.height;
		blobs[i].centroid.x 			  = (myMoments->m10 / myMoments->m00);
		blobs[i].centroid.y 			  = (myMoments->m01 / myMoments->m00);
	//------------------------------------------------------------------------------------------------------//
	//------------------------------------------------------------------------------------------------------//
// from testApp.cpp
////	Centroid
	if (viewCentroid)	{
		for (int j = 0; j < contourFinder.nBlobs; j++){
//			printf(" %f \n", contourFinder.blobs[0].area);			//  BLOB AREA
//			printf(" %b \n", contourFinder.blobs[0].hole);			//  BLOB EXIST
			blobCentroidX = ofMap(contourFinder.blobs[j].centroid.x, 0, 320, 0, ofGetWidth());
			blobCentroidY = ofMap(contourFinder.blobs[j].centroid.y, 0, 240, 0, ofGetHeight());
			for( int i=31; i<40; i++ ) {
				sketch[i].drawCentroid(blobCentroidX, blobCentroidY, 0, r2, g2, b2, a2);	
				sketch[i].drawCentroid(blobCentroidX, blobCentroidY, 0, r2, g2, b2, a2);	
			}
			sketch[0].buffers(int(ofMap(contourFinder.blobs[j].centroid.x, 0, 320, 0, 15)));
			sketch[7].sound(blobCentroidX, blobCentroidY);	
		}
	}

	//------------------------------------------------------------------------------------------------------//
	//------------------------------------------------------------------------------------------------------//	
	// Blobs drawing and fill
	if (viewBlobOrFill)	{
		for( int i=0; i<(int)contourFinder.blobs.size(); i++ ) {
			for( int j=0; j<contourFinder.blobs[i].nPts; j++ ) {
				blobX = ofMap(contourFinder.blobs[i].pts[j].x, 0, 320, 0, ofGetWidth());
				blobY = ofMap(contourFinder.blobs[i].pts[j].y, 0, 240, 0, ofGetHeight());
				if	(viewBlob)	{
					sketch[0].update( blobX, blobY, 0, r1, g1, b1, a1);
					sketch[1].updateNormal( blobX, blobY, 0,  r1, g1, b1, a1);
				}
				if	(viewFill)	{
					blobCentroidX = ofMap(contourFinder.blobs[0].centroid.x, 0, 320, 0, ofGetWidth());
					blobCentroidY = ofMap(contourFinder.blobs[0].centroid.y, 0, 240, 0, ofGetHeight());
					sketch[2].updateFill( blobX, blobY, blobCentroidX, blobCentroidY,  r3, g3, b3, a3);
				}
			}
		}
	}
//------------------------------------------------------------------------------------------------------//
	//------------------------------------------------------------------------------------------------------//

```