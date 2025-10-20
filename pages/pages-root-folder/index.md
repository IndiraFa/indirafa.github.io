---
#
# Use the widgets beneath and the content will be
# inserted automagically in the webpage. To make
# this work, you have to use › layout: frontpage
#
layout: frontpage
header:
  image_fullwidth: header_unsplash_5.jpg
  title: "Indira Fabre, PhD.<br><small>AI & Data – Healthcare – Strategy consulting</small>"
  text: Welcome !
widget1:
  title: '🧠 AI & ML Projects'
  url: 'http://indirafa.github.io/ai-ml/'
  image: med_img.png
  text: This section showcases my work at the intersection of AI and real-world impact on medical imaging, satelite imagery and natural language processing.
widget2:
  title: '🔧 Data Engineering & MlOps'
  url: 'http://indirafa.github.io/data-engineering/'
  image: datalake.png
  text: Here you'll find projects focused on building scalable data infrastructure—ranging from cloud-native data lakes to Retrieval-Augmented Generation (RAG) systems. I also integrated MLOps practices to streamline model deployment, monitoring, and reproducibility.
widget3:
  title: '🌱 Early works'
  permalink: /early-works/
  url: 'http://indirafa.github.io/early-works/'
  #image: widget-github-303x182.jpg
  text: A look back at my formative work during my PhD and earlier explorations. While not directly aligned with my current focus, these projects laid the groundwork for my analytical thinking, research rigor, and problem-solving approach.
  video: '<a href="#" data-reveal-id="videoModalEarlyWorks"><img src="/assets/img/phd.png" width="302" height="182" alt=""/></a>'


#
# Use the call for action to show a button on the frontpage
#
# To make internal links, just use a permalink like this
# url: /getting-started/
#
# To style the button in different colors, use no value
# to use the main color or success, alert or secondary.
# To change colors see sass/_01_settings_colors.scss
#
callforaction:
  url: 'http://indirafa.github.io/about-me/' # to be changed
  text: More about me ›
  style: alert
#permalink: /about-me/
permalink: /index.html
#
# This is a nasty hack to make the navigation highlight
# this page as active in the topbar navigation
#
homepage: true
---
<!-- 
<div id="videoModal" class="reveal-modal large" data-reveal="">
  <div class="flex-video widescreen vimeo" style="display: block;">
    <iframe width="1280" height="720" src="https://www.youtube.com/embed/3b5zCFSmVvU" frameborder="0" allowfullscreen></iframe>
  </div>
  <a class="close-reveal-modal">&#215;</a>
</div> -->
