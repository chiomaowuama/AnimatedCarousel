<script setup>
  import { ref, onMounted } from 'vue';

  let raduis = ref(240);
  let autoRotate = ref(true);
  let rotateSpead = ref(-60);
  let imgwidth = ref(120)
  let imgheight = ref(170);
  let dragcontainer =ref('');
  let spincontainer = ref('');
  let ground =ref('')
  let videoUrl = ref("https://player.vimeo.com/external/479728625.sd.mp4?s=f4f886d3d45a0312d8d47419647788178535a2c6&profile_id=165&oauth2_token_id=57447761")
  let videosHolder = ref([]);
  const images = ref([
    "https://images.unsplash.com/photo-1440589473619-3cde28941638?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8Y29vcnBlcmF0ZSUyMHBlb3BsZXxlbnwwfHwwfHx8MA%3D%3D",
    "https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8M3x8Y29vcnBlcmF0ZSUyMHBlb3BsZXxlbnwwfHwwfHx8MA%3D%3D",
    "https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8OHx8Y29vcnBlcmF0ZSUyMHBlb3BsZXxlbnwwfHwwfHx8MA%3D%3D",
    "https://images.unsplash.com/photo-1488426862026-3ee34a7d66df?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTl8fGNvb3JwZXJhdGUlMjBwZW9wbGV8ZW58MHx8MHx8fDA%3D",
    "https://images.unsplash.com/photo-1526413232644-8a40f03cc03b?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTB8fGNvb3JwZXJhdGUlMjBwZW9wbGV8ZW58MHx8MHx8fDA%3D",
    " https://images.unsplash.com/photo-1488426862026-3ee34a7d66df?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTl8fGNvb3JwZXJhdGUlMjBwZW9wbGV8ZW58MHx8MHx8fDA%3D",
    // ... add other image URLs
  ]);
  let alimg = ref([])
  let aEle = ref([]) //this is the thing that isnt working 
 

  function init(delaytime){
    // console.log("hello")
    for(let i = 0; i < aEle.value.length;i++){
      aEle.value.forEach((element, i) => {
        element.style.transform = `rotateY(${i * (360 / aEle.value.length)}deg) translateZ(${raduis.value}px)`;
        element.style.transition = `transform 1s`;
        element.style.transitionDelay = delaytime || (aEle.value.length - i) / 4 + 's';
    
      });

    }
  }
  function applyTranform(obj){
    tY = Math.max(Math.min(tY, 180), -180);
    obj.style.transform = `rotateX(${-tY}deg) rotateY(${tX}deg)`;
  }

  function playSpin(yes){
    spincontainer.value.style.animationPlayState = (yes?'running': 'paused ' )
    // console.log(spincontainer.value);
  }
  let sX, sY, nX, nY, desX = 0,
  desY = 0,
  tX = 0,
  tY = 10;

 
  setTimeout(() => {
    init(1000)
    
  }, 1000);
  
  onMounted(() => {
    
    aEle.value = [ ...alimg.value, videosHolder.value] //this is the thing that isnt working 
    
    if(spincontainer.value ){
      spincontainer.value.style.width = `${imgwidth.value}px`;
      spincontainer.value.style.height = `${imgheight.value}px`;
    }
    if( ground.value ){
      ground.value.style.width = `${raduis.value * 3}px`;
      ground.value.style.height = `${raduis.value * 3}px`;
      // console.log(  ground.value.style.width);
    }
    if(autoRotate){
      const animationName = rotateSpead > 0 ? 'spin' : 'spinRevert';
      if (spincontainer.value) {
        spincontainer.value.style.animation = `${animationName} ${Math.abs(rotateSpead.value)}s infinite linear`;
        console.log(rotateSpead.value);
      }
    }
    
  })
 
</script>

<template>
  <main>

    <div id="drag-container"  ref="dragcontainer">
      <div id="spin-container" ref="spincontainer">
        <!-- images -->
        <img  
        v-for="(image, index) in images"
        :key="index" alt="Unsplash Image"
        :src="image" ref="alimg" >
        
        
        <video controls autoplay="autoplay" loop ref="videosHolder">
          <source :src="videoUrl" type="video/mp4">
        </video>
        <!-- text at the center of the ground -->
        <p>3D carousel</p>
      </div>
      <div id="ground" ref="ground"></div>
    </div>
  </main>
</template>
