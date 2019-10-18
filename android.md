### Activity lifecycle related
- isFinishing() - called in onPause to figure out if it is really dying
- onDestroy() - not guaranteed to be called always. so save data in onPause()
- onStop() - same as above. no longer visible.
- onPause - partially visible
- onAttachFragment() - called in activity when a fragment is attached to it
- onRestart90 - called before onStart()
- onNewIntent() when an existing activity is recreated

### Fragments related
- Fragments - setArguments/getArguments as Bundle. only default constructor is preferred since it will be used during activity recreation
- onCreateOptionsMenu() created if setHasOptionsMenu(true) is invoked
- retainInstance() - onCreate/onDestory is called only once. rather only onAttach/onDetach is called
- Fragments dont have onRestoreInstanceState() - rather the bundle stored in onSaveInstanceState is sent back in onCreate, onCreateView, onActivityCreated.

###

- singleTop - does not clear the activities unless CLEAR_TOP is mentioned
- ContentProvider - data security & api abstration & IPC across process
- android:id - needed to restore the view state on activity recreation
- View optimization - Flatten view hierarchy using constraint layout to reduce the view lookup time during the initial onMeasure/onDraw flow, look for view overdraw,  
- Customview - attrs, onDraw() to draw the view, onMeasure()/onSizeChanged(), use Paint object
- 9 patch - stretchable bitmap image, used as a background for view to support multiple device configuration. it has 9 sections - 4 corners, 4 sides and 1 center. .9.png
- shareUid - appl manifest, same signature
- PackageManager.hasSystemFeature - to check for any features like sensors, also <uses-feature> in the manifest as a strict way to checking before installing the appl
- Handlers - to pass data between threads
- Sensors - SensorMgr, Sensor, SensorEvent, SensorEventListener

