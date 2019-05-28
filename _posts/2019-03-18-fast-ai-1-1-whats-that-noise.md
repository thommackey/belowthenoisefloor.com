---
title: "fast.ai 1.1: What's that noise?"
classes: wide
excerpt: "A first foray into sound classification using the fast.ai library."
header:
  og_image: assets/posts/2019-03-18-fast-ai-1-1-whats-that-noise/og-image.png
---
**BLUF:** I got a ~79% success rate using resnet50 to classify different vehicle sounds from [the BBC sound effect archive](http://bbcsfx.acropolis.org.uk/) based on week 1 of [the fast.ai deep learning course](https://course.fast.ai/).

I’ve started the fast.ai deep learning course. Homework for the first week is to try building a classifier of images for yourself, based on the notebook & steps provided in the lecture. In the course lecture, Jeremy mentioned that people used CNNs to do sound classification by converting the sounds to spectrographs and classifying the resulting images. At a high level, this process is going to teach the computer to tell the difference between what car sounds “look like”, and what boat sounds “look like”. Sounds easy enough — let’s give it a crack. I thought I’d try to classify sounds from the [BBC sound effects archive](http://bbcsfx.acropolis.org.uk/).

(Hat tip to [Jeremy Singer-Vine](https://twitter.com/jsvine)’s [Data is Plural](https://tinyletter.com/data-is-plural) newsletter — I’ve been subscribed for years, and while I don’t read it religiously, it’s incredibly useful to have hundreds of interesting, structured, publicly-available datasets just a gmail search away.)

Preparing the data was [simple but not easy](https://www.infoq.com/presentations/Simple-Made-Easy). While the [project homepage](http://bbcsfx.acropolis.org.uk/) provides an interactive way to search for and listen to the sounds, which lets you discover gems such as “Half a dozen werewolves” and “Execution being carried out with a block and sword” (which is distinct from “Execution being carried out with a block and large axe”), I had to bulk download the WAVs, categorise them, and convert them into images. I ended up with a bunch of spectrogram images of WAV recordings of various vehicles or vehicle-related things. The images we’ll be classifying look like this:

{% include image.html name="two-stroke.png" caption="I humbly present “Two-stroke petrol engine driving small elevator, start, run, stop.”" %}

Here’s a separate post where I go into painful detail about what I went through to get those images. Here, I’ll just talk about what I needed to change in the week 1 workbook to use the data once I’d prepared it and added it to my google drive. [Here’s the notebook](https://gist.github.com/thommackey/91401344eb0755c93694f9ef870c57a4) I ended up with.

I worked out how to read my google drive files from colab and create a fastai ImageDataBunch from my directory structure which looked like `data/Cars/07076115.wav.png`, `data/Trains/07073234.wav.png`, etc. That was pretty easy, I did it this way:

```python
import os.path
root_dir = "/content/gdrive/My Drive/"
root_path = Path(root_dir)
data_path = root_path/"data"
data = ImageDataBunch.from_folder(data_path,
                                train=".",
                                valid_pct=0.2,
                                ds_tfms=get_transforms(),
                                size=224,
                                bs=bs).normalize(imagenet_stats)
```

Then I simply ran the rest of the week 1 workbook as it was presented in the lecture. First of all, it worked, so that’s pretty good for a first try! Results weren’t astonishing, though. I got pretty poor results from the lesson 1 resnet34 defaults: ~42% error rate. Pretty ugly and only slightly better than chance. Still, it’s promising; it seems the spectrograms look more distinct to the neural net than they do to me! Picking a loss rate resulted in ~35% error; still pretty awful, but an improvement.

Then I forged *extremely slightly* ahead and trained it on resnet50 with larger images (512px) and a custom loss rate. This resulted in a~21% error rate, which while certainly an improvement, still isn’t anything to write home about…

…Or so I thought until I started the week 2 lecture! It turns out that a previous student had also [chosen to do audio classification](https://forums.fast.ai/t/share-your-work-here/27676/143) in the same way, and had achieved 20% error rate. And Jeremy was hyped about this, because that beats the previously published state of the art result — of ~21%! So suddenly I don’t feel so bad “only” getting the previous known world record on my first try :)

I’m not sure whether this project has any legs or whether I’m going to pursue it further. I am interested in the idea of ambient sound analysis as it seems like a natural fit for disconnected, low power IoT sensors as monitors, and identifying imprecise ambient sound is a problem that’s very difficult to solve with hard-coded rule-based heuristics. Of course I don’t yet know what I’m detecting… I suppose you could listen to planes near an airport, or frogs in a particular habitat. Actually, that could be a useful project — train a neural net to recognise different frog calls, leave a low-power device in the study area to log which frog calls it hears over some time, then retrieve the device & find out what it’s heard… hmm. I’m pretty sure I’ve heard a podcast about that, I wonder if I can look them up…

Now on to week 2!