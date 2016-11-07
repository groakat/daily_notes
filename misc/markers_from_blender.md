# Extract markers from Blender

```python
from __future__ import print_function
import bpy
import os

D = bpy.data

printFrameNums = True # include frame numbers in the csv file
relativeCoords = False # marker coords will be relative to the dimensions of the clip

output_folder = '/Volumes/Seagate_Backup_Plus_Drive/datasets/mech-sys/final_datasets/book/tracks/B_hom/'

f2=open(output_folder + 'export-markers.log', 'w')
print('First line test', file=f2)
for clip in D.movieclips:
    print('clip {0} found\n'.format(clip.name), file=f2)
    width=clip.size[0]
    height=clip.size[1]
    for ob in clip.tracking.objects:
        print('object {0} found\n'.format(ob.name), file=f2)
        for track in ob.tracks:
            print('track {0} found\n'.format(track.name), file=f2)
            fn = output_folder + '{0}_{1}_tr_{2}.csv'.format(clip.name.split('.')[0], ob.name, track.name)
            with open(fn, 'w') as f:
                framenum = 0
                while framenum < clip.frame_duration:
                    markerAtFrame = track.markers.find_frame(framenum)
                    if markerAtFrame and not markerAtFrame.mute:                        
                        coords = markerAtFrame.co.xy
                        if relativeCoords:
                            if printFrameNums:
                                print('{0},{1},{2}'.format(framenum, coords[0], coords[1]), file=f)
                            else:
                                print('{0},{1}'.format(coords[0], coords[1]), file=f)
                        else:
                            if printFrameNums:
                                print('{0},{1},{2}'.format(framenum, coords[0]*width, coords[1]*height), file=f)
                            else:
                                print('{0},{1}'.format(coords[0]*width, coords[1]*height), file=f)

                    framenum += 1
f2.close()
```
