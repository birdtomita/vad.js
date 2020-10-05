# Voice activity detection in Javascript


__vad.js__ is a small Javascript library for voice activity detection.

### Quick Start

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>VAD Test</title>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
</head>
<body>
  <button onclick="startrec()">start</button>
<script type="text/javascript" src="lib/vad.js"></script>
<script type="text/javascript">
function startrec() {
  // Create AudioContext
  window.AudioContext = window.AudioContext || window.webkitAudioContext;
  var audioContext = new AudioContext();

  // Define function called by getUserMedia 
  function startUserMedia(stream) {
    // Create MediaStreamAudioSourceNode
    var source = audioContext.createMediaStreamSource(stream);

    // Setup options
    var options = {
     source: source,
     voice_stop: function() {console.log('voice_stop');}, 
     voice_start: function() {console.log('voice_start');}
    }; 
    
    // Create VAD
    var vad = new VAD(options);
  }


  navigator.mediaDevices = navigator.mediaDevices || ((navigator.mozGetUserMedia || navigator.webkitGetUserMedia) ? {
   getUserMedia: function(c) {
     return new Promise(function(y, n) {
       (navigator.mozGetUserMedia ||
        navigator.webkitGetUserMedia).call(navigator, c, y, n);
     });
   }
} : null);

if (!navigator.mediaDevices) {
  console.log("getUserMedia() not supported.");
  return;
}

// Prefer camera resolution nearest to 1280x720.

var constraints = { audio: true, video: false };
navigator.mediaDevices.getUserMedia(constraints)
.then(function(stream) {
  startUserMedia(stream);
})
.catch(function(err) {
  console.log(err.name + ": " + err.message);
});
}
</script>
</body>
</html>
```


### Tested - Browser
* Firefox 45.0a1+

##Author

* Kelly Davis kdavis@mozilla.com
* Mark Panaghiston https://github.com/thepag


##Thanks
The code is based on the following implementations: 

+ https://github.com/happyworm/Playful-Demos

##Contribution

Any contribution will be welcome!

