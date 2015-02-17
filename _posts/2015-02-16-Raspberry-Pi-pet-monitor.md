---
layout: post
title: Second Test post
---

I'll start by admitting that this is definitely one of those cases in which
automating a task is much more work than just doing it. Katie and I have been
using an automatic pet feeder, _Le Bistro_, for a little while now. Despite
Serena being able to get her little paws up into the dispenser to steal food,
it seems to do a good job dispensing food when we tell it to. She must have a
love-hate relationship with it; Katie and I always joke about the reviews it
must be recieving on cat-yelp:

{% include image.html url="images/serena_yelp_review.jpg" description="Four stars is probably being overly generous." %}

Despite the fairly dependable track record of _Le Bistro_ when I'm watching,
Serena swears it fails everytime I'm not around. And, since I'm usually not
watching (or asleep) when it goes off, I thought it would be useful to have
some sort of independent verification that the food is in fact dispensing.

Enter the raspberry pi, which I recently picked up with a camera attachment.
There are many projects using the raspberry pi as a security camera, so I
figured that my application (recording at two specific times of day) would
actually be fairly easy.

The steps, as I envisioned them, include:

1. Synchronizing the raspberri pi and _Le Bistro_
2. Recording a video starting just a few moments before the food dispenses, lasting around a minute.
3. Uploading the video to some sort of online storage.

The last step is actually one I spent most of my time on, since I envisioned
this as working as a headless camera. That, and because the internet at my apartment is terrible.

To accomplish these I use a bash script activated via a cron job. Here's the main recording script:

{% highlight bash %}
#!/bin/bash

# Change to the correct directory
cd /home/pi/Serena/

# Remove the video from the previous meal, so the storage doesn't get
# filled with cat videos
rm recording*

# Get the current date and time
DATE=`date +%Y-%m-%d`
HOUR=`date +%H`

# Depending on whether its AM or PM, I assign a variable with the
# correct meal name
if [[ $HOUR > 12 ]]; then
	MEAL="Dinner"
else
	MEAL="Breakfast"
fi

# Record a one-minute video using the camera
raspivid -o recording.h264 -t 60000 -fps 30 -w 640 -h 480 -vf

# Re-encode the video as an mp4 file
MP4Box -fps 30 -add recording.h264 recording.mp4

DESC="An automatic recording to make sure Serena got her $MEAL"

# Upload the video to youtube
youtube-upload --title="$MEAL $DATE" --description=$DESC --location="=" \
	--tags="raspberry pi, cat" --client-secrets=client_secret.json \
	recording.mp4
{% endhighlight %}


The `youtube-upload` command I use is
[here](https://github.com/Miserlou/Youtube-Upload), which makes the process
relatively easy. Due to the pretty terrible internet speeds we have, I record
the video at a fairly low resolution. The raspberry pi camera is capable of a
crazy 1080p video, but the post-processing and uploading of a file that size
took too long.

Once the raspberry pi was all set up, crontabs at the ready, I mounted it inside a cardboard box and positioned it next door to _Le Bistro_.

{% include image.html url="images/IMG_1639.jpg" description="Stealthy" %}

Here's a still image from the point of view of the camera:

{% include image.html url="images/serena_setup_small.jpg" description="Ignore my feet" %}

Could use a bit of color-balancing. And now we wait!
