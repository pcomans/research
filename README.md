## Research

[Wiki](https://github.com/eonist/research/wiki/)


## TaskMenuMarkDownApp: 
<img width="398" alt="img" src="https://dl.dropboxusercontent.com/u/2559476/TaskMenuMarkDownApp_demo.mov.gif">

**The above app was made with:**
1. the regular expression class in the swift-utils repo
2. Utilizing the NSStatusBar class to create the TaskMenu 
3. Hacking the CGEventPost class to imitate keyboard copy and paste (This is the same way TextExpander do it, You could make TextExpander with this technique ) You also have to call `pauseForAMoment()` between the copy and paste operations or else copy wont finish before pasting. This is the same delay scheme TextExpander uses.

[vimeo](https://vimeo.com/156949321) 