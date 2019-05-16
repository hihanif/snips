# Build for billions

# Network efficiency
## Optimize images 
  * be specific about the device type such as smartphone/tablet, size and resolution
  * be specific about the current n/w also. if wifi, do all but 2G be slow, 4G do all and so on
  * use placeholder images for bette UX. Prefer webP instead of PNG/JPG which are space efficient
  * use image loading libraries that takes care of disk caching to reduce the network usage
## Optimize networking
  * Provide offline access using Room, batch n/w req instead of freq bring up of radio
  * Use workmanager instead of plling like wating for wifi, battery connected
## Fine-tune data transfer
  * Download only the most relevant content such as text instead of html for Emails. 
  * use less BW on slow connections
  * Prefetch accordingly. news app fetches 3 articles in 2G but 20 in wifi
  
# Device capability for billions
## Support multiple screen sizes
  * use dp for consistent UI instead of pixels (160dpi == mdpi is the benchmark. so, here 1pixel = 1dp)
  * test your text/layout in low/mdpi devices since these are common in low prices devices
## Provide backward compatibilty
  * target the latest, use 14 for minsdk
  * use jetpack/andoidx compat libs
## Use memory efficiently
  * andriod dynamic features
  * lesser apk sizes and fetures
  * isLowRamDevice() and control your features
  * Use memory profiler
  
# Reduced data cost for billions
## Reduce app size
  * reduces n/w data & internal storage
  * reduce UI assets - prefer SVG, webP, multiple APK support to avoid unused high density images
  * Proguard - removes unused res, 
  * uses cache dir for temp data so that system can clean if low memory

# Battery consumption for billions
  * batt benchmarking, wakelock usage, sensor usage, tasks scheduling/polling
  
  * GPS sensor - prefer Google's gms's FusedLocationProviderClient interface
  * no bg ops in battery power
  * wakelocks - say no no
  * batch n/w activity - to reduce freq bring up of radio
  * Use workmanager
  * Batterystats & battery historian tools

# UI and content for billions
  * keep quick launch, keep UI thread idle AMAP to increase UI responsiveness
## Target 60 frames per second on low-cost devices
  * no overdraw i.e. a button over an image. rather let the image be clickable :)
  * 16 ms refresh rate. Use profiler to find the time consuming tasks
  * Better layout & View hierarchy - Use constraint layout
  * Use launch screens with your brand image to fool the user while loading the content


# Few talk points:

![radio power](https://developer.android.com/images/efficient-downloads/mobile_radio_state_machine.png)

