---
layout: post
title: "Video-Generation-from_Simulation-Pictures-and-Manipulate-Videos-Through-FFmpeg"
categories_short_name: paraview_Insitu
meta: "paraview Insitu"
type: "Draft"
---

starts from number 100 *png with input framerate 10, to combine into *mp4 video
{% highlight console %}
~$ ffmpeg -framerate 10 -start_number 0100 -i gapA-clip_t-%4d.png -c:v libx264 -r 10 -pix_fmt yuv420p tau50-tQ10.86-ARelexing493-100frame.mp4
{% endhighlight %}

combine two *mp4 side-by-side into new *mp4 video 
{% highlight console %}
~$ ffmpeg -i tau50-tQ10.86-ARelexing1000-100frame.mp4 -i tau50-tQ10.86-ARelexing320-100frame.mp4 -filter_complex "hstack,format=yuv420p" -c:v libx264 -crf 18 tauQ50-tQ1-0.86-ARelx1000-320-100frame.mp4
{% endhighlight %}